# Ruby on Rails チュートリアル

第1, 2章は省略

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

- Railsにおいて重要なRubyについて

### 4.1 動機

#### 4.1.1 組み込みヘルパー

- Rubyのいくつかの概念（組み込み関数、カッコを使わないメソッド呼び出し、シンボル、ハッシュ）を含む説明

#### 4.1.2 カスタムヘルパー

- Railsのビューでは膨大な数の組み込み関数が使える
- 新しく作成もできる（カスタムヘルパー）
- 基本タイトルを定め、タイトルを与えていないときに空欄になるのを防ぐ
- `full_title`ヘルパーの実装

### 4.2 文字列とメソッド

- Railsコンソール
  - Railsアプリケーションを対話的に操作するためのコマンドラインツール
  - irb(interactive Ruby)を拡張して作られている
  - Rubyの機能をすべて使える
  - これを使用してRubyを学ぶ

```sh
$ rails console
Loading development environment (Rails 7.0.4)
irb(main):001:0> 
```

デフォルトでは、コンソールは、development環境で起動する。
`Ctrl-C`あるいは`Ctrl-D`で抜けられる。

#### 4.2.1 コメント

- `#`

#### 4.2.2 文字列

- Webアプリにおける最も重要なデータ構造
- > Webページというものが究極的にはサーバーからブラウザに送信された文字列にすぎない
- 文字列リテラル
- 文字列の結合
- 式展開 `#{}`

```irb
irb(main):016:0> "#{first_name} #{last_name}"
=> "test aaa"
```

出力

- `puts`：put string
  - `\n`が自動で末尾に出力
- `print`は改行しない

シングルクォート内の文字列

- シングルクォート内では、式展開が行われない
- エスケープせずに表示するときに使用

#### 4.2.3 オブジェクトとメッセージ受け渡し

- オブジェクト：（いついかなる場合にも）メッセージに応答するもの
  - メッセージ：（オブジェクト内で定義された）メソッドのこと
- `nil`もオブジェクト

```irb
irb(main):008:0> "atamanonaka".empty? # 文字列オブジェクトにempty?というメッセージを送っている
=> false # 結果が応答
```

- 返り値がbooleanの場合、メソッド名の末尾に疑問符をつける慣習

```irb
irb(main):010:1* if s.nil?
irb(main):011:1*   "nil!"
irb(main):012:1* elsif s.empty?
irb(main):013:1*   "empty"
irb(main):014:1* elsif s.include?("ue")
irb(main):015:1*   "he!"
irb(main):016:0> end
=> "he!"
```

論理値は`&&`, `||`, `!`

```irb
irb(main):018:0> nil.to_s.empty?
=> true
```

```rb
puts "xは空じゃないよ" if !x.empty? # 後続if
puts "#{x}だよ、空じゃないよ" unless x.empty? # 上と同じ意味
```

`nil`について

- Rubyのオブジェクトのうち、オブジェクトの論理値そのものがfalseになるのは、`false`と`nil`のみ
- `0`も`true`

#### 4.2.4 メソッドの定義

Railsコンソールでも、メソッドの定義は可能。

```rb
def string_message(str = '') # 引数にデフォルト値を含めている
  if str.empty?
    "空だお"
  else
    "空じゃないお"
  end
end
```

```irb
> puts string_message # メソッドの引数を省略できる
空だお
```

- Rubyの戻り値には、暗黙の戻り値がある
  - メソッド内で最後に評価された式の値が自動的に返却される

#### 4.2.5 titleヘルパー、再び

`module ApplicationHelper`

- 関連したメソッドをまとめる方法の1つ
- mixed inできる
  - `include`メソッドを使って、モジュールを読み込むこと
- Railsでは、自動的にヘルパーモジュールを読み込める

### 4.3 他のデータ構造

#### 4.3.1 配列と範囲演算子

配列

- 特定の順序を持つ要素のリスト
  - ハッシュやデータモデルを理解するための重要な基盤
    - データモデル：`has_many`などの関連づけ

```irb
> "bar baz    foo".split
=> ["bar", "baz", "foo"]
> "booxgggxgog".split('x')
=> ["boo", "ggg", "gog"]
```

- 配列のインデックスはマイナスにもなれる
  - `-1`を使うことで、（長さを知らなくても）最後の要素を指定できる
  - `a.length - 1`と同じ意味
- 角カッコ以外に配列要素にアクセスできる(`first`, `second`, `last`)

- さまざまなメソッド
  - `length`
  - `empty?`
  - `include?`
  - `sort`
  - `reverse`
  - `shuffle`
  - `join`
- `!`を末尾に追加で、破壊的メソッドになる

比較演算子

- `==`

配列の要素の追加

- `push`や`<<`
- 要素をチェーンで追加できる
- 異なる型の共存も可能

