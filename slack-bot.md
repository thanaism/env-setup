# Django で SlackBot をつくる

## 環境構築

### 仮想環境と各種インストール

```bash
$ mkdir slack-bot
$ cd slack-bot
$ git init
$ gh repo create
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip list
Package    Version
---------- -------
pip        19.2.3
setuptools 41.2.0
WARNING: You are using pip version 19.2.3, however version 20.2.3 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
$ pip install --upgrade pip
$ pip install --upgrade setuptools
$ pip install black flake8
$ pip install django django_rest_framework slackclient requests
```

### Django プロジェクトの作成・起動

```bash
$ django-admin startproject config .
$ python manage.py maigrate
$ python manage.py runserver
```

ここまででいったん Django が localhost でちゃんと起動しているか確かめる。

## Django と Slack の接続

### app の追加

Slack につなげる app を Django に追加する。

```bash
$ python manage.py startapp slack_app
```

Django に追加したことを知らせる。

```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'slack_app.apps.SlackAppConfig',
]
```

### ルーティング

`/slack`へのアクセスを`slack_app.urls`につなぐ。
`include`をインポートするのを忘れない。

```py
# config/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('slack/', include('slack_app.urls'))
]
```

`slack_app.urls`側では`index.html`および`vews.py`での処理につなぐ。

```py
# slack_app/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('oauth/', views.oauth, name='slack_oauth')
]
```

### index.html を書く

Django では`templates`内に html を記述していく。

```html
<!-- slack_app/templates/slack/index.html -->
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <h1>Add bot to your workspace</h1>
    <a
      href="https://slack.com/oauth/authorize?scope=bot&client_id={{ client_id }}"
    >
      Add bot
    </a>
  </body>
</html>
```

OAuth はページの表示ではなく`views.py`で REST の処理を行う。

```py
# slack_app/views.py
from django.shortcuts import render
from django.http import HttpRequest, HttpResponse
import requests
import json

from config.settings import SLACK_CLIENT_ID, SLACK_CLIENT_SECRET


def index(request: HttpRequest) -> HttpResponse:
    context = {
        "client_id": SLACK_CLIENT_ID
    }
    return render(request, 'slack/index.html', context)


def oauth(request: HttpRequest) -> HttpResponse:
    """
    == Send Request ==
    requests.get(url, {
        "code": "xxx",
        "client_id": "xxx",
        "client_secret": "xxx"
    })

    == Response ==
    {
        'ok': True,
        'access_token':
        'xoxp-xxx',
        'scope': 'xxx, yyy',
        'user_id': 'xxx',
        'team_id': 'xxx',
        'enterprise_id': None,
        'team_name': 'xxx',
        'bot': {
            'bot_user_id': 'xxx',
            'bot_access_token': 'xxx'
        }
    }
    """
    response = json.loads(
        requests.get('https://slack.com/api/oauth.access', params={
            "code": request.GET.get('code'),
            "client_id": SLACK_CLIENT_ID,
            "client_secret": SLACK_CLIENT_SECRET
        }).text
    )

    if response['ok']:
        return HttpResponse('Your bot joined your workspace!')
    else:
        return HttpResponse('Failed! Please retry.')
```

ここで、上記の REST 処理に必要な API キーを`config/setting.py`に入れたいので、
Slack App のページで情報を取得する。

## Slack 側の設定

### Create New Slack App

1. <https://api.slack.com/apps> に行く
2. `Create New Slack App`して得られた情報を settings.py に書く

```py
# config/settings.py
...
SLACK_CLIENT_ID = "アクセスしたページからコピペする"
SLACK_CLIENT_SECRET = "アクセスしたページからコピペする"
SLACK_VERIFICATION_TOKEN = "アクセスしたページからコピペする"
...
```

※ローカルでお試しするだけならこれでよいが、GitHub にあげるなら環境変数にしてしまうのがベストプラクティス？

```py
import os
SLACK_CLIENT_ID = os.environ['SLACK_CLIENT_ID']
```

一応、どっかにデプロイすることも考えて上記のようにはした。

```bash
# .envファイルをどっかにつくる
export SLACK_CLIENT_ID="わしのID"
```

