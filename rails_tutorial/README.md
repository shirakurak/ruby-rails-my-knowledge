# Ruby on Rails チュートリアル

## 第3章 ほぼ静的なページ

- 静的なページの作成から始める
  - 最終的にはマイクロポスト、ログイン/ログアウトの認証などの機能を持つ
  - Rails自体は、DBと連携して動的なWebサイトを開発するよう設計

### 3.1 セットアップ

- `hello`アクションとルーティングの追加

---

## 気になったこと

### bundle installとbundle updateの違い

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
- 新しい環境や、`Gemfile`に新しくgemを追加した時
s
bundle update

- `Gemfile`をもとにgemをインストールする
- その後、`Gemfile.lock`を更新する
- gemのバージョンを更新するとき
- ローカル環境で実施