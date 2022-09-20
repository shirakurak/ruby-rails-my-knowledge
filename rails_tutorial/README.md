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