```irb
> b = [1, 2, "test"]
=> [1, 2, "test"]
> b << 3 << "desu"
=> [1, 2, "test", 3, "desu"]
```

範囲

- `0..9`みたいなやつ
- `('a'..'e')`もできる
- `(0..9).to_a`は、`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`
- 配列の要素を取り出すのに便利

```irb
a = %w[foo bar yes] # %wを使って文字列の配列に変換
=> ["foo", "bar", "yes"]
a[0..1]
=> ["foo", "bar"]
```

#### 4.3.2 ブロック

配列と範囲は、ブロックを伴うメソッドに対して応答できる

- 波カッコ(短い1行)か`do~end`(長い1行、複数行)で囲んで、ブロックと示せる

- map
  - 渡されたブロックを、配列や範囲オブジェクトの各要素に対して適用
  - 適用した結果を返却する
  - symbol-to-proc記法（下述）による省略が可能な場合あり

```rb
# 以下は同じ意味 ["a", "b", "c"]
%w[A B C].map { |char| char.downcase }
%w[A B C].map(&:downcase)
```

#### 4.3.3 ハッシュとシンボル

ハッシュ

- インデックスとして整数値以外のものを使える
- 並び順が保証されない
- キーとしては（文字列ではなく）シンボルを使うのが普通

```rb
user = {}
user = { "first_name" => "hoge", "last_name" => "hahaha" } # リテラル表現での定義
user = { :name => "hoge", :email => "hahaha" } # リテラル表現での定義
x = user[:password] # nil
user = { name: "hoge", email: "hahaha" } # 同じ意味
```

ネストされたハッシュ

```rb
params = {}
params[:user] = { name: "test", email: "aaa@example.com" }
y = params[:user][:email] # aaa@example.com
```

- inspect
  - 要求されたオブジェクトを表現する文字列を返す
  - `p`

#### 4.3.4 CSS、再び

- メソッド呼び出しの丸カッコは省略可能
- ハッシュがメソッド呼び出しの最後の引数である場合は、波カッコを省略できる

```erb
<%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
```

- `stylesheet_link_tag`メソッドを2つの引数で呼び出し
- 2つ目の引数はハッシュ
  - turbolinksという機能をONにしている

### 4.4 Rubyにおけるクラス

クラスにより、メソッドをまとめ、インスタンスが生成されることで、オブジェクトが作成される

#### 4.4.1 コンストラクタ

- ダブルクォートは、（暗黙的な）文字列のコンストラクタ
- 明示的に、名前付きコンストラクタも使える
  - `s = String.new("foo")`
- 同様に：`a = Array.new([1, 3, 1])`
- ハッシュでは、ハッシュのデフォルト値を引数に取る

```irb
> h = Hash.new(0)
=> {}
> h[:foo]
=> 0
```

上記の`new`は、クラスメソッド

#### 4.4.2 クラス継承

`superclass`メソッドを使って、クラス階層を調べられる

- `String < Object < BasicObject`

Stringクラスを継承したWordクラスを定義する

```rb
class Word < String
  def palindrome?
    self == self.reverse # オブジェクト自身が持つ単語にアクセスし比較
  end
end
```

#### 4.4.3 組み込みクラスの変更

Rubyでは組み込み基本クラスの拡張が可能

- 真に正統的な理由がある場合
  - 例：変数が絶対に空白にならないようにしたい場合
  - Railsは`blank?`メソッドをRubyに追加している
    - `""`, `"   "`, `nil`は`blank?`でtrue

#### 4.4.4 コントローラクラス

`StaticPagesController < ApplicationController < ActionController::Base < ActionController::Metal < AbstractController::Base < Object`

- Railsのアクションには戻り値がない

#### 4.4.5 ユーザークラス

- マスアサインメント(mass assignment)
  - 属性が定義済みの他のユーザを作成できる
  - `user = User.new(name: "Michael Hartl`, email: "mhartl@example.com")

### 4.5 最後に

## 第5章　レイアウトを作成する

- アプリケーションにBootstrapフレームワークを組み込む
- カスタムスタイルを追加する
- 作成したページへのリンクをレイアウトに追加する
- パーシャル
- ルーティング
- Asset Pipeline
- Sass

### 5.1 構造を追加する

- Bootstrapを利用する
- レイアウトのコードを、partial機能を使って整えていく
- モックアップ（ワイヤーフレーム）を作成する

#### 5.1.1 ナビゲーション

- `link_to`（Railsヘルパー）
  - 第一引数：リンクテキスト
  - 第二引数：URL
  - 第三引数：オプションハッシュ
- `image_tag`：該当する画像を、アセットパイプラインを通して、`app/assets/images`から取得する

`<img alt="Rails logo" src="/assets/rails-9308b8f92fea4c19a3a0d8385b494526.png" />`

