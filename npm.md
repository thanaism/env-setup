# npm の使い方

## バージョンの確認

```bash
$ npm -v
6.14.8

$ npm version
{
  npm: '6.14.8',
  ares: '1.16.0',
  brotli: '1.0.7',
  cldr: '37.0',
  icu: '67.1',
  llhttp: '2.0.4',
  modules: '83',
  napi: '6',
  nghttp2: '1.41.0',
  node: '14.8.0',
  openssl: '1.1.1g',
  tz: '2019c',
  unicode: '13.0',
  uv: '1.38.1',
  v8: '8.4.371.19-node.12',
  zlib: '1.2.11'
}
```

## ヘルプ

```bash
$ npm help {command}
$ npm help init
```

## パッケージのインストール

```bash
# ローカルへのインストール
$ npm install {package-name}

# グローバルへのインストール
$ npm install --global {package-name}

# グローバルのディレクトリ
$ npm root -g
```

## インストールされたパッケージの確認

```bash
$ npm list
/Users/thanai/git
└── (empty)

$ npm list -g --depth=0
/usr/local/Cellar/node/14.8.0/lib
└── (empty)
```

## package.json

> npm は依存関係を解決してパッケージをインストールするだけではなく、必要に応じてバージョン管理や本番環境（Production）用、開発環境（Development）用と言ったパッケージ管理が可能です。
>
> このため、パッケージをインストールするにはパッケージの情報が記載された package.json というファイルを作成します。
>
> package.json にはそのプロジェクトでインストールしたパッケージの情報が追加され、インストールした際のオプション（本番環境用、開発環境用、プロジェクト固有など）も追加され更新されます。
>
> また、パッケージをインストールすると package.json の他に、package-lock.json というファイルが自動的に生成され、このファイルを使ってパッケージのバージョンを固定することなどができます。
>
> package.json や package-lock.json を利用することでプロジェクトのパッケージを管理をすることができ、また簡単に同じパッケージ環境のコピーなどができるようになっています。

### package.json の作成

```bash
$ cd {project-directory}
$ npm init
```

### デフォルト値での作成

`$ npm init`コマンドは対話式で面倒のため、全部デフォルトで初期化する方法もある。

```bash
# 対話的に入力する必要がない
$ npm init -y
```

## バージョンを指定してのインストール

```bash
$ npm install {package-name}@{version}
$ npm install sass@1.22.12
# 範囲指定も可能
$ npm install sax@">=0.1.0 <0.2.0"
```

## その他のインストールオプション

| option     | -    | 内容                      |
| ---------- | ---- | ------------------------- |
| --save-dev | -D   | 開発環境用                |
| --no-save  | なし | package.json に記載しない |

##　パッケージの更新

### 更新要否の確認

```bash
$ npm outdated
Package  Current  Wanted  Latest  Location
sass     1.22.12  1.26.5  1.26.5  myProject
```

### 特定パッケージの更新

```bash
$ npm update {package-name}
# ただし、これはメジャーアップデートは実行しない
# ex) 1.x.x系から2.x.x系には自動で移行しない
```

## パッケージのアンインストール

```bash
$ npm uninstall {package-name}
$ npm un {package-name}
```

## npx

```bash
$ npm init -y
$ npm install cowsay
$ npx cowsay 'hello'
 _______
< hello >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

# npxを使用しないで同じことをする
$ ./node_modules/.bin/cowsay 'hello'
 _______
< hello >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

$ $(npm bin)/cowsay 'hello'
 _______
< hello >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

$ npm bin
/Users/thanai/git/test/node_modules/.bin
```

インストールしている場合はそのバイナリが起動されるが、そうでない場合は、グローバルにインストールされ、コマンド実行後にアンインストールされる。

```bash
$ npx create-react-app my-react-app
# 初回生成時にしか使わないものなどに効果的
```
