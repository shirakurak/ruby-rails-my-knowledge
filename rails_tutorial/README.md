# Ruby on Rails チュートリアル

## 第3章 ほぼ静的なページ

- 静的なページの作成から始める
  - 最終的にはマイクロポスト、ログイン/ログアウトの認証などの機能を持つ
  - Rails自体は、DBと連携して動的なWebサイトを開発するよう設計

### 3.1 セットアップ

- `hello`アクションとルーティングの追加

### 3.2 静的ページ

- アクションやビューを用いた静的なHTMLページを作る

#### 3.2.1 静的なページの作成

- `generate`スクリプトで、コントローラを生成する
- コントローラ名はキャメルケースで指定し、スネークケースのファイルが生成される
- `rails destroy`で元に戻せる
- マイグレーションの変更（`rails db:migrate`）も元に戻せる（`rails db:rollback`, `rails db:migrate VERSION=0`)
- `config/routes.rb`に生成されたアクションに対するルールが追加される：`get 'static_pages/home'`

`get 'static_pages/home'`について

- `/static_pages/home`というURLに対するGETリクエストを、`StaticPages`コントローラの`home`アクションに結びつけている
- `http://localhost:3000/static_pages/home`にアクセスして確認

`static_pages_controller.rb`について

- `StaticPagesController`は、`ApplicationController`を継承している
- `home`アクションは空→対応するビュー（`home.html.erb`）が表示されるだけ

#### 3.2.2 静的なページの調整

### 3.3 テストから始める

テストを行う目的

- 回帰バグ（以前のバグの再発、機能の追加・変更によって生じる副作用）の防止
- 安全にリファクタリングができる
- アプリケーションの設計やインタフェースを決めるときに役立つ
  - テストコードは、アプリケーションコードから見ればクライアントとして動作する

#### 3.3.1 最初のテスト

#### 3.3.2 Red

#### 3.3.3 Green

#### 3.3.4 Refactor

※3.3は省略した

### 3.4 少しだけ動的なページ

- ページごとに、`<title>`タグを変更する

#### 3.4.1 タイトルをテストする (Red)

#### 3.4.2 タイトルを追加する (Green)

#### 3.4.3 レイアウトと埋め込みRuby (Refactor)

- 埋め込みRubyをビューに使うことで、重複を取り除く
  - `provide`メソッド
- レイアウトファイルは、`application.html.erb`

```erb
...
<body>
  <%= yield %>
</body>
...
```

- 上記は、各ページの内容をレイアウトに挿入するためのもの
- ERBの`csrf_meta_tags`をページ内で展開するための、`<%= csrf_meta_tags %>`もある

#### 3.4.4 ルーティングの設定

### 3.5 最後に

### 3.6 高度なセットアップ

#### 3.6.1 minitest reporters

#### 3.6.2 Guardによるテスト自動化

※3.6は略

## 第4章　Rails風味のRuby

---

## 気になったこと

### 1. bundle installとbundle updateの違い

[bundle install と bundle updateの違いについて](https://qiita.com/lasershow/items/1a048d03ddaaba98171e)

- Bundler
  - gemを管理するためのgem
  - 依存関係やバージョン管理など
- Gemfile
  - 自分のアプリケーションに必要なgemを記載
  - gemをインストールするための設計図のようなもの
- Gemfile.lock
  - インストールされたgemの一覧とバージョンが記載
  - 依存関係にあるgemも含む
  - どの環境でも同じgem, gemのバージョンを使用するために必要

bundle install

- `Gemfile.lock`をもとに、gemをインストールする
- `Gemfile.lock`に記載されていないgemが、`Gemfile`にある場合、そのgemと関連するgemをインストール（`Gemfile.lock`も更新）
- 新しい環境や、`Gemfile`に新しくgemを追加した時に実施

bundle update

- `Gemfile`をもとにgemをインストールする
- その後、`Gemfile.lock`を更新する
- gemのバージョンを更新するとき
- ローカル環境で実施

### 2. Railsのアクション

アクションとは、ユーザからリクエストを受けた時の、Webサーバ側で行われる動作のこと。
railsでいうコントローラのメソッドのこと。

[アクションとは【Ruby on Rails超入門】初心者にも分かりやすく解説](https://web.lingual-ninja.com/2018/12/ruby-on-rails-action.html)