```bash
$ source .env
$ echo $SLACK_CLIENT_ID
"わしのID"
```

### Redirect URL の設定（なんのためのものかよく分かっておらず）

同じページの横のカラムに`OAuth & Premissions`のタブがある。
`http://localhost:8000/slack/oauth/`を`Redirect URLs`に追加する。

### bot への権限追加

`App Home`タブから`Review scoped to add`=>`Scopes`=>`bot token scopes`と進み, 適当にスコープを追加
`app_mentions:read`にしたが、~~どれを使うべきなのかよくわかっていない~~
=> `chat:write`をここでは選択しておけば OK

### bot の追加

```bash
python migrate.py runserver
```

`localhost:8000/slack`にアクセスすると先ほど書いた html が表示されるので、
bot を追加する。成功すると`ボットがワークスペースに参加しました！`と出るはず。

## 続きの設定

`OAuth & Permissions`に追加した bot のトークンが追加されているので、
`settings.py`に追加しておく。

```py
# config/settings.py
...
SLACK_BOT_USER_TOKEN = "xoxb-##################"
...
```

追加したら、bot の本番の処理を`views.py`に追記していく。

```py
from django.shortcuts import render
from django.http import HttpRequest, HttpResponse
import requests
import json

from config.settings import SLACK_CLIENT_ID, SLACK_CLIENT_SECRET

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from slack import WebClient
from slack.errors import SlackApiError

from config.settings import SLACK_BOT_USER_TOKEN, SLACK_VERIFICATION_TOKEN

client = WebClient(token=SLACK_BOT_USER_TOKEN)


class Events(APIView):
    def post(self, request: HttpRequest, *args, **kwargs) -> HttpResponse:
        print(request.data)
        user = request.data["event"].get("user")
        print(f"user: {user}")
        # トークン認証
        if request.data.get("token") != SLACK_VERIFICATION_TOKEN:
            return Response(status=status.HTTP_403_FORBIDDEN)

        # Endpoint 認証
        if request.data.get("type") == "url_verification":
            return Response(data=request.data, status=status.HTTP_200_OK)

        # Botのメッセージは除外する
        if request.data["event"].get("bot_id") is not None:
            print("Skipped bot message ...")
            return Response(status=status.HTTP_200_OK)

        # ロジック
        message_info = request.data.get("event")

        channel = message_info.get("channel")
        text = message_info.get("text")

        if "hi" == text:
            try:
                client.chat_postMessage(channel=channel, text="hi")
            except SlackApiError as e:
                print(e)
                return Response("Failed")
            return Response(status=status.HTTP_200_OK)
        return Response(status=status.HTTP_200_OK)


def index(request: HttpRequest) -> HttpResponse:
    ...


def oauth(request: HttpRequest) -> HttpResponse:
    ...
```

いま追加した POST 処理に対するルーティングを追加しておく。

```py
# slack_app/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('oauth/', views.oauth, name='slack_oauth'),
    path('events/', views.Events.as_view(), name='events')
]
```

## デプロイ

localhost に外からアクセスするための URL を付与してくれる ngrok とかいう便利なサービスを使う。
（ngrok ってよく見るけどこういうものだったのか）

```bash
brew cask install ngrok
ngrok http 8000
```

そのままだと host 的に通らないので、`ALLOWED_HOSTS`に追加してあげる。

```py
# config/settings.py
ALLOWED_HOSTS = ['.ngrok.io', 'localhost', '127.0.0.1', '[::1]']
```

ワイルドカード使えるっていうから`'*.ngrok.io'`って書いて通らなくてハマった。

### Event Subscriptions

ngrok を起動してから、
`Request URL`に`https://############.ngrok.io/slack/events/`を追加する。
うまくいくと`Verified`となる。

#### 重要ポイント

`Subscribe to bot events`と`Subscribe to events on behalf of users`の両方で、
`message.channels`と`message.groups`を追加する。
最後に`Save Changes`するのを忘れない。

## 完了

これで`hi`と送ったら`hi`と返すだけの機能が実装できた。
あとはちゃんとサーバーにデプロイするなり、場合分け処理を追加するなどして面白い Bot に仕上げればよい。
