## Q1 

> Which statement will create a quantum circuit with four quantum bits and four classical bits?
>> A. QuantumCircuit(4, 4)  
 B. QuantumCircuit(4)  
 C. QuantumCircuit(QuantumRegister(4, 'qr0'), QuantumRegister(4, 'cr1'))  
 D. QuantumCircuit([4, 4])

## 答え
A

## 解説
問題文は４つの量子ビットと4つの古典ビットを作るCode分を求められている。
量子ビットと古典ビットを準備する最も簡単な方法がAである。

また，選択肢にはないが下記のように　量子，古典レジスタを使って回路を構成しても良い。
```
QuantumCircuit(QuantumRegister(4,"q"), ClassicalRegister(4, "c"))
```

##　他の選択肢の解説
- B  
因数を1つにする場合，その数の量子ビットが作られるため 4つの量子ビットを持つ回路となる。
- C  
名称指定されているqr0 量子ビットレジスタが4, cr1量子ビットが4つの合計8つの量子ビットが準備された回路となる。
- D
数値を入力したリストは受け付けていない




# Q2. 