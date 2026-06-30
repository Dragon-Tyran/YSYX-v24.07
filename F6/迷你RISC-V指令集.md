
### 1.PC寄存器的位宽为多少？

**Ans:** 32bits

**参考**
![[Pasted image 20260630140309.png]]


### 2.GPR共有多少个? 每个GPR的位宽是多少?

**Ans:** 32个，X0-X31

### 3.R[0]和sISA的R[0]有什么不同之处?


**Ans:**  For RV32I Register x0 is hardwired with all bits equal to 0 , sISA的R[0] 是普通寄存器

### 4. 指令编码的位宽是多少? 指令有多少种基本格式?

**Ans:** 32位宽，4种基本格式（R/I/S/U）

![[Pasted image 20260630141625.png]]
### 5. 在指令的基本格式中, 需要多少位来表示一个GPR? 为什么?

**Ans:** RV32I 共有 32 个通用寄存器，而 $2^5 = 32$

### 6.`add`指令的格式具体是什么?

**Ans:**
![[Pasted image 20260630142244.png]]

### 7.还有一种基础指令集称为RV32E, 它和RV32I有什么不同?


**Ans:** RV32E is designed for microcontrollers in embedded systems with the number of integer registers reduced to 16
![[Pasted image 20260630142652.png]]