- 画像ファイルを更新した場合に、ブラウザ内に保存されたキャッシュを意図的にヒットさせないようにするための仕組み
- `src`属性に、`images`というディレクトリ名が含まれていない
  - フラットなディレクトリ構成（ブラウザから見ると、全て同じディレクトリにあるように見える）だと、ファイルを高速にブラウザに渡せる

#### 5.1.2 BootstrapとカスタムCSS

Bootstrap

- 洗練されたWebデザインとUI要素をHTML5アプリに追加できる
- レスポンシブデザインにできる
- 動的なスタイルシートを生成するために、LESS CSS言語を使う

カスタムCSSとBootstrapを組み合わせて使う

bootstrap-sass

- LESSをSassに変換する
  - RailsのAsset PipelineはデフォルトではSassをサポートする
- 必要なBootstrapファイルを現在のアプリ全てで利用できるようにする

`app/assets/stylesheets/`は、Asset Pipelineの一部

- このディレクトリに置かれたスタイルシートは、`applidation.css`の一部として、Webサイトのレイアウトに読み込まれる

Sass

- Sassy CSS
- CSSを拡張した言語

#### 5.1.3 パーシャル (partial)

`<%= render 'layout/shim' %>`

- `app/views/layouts/_shim.html.erb`というファイルを探して、内容を評価、結果をビューに挿入

### 5.2 Sassとアセットパイプライン

- アセットパイプライン
  - CSS, JavaScript, 画像などの静的コンテンツの生産性と管理を強化
- Sass
  - CSS生成ツール

#### 5.2.1 アセットパイプライン

- アセットディレクトリ
- マニフェストファイル
- プリプロセッサエンジン

アセットディレクトリ

- 静的ファイルを目的別に分類するディレクトリ
  - `app/assets`
    - 現在のアプリケーション固有のアセット
  - `lib/assets`
    - 開発用ライブラリのアセット
  - `vendor/assets`
    - サードパーティのアセット
- それぞれアセットクラス用のサブディレクトリがある

マニフェストファイル

- アセットディレクトリに配置した静的ファイルを、どのように1つのファイルにまとめるかRailsに指示するファイル
- 実際にまとめる処理を行うのは、Sprocketsというgem

プリプロセッサエンジン

- ディレクトリに配置されたアセットを実行
- マニフェストファイルを用いて、ブラウザで配信できるように結合
- サイトテンプレート用に準備

Railsが使うプリプロセッサを判断するための拡張子

- `.sass`
- `.coffee`
- `.erb`

プリプロセッサはチェインできる

- Asset Pipelineのメリット
  - 本番のアプリで効率的になるよう最適化されたアセットも自動的に生成される
  - 最小化されていないCSSやJavaScriptファイルを多数に分割すると、ページの読み込み時間が遅くなる
  - 開発環境ではプログラマによって読みやすく、本番環境ではAsset Pipelineを用いてファイルを最小化する

Asset Pipelineがやること

- 全てのCSSファイルを`application.css`にまとめる
- 全てのJavaScriptファイルを`javascript.js`にまとめる
- 不要な空白やインデントを取り除き、ファイルを最小化

#### 5.2.2 素晴らしい構文を備えたスタイルシート

- Sassが提供する2つの重要な機能
  - ネスト
  - 変数
  - ミックスイン

SassはCSSに新しい機能を追加しただけ

```scss
.center {
  text-align: center:
  h1 {  // h1は.centerのルールを継承する
    margin-bottom: 10px;
  }
}
```

```scss
#logo {
  float: left;
  ...
  &:hover { // #logo:hover
    color: #fff;
  }
}
```

```scss
$light-gray: #777;  // 変数の定義が可能
...
h2 {
  ...
  color: $light-gray;
}
```

### 5.3 レイアウトのリンク

Railsでのリンクの記法は、名前付きルートを用いるのが慣例：`<%= link_to "About", about_path %>`

#### 5.3.1 Contactページ

#### 5.3.2 RailsのルートURL

`root`メソッドを使って、ルートURL"/"をコントローラのアクションに紐づけている

```rb
root 'static_pages#home'
```

ルートURLを定義することで、メソッドを通してURLを参照できる

```rb
root_path -> '/'
root_url -> 'http://www.example.com/'
```

- `_path`：基本的にはこちらを利用
- `_url`：リダイレクトの時のみ

#### 5.3.3 名前付きルート

`config/routes.rb`でルートを定義したので、レイアウトの中で名前付きルートが使えるようになる

#### 5.3.4 リンクのテスト

統合テスト(Integration Test)

- アプリケーションのend-to-endをテストできる

アプリケーションのHTML構造を調べて、レイアウトの各リンクが正しく動くかをチェックする

- ルートURL（Homeページ）にGETリクエストを送る
- 正しいページテンプレートが描画されているかどうかを確かめる
- Home, Help, About, Contactの各ページへのリンクが正しく動くか確かめる

※テストファイルは作成したが、内容については省略

### 5.4 ユーザ登録：最初のステップ

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
