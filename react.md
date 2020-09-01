# React

## React の概要

`仮想DOM`である`jsx`を用いた高速レンダリングを特徴とする js ライブラリ。  
同じようなライブラリやフレームワークに`Vue`や`Angular`などがあるが、今は`React`がメインストリーム。  
ただ、簡素なページにとってはややオーバースペック感はあるので、日本では Vue が優先的に選択されるシーンも多いらしい。  
`jQuery`といった旧式ライブラリもあるが、すでにレガシー化しているらしい。

## SPA

`SPA: Single Page Application`のためのライブラリなので、基本的にページ遷移による DOM の再レンダリングは本末転倒である。しかし、コンポーネントのみの書き換えでは次のような不具合もある。

- URL が変化しないのでブラウザの戻るボタンが効かない
- 特定の状態でブックマークができない

このような問題を解消するための方法として、HTML5 では`History API`という機能が追加されている。

## History API

以下のようなサポートとなる。

```js
// 擬似的にhttps://{domain}/secondに飛んだ状態になる
window.history.pushState(null, null, "/second");
```

## 状態管理の簡略化

いくらサポートされているといっても、この状態管理をオンコードでやっていくのは辛いものがある。  
そこで利用するのが、React Router というライブラリとなる。

### React Router のインストール

ライブラリは`react-router-dom`で行う。  
`react-router`という旧式のものもあるらしいので混同しない。
基本的には dom 付きのほうが上位互換らしい。

```bash
$ npm install react-router-dom
```

`yarn`を使用しているなら下記でインストールする。

```bash
$ yarn add react-router-dom
$ yarn add @types/react-router-dom # typescriptプロジェクトではこちらも必要
```

### 使い方

`App.js`にモジュールをインポートして使用している基本的な例を記載する。

```js
import { BrowserRouter, Route } from "react-router-dom";

// 自作の画面
import HomePage from "./home";
import MyPage1 from "./mypage1";
import MyPage2 from "./mypage2";

const App = () => (
  <BrowserRouter>
    {/* BrowserRouter直下に置けるコンポーネントは1つだけ */}
    <div>
      {/* RouteはBrowserRouter以下ならばどこの階層に置いてもよい */}
      <Route exact path="/" component={HomePage} /> {/*（1）*/}
      <Route path="/mypage/1" component={MyPage1} />
      <Route path="/mypage/2" component={MyPage2} />
    </div>
  </BrowserRouter>
);
```

### 参考

`render()`要素には単一のコンポーネントしか返せないが、余計なネストをしたくない場合は`<React.Fragment>`タグを使用するとよい。

### 注意点

`Route`タグにより指定された URL に応じて表示内容が変化するが、アドレスは前方一致で判断される。  
そのため、設定のないアドレスを使用するとルートに飛んでしまう。これを防ぎたい場合は`exact`を指定する。

### a タグの代替

a タグを使用すると再レンダリングがかかってしまうため、これを内部的に History API に準じた挙動に変換するために、`Link`タグを代替的に使用する。  
具体的には、`<Link to="/target">targetを開く</Link>`というようなイメージとなる。

## 少し踏み込んだ React Router の使い方

### パラメータを受け取る

React Router では URL からパラメータをもらってくる方法が提供されている。

```jsx
<Route
  path="/articles/:articleId" {/* 記事のIDを持つURLに反応させる */}
  component={Article} /> {/* 記事を表示するコンポーネント */}
```

コンポーネント側では、

```jsx
const articleId = this.props.match.params.articleId; // （1）

fetch(`https://example.com/api/articles/${articleId}`).then((article) => {
  this.setState({ article }); // 画面に記事を描画する
});
```

Route の path でコロンを付けたパラメータが props.match.params に入っており、（1）のように取り出せる。  
この仕組みにより。Link コンポーネントで URL にパラメータを指定するだけでリンク先のコンテンツを差し替えられるようになる。

```jsx
<Link to="/articles/001">記事001</Link>
<Link to="/articles/002">記事002</Link>
<Link to="/articles/003">記事003</Link>
```

ID やカテゴリ名などをパラメータとして渡すことで、コンポーネントの共通化が捗る可能性がある。  
設計時に考慮してみるとよい。

### デフォルトで表示するコンポーネントを設定する

パラメータ付きの Route を作成した際には、パラメータがなかった場合のことも考慮する必要がある。

```jsx
<div>
  <Route path="/articles/:articleId" component={Article} />
</div>
```

articleId を渡さなかった場合、つまり/articles としてアクセスした場合には、マッチする path がないと見なされて、何も表示されない。  
ユーザビリティ上あまり好ましい状況ではないので、デフォルトで何らかの記事を表示したい。  
Redirect コンポーネントを使って、次の通りに書き換える。

```jsx
<div>
  <Route path="/articles/:articleId" component={Article} />
  <Redirect to="/articles/001" />
