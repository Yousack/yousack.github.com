---
layout: post
title: "AOJ 0076 Treasure Hunt II with arctan"
description: ""
category: procom
tags: [aoj]
---
{% include JB/setup %}

[問題文](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0076)．0016のTreasure Huntとは違って，点 \\( (1,0) \\) から，その位置ベクトルの反時計回りの単位法線ベクトルを足すという操作をひたすら繰り返す．先に座標から \\( \arctan \\) を使って角度を求めてそれに \\( \pi / 2 \\) を足すという幾何的な方法でACを出したけど，普通に代数的に法線ベクトルが求まるのを解き始めた翌日に気付いた．ということで先に \\( \arctan \\) を使う方．

## 解法

C#では \\( -\pi / 2 \leq \arctan{x} \leq \pi / 2 \\) なので，原点と点 \\( (x,y) \\) の成す角 \\( \theta \\) は \\( \arctan{ \frac{y}{x} } \\)．ここから反時計回りに \\( \pi / 2 \\) だけ進めた角度 \\( \theta' \\) は

\\[
\theta' =
\begin{cases}
\theta + \frac{\pi}{2} (x \geq 0) \\\
\theta - \frac{\pi}{2} (x \lt 0)
\end{cases}
\\]

よって進める方向は \\( (\cos{\theta'}, \sin{\theta'}) \\)．これを先に1,000回分計算してリストとして持っておいて，あとは入力された行数を元にリストから索引して出力する．

## 提出コード

C#の配列は参照なので，L17でリストの最後の要素を参照してそれに加算代入しちゃったりすると最終的にリストの中身が全部等しく最後に計算した値になる．のでちゃんと配列を新しく作ってAddしましょう．

{% gist Yousack/e57901ba03c07b901da3 %}
