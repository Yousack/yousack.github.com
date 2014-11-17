---
layout: post
title: "AOJ 0076 Treasure Hunt II with normal vector"
description: ""
category: procom
tags: [aoj]
---
{% include JB/setup %}

前回の続き．法線ベクトルを使った解法．とりあえず一般的な式を求める．高校までの知識だけで大丈夫だった．

あるベクトル \\( \mathbf{a} = (a,b)\ (\mathbf{a} \in \mathbb{R}^2,\ a \neq 0 \land b \neq 0) \\) に対する単位法線ベクトルが \\( \mathbf{e} = (x, y) \\) だとする．この時，この2つのベクトルが成す角が直角であることと，\\( \mathbf{e} \\) のノルムが1であることから，

\\[\begin{aligned}
\mathbf{a} \cdot \mathbf{e}\ &= 0 \iff ax + by = 0 & (1)\\\
\| \mathbf{e} \|\ &= 1 \iff x^2 + y^2 = 1^2 = 1 & (2)
\end{aligned}\\]

という関係が成り立つ．この2式を連立して解くと，

\\[
x = - \frac{b}{a}y \\\
\left( - \frac{b}{a} y \right) ^2 + y^2
= \left( 1 + \frac{b^2}{a^2} \right)y^2 = 1 \\\
\Rightarrow y^2 = \frac{1}{ 1+ \frac{b^2}{a^2} }
= \frac{a^2}{a^2 + b^2} \\\
\Rightarrow y = \pm \sqrt{ \frac{a^2}{a^2+b^2} }
= \pm \frac{a}{\sqrt{a^2+b^2}}
= \pm \frac{a}{|\mathbf{a}|} \\\
\Rightarrow x = - \frac{b}{a} \cdot \left( \pm \frac{a}{|\mathbf{a}|} \right)
= \mp \frac{b}{|\mathbf{a}|}
\\]

よって

\\[
\mathbf{e} = \left( \mp \frac{b}{|\mathbf{a}|}, \pm \frac{a}{|\mathbf{a}|} \right)
\\]

結構きれいな形になった．途中でルートを外すときに絶対値を付けたりしてないのはまあ目をつぶっていただきたく……（というかそこからどういうふうに進めれば \\( b \\) にもうまく絶対値を付けられるのかが<del>めんどう</del>分からなかった）．どうもこれは複号の上の記号を取ると常に反時計回りの法線ベクトルになるっぽい．何でかはよく分からない．

もう1つ．法線ベクトルというのはどうも勾配ベクトルというのに一致するっぽい．勾配ベクトルっていうのは，曲面上のある1点について，そこから登るのが一番厳しい方向を表すベクトルのこと（とても雑）で，この定義が，

\\[
\nabla f(x_1, x_2, x_3, \cdots, x_n) =
\frac{\partial f}{\partial x_1}\mathbf{e_1} +
\frac{\partial f}{\partial x_2}\mathbf{e_2} +
\frac{\partial f}{\partial x_3}\mathbf{e_3} + \cdots +
\frac{\partial f}{\partial x_n}\mathbf{e_n}
\\]

で，上式の \\( \mathbf{e_i} \\) は2次元平面上においては \\( \mathbf{e_x} \\) と \\( \mathbf{e_y} \\) ということになる．ここで，原点と点 \\( (a,b) \\) を通る直線の式は

\\[
y = \frac{b}{a}x \iff bx - ay = 0
\\]

ここで，右側の方程式を \\( f(x,y) \\) とおくと，

\\[\begin{aligned}
\nabla f(x,y) = \frac{\partial f}{\partial x}\mathbf{e_x} +
\frac{\partial f}{\partial y}\mathbf{e_y}
= b \mathbf{e_x} - a \mathbf{e_y}
= (b, -a)
\end{aligned}\\]

で，これは上の方で出した \\( \mathbf{e} \\) の定数 ( \\( \mp \|\mathbf{a}\| \\) ) 倍なので，これも直線 \\( f(x,y) \\) の法線ベクトルの1つということになる．

分かったような分からんような……．

## 提出コード

どう考えてもこっちの方が綺麗です本当にありがとうございました．

{% gist Yousack/05d8875db1a32ede816a %}
