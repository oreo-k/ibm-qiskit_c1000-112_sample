# Q1 

> Which statement will create a quantum circuit with four quantum bits and four classical bits?  

[A]  
QuantumCircuit(4, 4)  

[B]  
QuantumCircuit(4)  

[C]  
QuantumCircuit(QuantumRegister(4, 'qr0')
QuantumRegister(4, 'cr1'))  

[D]  
 QuantumCircuit([4, 4])  

## 答え
[A]

## 解説
問題文は４つの量子ビットと4つの古典ビットを作るCode分を求めている。量子ビットを扱う回路の中に量子ビットと古典ビットを準備する最も簡単な方法がAである。

qiskit APIには，QuantumeCircuit Classが準備されており
```
qc = QuantumCicuit(q,c)
```
と書くことで,量子ビットをq個，古典ビットをc個並べた量子回路のインスタンスを作ることができる。  
また，選択肢にはないが下記のように　量子，古典レジスタを使って回路を構成しても良い。
```
QuantumCircuit(QuantumRegister(4,"q"), ClassicalRegister(4, "c"))
```

### 他の選択肢の解説
[B]  
因数を1つにする場合，その数の量子ビットが作られるため 4つの量子ビットを持つ回路となる。  
[C]  
名称指定されているqr0 量子ビットレジスタが4, cr1量子ビットが4つの合計8つの量子ビットが準備された回路となる。  
[D]  
数値を入力したリストは受け付けていない

## その他
### 量子, 古典レジスタの使い方
qiskit APIには，QuantumRegister, ClassicalRegister Classが用意されており，
それらのビットに任意の名前を付けたい際に便利である。



# Q2. 

> Given this code fragment, what is the probability that a measurement would result in $\ket{0}$ ?

```
qc = QuantumCircuit(1)
qc.ry(3 * math.pi/4, 0)
```

[A]  
0.8536  

[B]  
0.5  

[C]  
0.1464  

[D]  
1.0  


## 答え
[C]

## 解説
初期状態 $\ket{0}$に対して, Y軸方向に$\theta = \frac{3 \pi}{4}$回転させた場合に，$\ket{0}$が観測される確率を求める問題。

回転後の状態を$\ket{\Psi'}$とすると，Y軸方向の$\theta$回転後の状態は
$\ket{\Psi'} = \cos{\frac{\theta}{2}} \ket{0} + \sin{\frac{\theta}{2}\ket{1}}$となる。
ちなみに，上式が得られる理由は，Y軸方向の回転を操作する行列が下記で表されるためであり，
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
この行列を$\ket{0}$に作用させると，$\ket{\Psi'}$が得られることが計算で確かめることができる。

そのため，回転後に$\ket{0}$が観測される確率
$$
\begin{align}
    P_{\ket{0}}  &= \braket{0|\Psi'}^2 \\
    &=\bra{0} (\cos{\frac{3 \pi}{8} \ket{0}} + \sin{\frac{3 \pi}{8}} \ket{1})^2 \\
    &= \cos^2{\frac{3 \pi}{8}}
\end{align}
$$Name: Paste Image
Id: mushan.vscode-paste-image
Description: paste image from clipboard directly
Version: 1.0.4
Publisher: mushan
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image

このとき，倍角の公式を使って，$\cos^2{\frac{3 \pi}{8}}$を計算してもいいが，面倒なので　それよりもざっくりとした値を把握することを優先する。こういった場合は，角度$\theta$で$\cos^2{\frac{3 \pi}{8}}$を評価すればよい。高校数学の範囲の計算だが，自分の理解の確認のために書いてみる。

### $\cos^2{\theta}$の評価(高校数学の復習)
$\theta=\frac{3 \pi}{8}$は
$\frac{\pi}{3}<\theta = \frac{3\pi}{8} < \frac{\pi}{2}$
と評価できる。このため，

$$
\begin{align}
\cos{\frac{\pi}{2}} & < \cos{\theta} < \cos{\frac{\pi}{3}} \\
0 & < \cos{\theta} < \frac{1}{2} \\
よって，
0 & < \cos^2{\theta} < \frac{1}{4}
\end{align}
$$

つまり，選択肢の中で0より大きく$\frac{1}{4}$より小さい値となり，答えはCとなる。


# Q3. 
> Assuming the fragment below, which three code fragments would produce the circuit illustrated?

![](2023-06-05-20-37-26.png)
 
```python 
  inp_reg = QuantumRegister(2, name='inp')
  ancilla = QuantumRegister(1, name='anc')
  qc = QuantumCircuit(inp_reg, ancilla)
  #Insert code here
```
>
[A]  
qc.h(inp_reg)  
qc.x(ancilla)  
qc.draw()  

[B]  
qc.h(inp_reg[0:2])  
qc.x(ancilla[0])  
qc.draw()  

[C]  
qc.h(inp_reg[0:1])  
qc.x(ancilla[0])  
qc.draw()  

[D]  
qc.h(inp_reg[0])  
qc.h(inp_reg[1])  
qc.x(ancilla[0])  
qc.draw()

[E]  
qc.h(inp_reg[1])  
qc.h(inp_reg[2])  
qc.x(ancilla[1])  
qc.draw()  

[F]  
qc.h(inp_reg)  
qc.h(inp_reg)  
qc.x(ancilla)  
qc.draw()  



