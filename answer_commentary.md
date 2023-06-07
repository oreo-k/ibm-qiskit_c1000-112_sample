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
$$

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

## bloch simulator 

bloch　simulatorを使って，Y軸方向の$\frac{3 \pi}{4}$回転した状態を描画してみる。

```python
simulator = Aer.get_backend('statevector_simulator') # statevector simulator
job = execute(qc, simulator) # run simulation
result = job.result() # get results
statevector = result.get_statevector(qc) # get state vector
plot_bloch_multivector(statevector) # plot state vector
```

![](2023-06-07-21-08-22.png)

## Aer simulation
この結果はAer Simulator を使って，ノイズフリーシミュレーションを実行できる。

```python

## from qiskit import Aer
## from qiskit.visualization import plot_histogram

#simulating a quantum circuit with Aer Simulator
simulator = Aer.get_background("aer_simulator") ##
circ = transpile(qc,simulator)
result = simulator.run(circ).result()
counts = result.get_counts(circ)

#calculating and plotting the probability to get "0" state after measurering the states
probability_0 = counts["0"]/(counts["0"]+counts["1"])
print("probabity to get '0' state is {}".format(round(probability_0,4)))
plot_histogram(counts)
```
![](2023-06-07-21-05-43.png)
上記の通り，今回のシミュレートでは状態$\ket{0}$を得る確率は0.1406であることがわかり，選択肢[C]が最も近い答えであることがわかる。


# Q3. 
> Assuming the fragment below, which three code fragments would produce the circuit illustrated?



```python 
  inp_reg = QuantumRegister(2, name='inp')
  ancilla = QuantumRegister(1, name='anc')
  qc = QuantumCircuit(inp_reg, ancilla)
  #Insert code here
```
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

## 答え
[A, B, D]

## 用語
ancilla : 日本語では補助の意味に相当する。量子計算の分野では，計算を進めるにあたり補助的に作用させるビットをancilla bitと呼ぶ。例えば，制御NOTでは，制御ビットの状態に応じてターゲットビットに作用させる演算が決定される。この際，ターゲットビットの状態を決めるための補助的なビットとして制御ビットが存在すると考えることができる。

##　解説
inp_regの0から1番目にq0, q1量子ビットが格納されており，ancillaの0番目にanc量子ビットが格納されている。  
与えられた画像からinp0 とinp1にアダマールゲート，anc0にNOTゲートを作用させる量子回路が与えられており，それらを実装するためのCodeを解答する必要がある。

今回この問題を解いていて初めて知ったが，リストのスライス表現で複数の量子ビットに同じ量子操作を実施できるらしいってことを知ったので勉強してみた。

