#　Zennでの数式表記まとめ

## 行列表記
### 文章と分離した数式上に行列を書く
これは表記できるらしい。

$$
\begin{align}
    R_{Y}(\theta) =
        \left(
        \begin{matrix}
        \cos{\frac{\theta}{2}} & - \sin{\frac{\theta}{2}} \\
        \sin{\frac{\theta}{2}} & \cos{\frac{\theta}{2}}
        \end{matrix}
        \right)
\end{align}
$$  


### 文中にMatrixを入れたい
初期状態$\ket{\Psi_{0}} =
  \left(
    \begin{matrix}
      1 \\
      0 \\
    \end{matrix}
  \right)
    = \ket{0}$に対して, Y軸方向に$\theta = \frac{3 \pi}{4}$回転させた場合に，