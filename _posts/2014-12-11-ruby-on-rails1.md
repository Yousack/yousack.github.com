---
layout: post
title: "Ruby on RailsによるWebアプリケーションを自分で1から書く"
description: ""
category: tech
tags: [ruby]
---
{% include JB/setup %}

# 動機

- アイドルマスターSideMでデッキを組むときに，タイプを通したRのリストが欲しくなった
- Wikiサイトはどこもタイプごとに分割してある
- じゃあ自分でスクレイピングしてDBにして使えるようにしよう（涙目）

# プロジェクトを作る

~~~ sh
rails new sidem_carddb
~~~

おわり．

# コントローラーを作る

ここから．モデルからデータを取ってきてビューに渡すコントローラーから書く．コントローラーは複数形．コントローラー名の後にエンドポイントを指定するとビューとメソッドも自動で追加される？

~~~ sh
rails g controller cards index
~~~

`./app/cards_controller.rb`

~~~ ruby
class CardsController < ApplicationController
  def index
    @cards = Card.order(:sum).reverse_order
  end
end
~~~

Cardモデルから能力の合計値でソート（昇順）したのをひっくり返して（降順）ビューに渡す．`reverse_order`には引数は渡せないらしい．悲しい．

# モデルを作る

~~~ sh
rails g model card name:string vocal:integer dance:integer visual:integer sum:integer cost:integer
rake:db migrate
~~~

こんな感じだった気がする．

## シードを投入する

今回初めて知った．`rake:db seed`すると，`./db/seed.rb`を実行するので，このファイルの中にシード（初期値？　テスト用に？　みたいな）を投入するスクリプトを書けばいいらしい．今回は面倒なので既存のカードを全て突っ込んだ．研修あまとうのMMMがまだ判明してなくて値が"-"になってたから例外処理を書いておいたけどなんか勝手に0に変換されて登録された．こわい．

`.db/seed.rb`

~~~ ruby
require 'csv'

CSV.foreach('db/cards.csv') do |row|
  begin
    Card.create(name: row[0], vocal: row[1], dance: row[2], visual: row[3],
                sum: row[4], cost: row[5])
  rescue
    puts "#{row[0]}は突っ込めませんでした"
  end
end
~~~

# ビューを作る

`./app/views/cards/index.html.erb`

~~~ erb
<h1>アイドルマスターSideM カード一覧</h1>

<table>
    <tr>
        <th>カード名</th>
        <th>Vocal</th>
        <th>Dance</th>
        <th>Visual</th>
        <th>合計値</th>
        <th>編成値</th>
    </tr>
    <% @cards.each do |card| %>
        <tr>
            <td><%= card.name %></td>
            <td><%= card.vocal %></td>
            <td><%= card.dance %></td>
            <td><%= card.visual %></td>
            <td><%= card.sum %></td>
            <td><%= card.cost %></td>
        </tr>
    <% end %>
</table>
~~~

まあそのまま．`render`メソッドを使おうと思ったけどどうも自分で部分テンプレートを作らないといけないっぽいので面倒だからやってない．

とりあえず今回はここまで，次に時間が余裕のあるとき（というか冬休みに入ったら……？）CRUDを実装する．
