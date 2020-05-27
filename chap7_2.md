$\newcommand{\V}[1]{\boldsymbol{#1}}$

# 7. 自己位置推定の<br />諸問題（後半）

千葉工業大学 上田 隆一

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 7.2 より難しい自己位置推定

* やること
    * スタックや誘拐で起こる問題を確認する
    * 問題に対応するためのアルゴリズムを考える

---

## 7.2.1 大域的自己位置推定

* ロボットの初期姿勢$\V{x}_0$が分からない場合を考える
    * 5, 6章では$\V{x}_0$が分かるという前提だった<br />　
* 初期の信念分布$b_0$は一様分布が適切
    * カルマンフィルタでは非常に広い分布で近似可能だが<br />線形化の影響が心配
    * パーティクルフィルタだと近似はできるが<br />パーティクルの数が不足

---

### 実験

* ランダムにパーティクルを配置して自己位置推定
    * 図は$N=100$の場合<br />
<img width="40%" src="./figs/mcl_global.gif" />
    * 偶然センサ値と矛盾のない姿勢にいたパーティクルが残る


---

### 実験結果

* 大域的自己位置推定に成功する確率を求める
    * 成功: 30秒後に$XY$平面での誤差が1[m]以内<br />　
* 結果
    * カルマンフィルタ: 半分失敗
    * パーティクルフィルタ: $N$と成功率は比例しない
        * 多次元空間を埋め尽くすには指数乗のパーティクルが必要

![](./figs/table7.1.jpg)

---

## 7.2.2 誘拐ロボット問題

* ロボットの姿勢が突然移動するという問題
    * 突然、信念の分布とロボットの姿勢が乖離
    * 下図: パーティクルとロボットを別の場所からスタート<br />
<img width="40%" src="./figs/mcl_kidnap.gif" />
    * パーティクルのないところのことは何も考慮されないので<br />対応不可能


---

### 実験と結果

* 大域的自己位置推定のときと同様に実験
    * 前ページのアニメーションのようにロボットを信念分布と離してスタート<br />　
* パーティクルフィルタ: 対応できていない
* カルマンフィルタ: 多少は対応可能
    * 少しずつ分布が観測したランドマークの方に寄っていくので

<img width="70%" src="./figs/table7.2.jpg" />

---

## 7.3 推定の誤りの考慮

* 信念分布が間違っているかもしれない
    * これを今まで考えてこなかったのがいけない
    * ロボットの不安を拡張
        * 姿勢が不確か$\rightarrow$<span style="color:red">推定自体が不確か</span><br />　
* 準備
    * 「信念分布が正しいかどうかを表す変数$\Upsilon$」の導入
        * $\Upsilon=0$: 正しい
        * $\Upsilon=1$: 正しくない<br />
        （$\Upsilon$は「ウプシロン」と読む）

---

### 信念分布の拡張

* $\Upsilon$を考慮して信念分布を書き直し
    * $b(\boldsymbol{x}) = b(\boldsymbol{x} | \Upsilon=0)P(\Upsilon=0) + b(\boldsymbol{x} | \Upsilon=1)P(\Upsilon=1)$
        * 第一項: 今までの信念分布
        * 第二項: 自己位置推定が間違っているときの信念分布<br />　
* 新たな信念分布の計算に必要なこと
    * 間違っている確率$P(\Upsilon)$をどうやって求めるか
    * 分布$b(\boldsymbol{x} | \Upsilon=1)$をどう作るか