[Hadamard on qiskit API ref](https://qiskit.org/documentation/stubs/qiskit.circuit.QuantumCircuit.h.html)


[A]
QuantumCircuit.h(qubit)はパラメータとして，QuantumRegisterを取る.
この時，複数のqubitを指定する場合，指定したqubit全てのアダマール変換を施す。

```python
qc.h(inp_reg)  ##複数準備された量子ビットinp_regすべてにアダマール変換を施す
qc.x(ancilla)  ## ancillaにNOT ゲートを施す
qc.draw()
```
これは題意の通りのため，正解。　　

[B]  
```python
qc.h(inp_reg[0:2]) ##inp_regの0~1番目の量子ビットにアダマール変換を施す  
qc.x(ancilla[0])   ## ancillaにNOT ゲートを施す
```
これは題意の通りのため，正解。　　


[C]  
```python
qc.h(inp_reg[0:1])  ##inp_regの0番目の量子ビットにアダマール変換を施す
qc.x(ancilla[0])   ## ancillaにNOT ゲートを施す
```
これは，inp_regの1番目の量子ビットにアダマール変換が施されないため，題意を見足さないため不正解。
![](2023-06-05-21-40-37.png)



[D]  
```python
qc.h(inp_reg[0])  ##inp_regの0番目の量子ビットにアダマール変換を施す
qc.h(inp_reg[1])  ##inp_regの1番目の量子ビットにアダマール変換を施す
qc.x(ancilla[0])  ##ancillaの0番目の量子ビットにアダマール変換を施す
```
これは題意の通りのため，正解。　　


[E]  
```python
qc.h(inp_reg[1])  ##inp_regの1番目の量子ビットにアダマール変換を施す
qc.h(inp_reg[2])  ##inp_regの2番目の量子ビットにアダマール変換を施す
qc.x(ancilla[1])  ##ancillaの1番目の量子ビットにアダマール変換を施す
qc.draw()
```  
inp_regは, 0と1番目の量子ビットの2つしかなく，２番目の量子ビットは存在しない。
そのため，2行目で "IndexError: list index out of range"となりエラーとなる。

[F]  
```python
qc.h(inp_reg)  ##inp_regの0~1番目の量子ビットにアダマール変換を施す  
qc.h(inp_reg)  ##inp_regの0~1番目の量子ビットにアダマール変換を施す  
qc.x(ancilla)   ##ancillaの1番目の量子ビットにアダマール変換を施す
qc.draw()  
```
0~1番目の量子ビットに2回の
アダマール変換が解かされており，題意に反する。
![](2023-06-05-21-47-42.png)

# Q4.
Given an empty QuantumCircuit object, qc, with three qubits and three classical bits, which one of these code fragments would create this circuit?

![](2023-06-05-22-20-47.png)

[A]  
qc.measure([0,1,2], [0,1,2])  

[B]  
qc.measure([0,0], [1,1], [2,2])

[C]  
qc.measure_all()  

[D]  
qc.measure(0,1,2)


## 答え
3つの量子ビットと3つの古典ビットを用意する。
量子ビット[0,1,2]をそれぞれの順番で　古典ビット[0,1,2]で測定する回路を作成する。
qc.measureはqc.cmeasure(qubit, cbit)のように，2つのパラメータを取る。
これを満たしているのは，[A]となる。

[A]  
qc.measure([0,1,2], [0,1,2])  
これは，題意を満たしており，正解。

[B]  
qc.measure([0,0], [1,1], [2,2])
3つのパラメータを取っているため，TypeErrorになる。

[C]  
qc.measure_all()  


qc.measure_all()は，新たに測定用のcbitを追加して測定結果を保存する。
また，qc.measre_all()は，測定前にBarrierを入れる仕様になっている。
与えられた回路では，古典ビットは3つのため，追加された分古典ビットが多くなるため題意に沿わない。そのため不正解。
![](2023-06-05-22-53-30.png)

ちなみに，qc.measure_all()はparameterで既存の古典ビットに測定を保存することができる。
その場合，qc.measure_all(add_bits=False)とする。
この場合，題意を満たすため正解となる。
![](2023-06-05-22-57-14.png)



[D]  
qc.measure(0,1,2)

これも[B]同様，3つのパラメータを取っているため，TypeErrorになる。



# Q5
> Which code fragment will produce a maximally entangled, or Bell, state?

[A]  
bell = QuantumCircuit(2)  
bell.h(0)  
bell.x(1)  
bell.cx(0, 1)  

[B]  
bell = QuantumCircuit(2)  
bell.cx(0, 1)  
bell.h(0)  
bell.x(1)  

[C]  
bell = QuantumCircuit(2)  
bell.h(0)  
bell.x(1)  
bell.cz(0, 1)  

[D]  
bell = QuantumCircuit(2)  
bell.h(0)  
bell.h(0)


## 答え
[A]

## 解説


## simulation
Aer Simulatorでそれぞれの測定確率をPlotしてみる。
![](2023-06-07-20-33-50.png)

上記の結果の通り，$\ket{q_{0}} \otimes \ket{q_{1}}$の直積状態で表されるのは, 
選択肢の[A]のみである。

ちなみに[B], [C]は$\left( \ket{1} \otimes \frac{1}{\sqrt{2}} (\ket{0} + \ket{1}) \right)$, [D]は $\left( \ket{0} \otimes \ket{0} \right)$の直積で表される。[A]は直席では表すことができず，エンタングルしていると言える。

```python
## 量子回路とsubplotのaxを与えることで，ヒストグラムを描画する関数
def plot_hist_states(qc, ax):
    simulator = Aer.get_backend('aer_simulator')
    circ = transpile(bell, simulator)
    result = simulator.run(bell).result()

    counts = result.get_counts()

    return plot_histogram(counts, ax=ax)

fig, axes = plt.subplots(2, 2, figsize=(10, 5))
axax=axes.ravel()

## 各選択肢ごとの量子回路を作成，その状態のヒストグラムを描画する
#[A]
bell = QuantumCircuit(2,2) # 1 qubit, 1 classical bit
bell.h(0)
bell.x(1)
bell.cx(0,1)
bell.measure([0,1],[0,1])
plot_hist_states(bell, axax[0])
axax[0].set_title("A")

#[B]
bell = QuantumCircuit(2, 2)
bell.cx(0,1)
bell.h(0)
bell.x(1)
bell.measure([0,1], [0,1])
plot_hist_states(bell, axax[1])
axax[1].set_title("B")

#[C]
bell = QuantumCircuit(2, 2)
bell.h(0)
bell.x(1)
bell.cz(0,1)
bell.measure([0,1], [0,1])
plot_hist_states(bell, axax[2])
axax[2].set_title("C")


#[D]
bell = QuantumCircuit(2, 2)
bell.h(0)
bell.h(0)
bell.measure([0,1], [0,1])
plot_hist_states(bell, axax[3])
axax[3].set_title("D")

fig.tight_layout()
fig.show()
```




# Q6
> Given this code, which two inserted code fragments result in the state vector represented by this Bloch sphere?
```python
  qc = QuantumCircuit(1,1)
  # Insert code fragment here
  simulator = Aer.get_backend('statevector_simulator')  
  job = execute(qc, simulator)  
  result = job.result()  
  outputstate = result.get_statevector(qc)  
  plot_bloch_multivector(outputstate)
```
![](2023-06-07-21-16-09.png)

[A]  
qc.h(0)  

[B]  
qc.rx(math.pi / 2, 0)  

[C]  
qc.ry(math.pi / 2, 0)  

[D]  
qc.rx(math.pi / 2, 0)  
qc.rz(-math.pi / 2, 0)  

[E]  
qc.ry(math.pi, 0)


## 答え
[A], [C]

## 解説
与えられた操作後，X軸上倒れたベクトルになっている。
初期状態は，$\ket{\Psi_{0}} = \ket{0}$であり，ブロッホ球は  
![](2023-06-07-21-27-54.png)
と描かれる。
この初期状態をY軸方向に$\frac{\pi}{2}$回転させる，問題文で与えられた操作後の状態ベクトルになることがわかる。これは選択肢で言うと[C]に相当する。また，初期状態$\ket{0}$をアダマール変換された状態と同等であり，選択肢[A]に相当する。

ちなみに，選択肢[C]は
![](2023-06-07-21-35-59.png)
となる。

# Q7.

> S-gate is a Qiskit phase gate with what value of the phase parameter?

[A]  
π/4  

[B]  
π/2  

[C]  
π/8  

[D]  
π

## 答え
[B]