</div>
```

マッチする Route が 1 つもなかった場合に、Redirect の to 属性に設定された URL にリダイレクトされ、少なくとも何かのコンテンツが表示されている状況を作る。

### 動的にリダイレクトを行う

JSX に定義した静的な設定でリダイレクトを仕込んでおく方法は、前項。  
では、動的にリダイレクトを発生させるにはどうしたらよいか。  
Route を使って描画したコンポーネントに渡される props のひとつに、history がある。これは HTML5 の History API を薄くラップしたライブラリ（BrowserRouter と併用した場合の挙動）。  
これを使って、動的にリダイレクトを実現できる。

この history を使ってリダイレクトを行う場合は、history.replace("遷移先のパス")を利用する。  
前項の Route で articleId に該当する記事が存在する場合には記事データの取得を行い、存在しない場合には"/articles/001"にリダイレクトする処理は、以下のように書く。

```jsx
const articleId = this.props.match.params.articleId;

if (articleExists(articleId)) {
  // そのIDに該当する記事の存在を確認する
  fetch(`https://example.com/api/articles/${articleId}`).then((article) => {
    this.setState({ article }); // 画面に記事を描画する
  });
} else {
  this.props.history.replace("/articles/001"); // （1）
}
```

（1）で指定したパスに BrowserRouter や Route が反応することで、画面が切り替わる。

### 動的に画面遷移を行う

前項と同様、Link を使わず画面遷移を行う方法がある。  
Link の to 要素に指定していた遷移先のパスを、history.push("遷移先のパス")という形で実行すると、画面遷移が発生する。

### Route が 1 つしか表示されないことを保証する

前述の通り、Route は現在の URL が path にマッチした場合だけ表示され、マッチしなかった場合には表示されない。  
これは大原則である一方、裏を返せば「マッチした Route はすべて表示される」ということでもある。  
リスト 1 を元にして、exact 要素を書かなかった場合の例が以下。

```jsx
<div>
  <Route path="/mypage/1" component={MyPage1} />
  <Route path="/mypage/2" component={MyPage2} />
  <Route path="/" component={HomePage} />
</div>
```

この場合、”/mypage/1”では MyPage1 と HomePage が表示され、”/mypage/2”では MyPage2 と HomePage が表示されてしまう。これは意図せぬ動作となる。

このケースであれば exact を使う形でも解決可能だが、パスの構造が複雑になってくると、exact だけでは対処できないケースも出てくる可能性がある。  
こうした「ある階層に表示するコンポーネントを排他的に必ず 1 つにしたい」という要件を満たすアプローチとして、Switch コンポーネントが使用できる。

Switch は、children となった Route や Redirect のうち、マッチしたはじめの 1 つしか表示しない特性を持ったコンポーネント。次の通り、Route の上位に置くことで動作する。

```jsx
<Switch>
  <Route path="/mypage/1" component={MyPage1} />
  <Route path="/mypage/2" component={MyPage2} />
  <Route path="/" component={HomePage} />
</Switch>
```

この場合、Switch の特性により、次のように動作する。

```html
// http://example.com/ のとき
<div>
  <HomePage />
</div>

// http://example.com/mypage/1 のとき
<div>
  <MyPage1 />
</div>

// http://example.com/mypage/2 のとき
<div>
  <MyPage2 />
</div>
```

意図した動作となった。  
画面全体の差し替えや、固定のレイアウトの中で左ペインや右ペインの中身だけを切り替えていくような挙動をさせたい場合には、Switch を積極的にするとよさそうだ。

### 注意：URL を直接打ち込んだときの対応

React Router を使うことで、index.html を表示したまま、あたかも画面遷移をしたかのように URL を変えることができるようになった。

しかし、一点だけ気を付けたほうがいいことがある。  
ユーザーが「`https://example.com/articles/001`」といった URL をブラウザに直接入力した場合、Apache などの Web サーバーは「`https://example.com/articles/001/index.html`」を返却しようとするケースが多い。  
しかし、実際には「`https://example.com/index.html`」しか用意されていないため、`404 NotFound` になってしまう。

これを防ぐためには、サーバーサイドのルーティングを設定して、「`https://example.com/`」以下のすべての階層へのアクセスに対し、「`https://example.com/index.html`」と同じファイルを返すようにするなどの対策を行う必要がある。

create-react-app の開発サーバーは元々こうした設定になっているため見落としがちだが、本番運用の段階でしばしば落とし穴になる部分なので、気を付ける。
