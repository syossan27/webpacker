# Webpacker

Webpackerを使用すると、JavaScriptプリプロセッサとバンドラ [Webpack]（http://webpack.github.io） を使いやすくなります。
RailsでアプリケーションのようなJavaScriptを管理します。

アセットパイプラインと共存するのは、WebpackをアプリのようなJavaScriptだけで使用し、画像、CSS、またはJavaScriptのスプリンクル（すべてがアプリ/アセットに存続する）ではないからです。

これは、Rails5.1以上で動作するように設計されており、Yarnによる依存関係管理
は5.1からデフォルトになっています。
現在、Rails5.0の安定版と互換性がありますが、今後も保証はありません。
`--webpack`で新しいアプリケーションをセットアップするときにWebpackerを使うか
またはgemを追加して、既存のアプリケーションに `bin/rails webpacker：install`を実行することができます。

## Binstubs

Webpackerには、`./bin/webpack`と`./bin/webpack-watcher`の2つのbinstubが付属しています。
それらは両方とも標準のwebpack.js実行可能ファイルの周りの薄いラッパーで、適切な構成ファイルがロードされ、ベンダーのnode_moduleが確実に使用されるようにします。

開発時には、`./bin/webpack-watcher`を`./bin/rails server`とは別のターミナルで実行して、変更を加えるときに`app/javascript/packs/*。js`ファイルをコンパイルする必要があります。
2つのプロセスを手動で別々に実行する必要がない場合は、[Foreman](http://ddollar.github.io/foreman/)を使用できます。


## Configuration

Webpackerは、開発と生産のための設定ファイルのデフォルトセットを提供します。
それらはすべて `config/webpack/*。js`の共有ポイントと同居しています。
デフォルトでは、ボックスの基本的な設定を変更する必要はありません。

Webpackがデフォルトでコンパイルするはずの設定は、`app/javascript/packs/*`内のすべてのファイルを独自の出力ファイル（またはWebpackが呼び出すときのエントリポイント）に変換するという規則に基づいています。

カレンダーを作成すると、構造は次のようになります：

```erb
<%# app/views/layout/application.html.erb %>
<%= javascript_pack_tag 'calendar' %>
```

```js
// app/javascript/packs/calendar.js
require('calendar')
```

```
app/javascript/calendar/index.js // gets loaded by require('calendar')
app/javascript/calendar/components/grid.jsx
app/javascript/calendar/models/month.js
```

しかし、それは数多くの他の方法でも見ることが出来ます。
Webpackerが強制する唯一の慣例は、エントリポイントが `app/javascript/packs`のファイルによって自動的に設定されるものです。

## Deployment

開発中にすべてのパックをコンパイルするには、`rails webpacker：compile`コマンドを使用できます。
これにより、ダイジェストを含むプロダクション構成が呼び出されます。

`javascript_pack_tag`ヘルパーメソッドは、プロダクションモードで実行されるときに、アセットパイプラインと同じように自動的に正しいダイジェストを挿入します。


## Ready for React

ReactでWebpackerを使用するには、`rails new myapp - webbpack = react`を使って新しいアプリケーションを作成してください（またはwebpackで既に設定されているRails5.1で`rails webpacker：install：react`を実行してください）。
すべての関連する依存関係がyarnを介して追加され、設定ファイルが変更されます。
これで、JSXファイルを作成し、自動的に適切にコンパイルすることができます。

## Work left to do

- Webpackからアセットパイプラインのダイジェストを読みやすくして、画像などを参照できるようにする
- チャンク設定を検討する
- digesting = trueの場合、ダイジェストによるオンデマンドコンパイルを検討する
- きっと他に多くのダメな部分がある

## License
Webpacker is released under the [MIT License](http://www.opensource.org/licenses/MIT).
