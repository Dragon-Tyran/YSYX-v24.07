
**<font size="4"> Q1：查阅RISC-V手册, 找到`addi`指令的编码和相应的功能描述. 在第34章`RV32/64G Instruction Set Listings`中有一些指令表, 可以帮助你查阅`addi`指令的编码</font>**

 >**ADDI 指令：**
 >![](attachments/Pasted%20image%2020260630143808.png)
 >![](attachments/Pasted%20image%2020260630144148.png)


**<font size="4">Q2:为了了解RISC-V对存储器的若干约定, 你需要阅读RISC-V手册第1.4节的第一段, 从ISA的层面了解存储器的规格, 尤其是宽度的定义.</font>**
> 一个 RISC-V hart 拥有单一的字节寻址地址空间，用于所有内存访问，一个字是4字节（4bytes），也就是32bits。所以64bits就是8字节，也就是2个字


**<font size="4">Q3:查阅RISC-V手册, 找到`jalr`指令的编码和相应的功能描述.</font>**

> ![](attachments/Pasted%20image%2020260630145324.png)
> ![](attachments/Pasted%20image%2020260630145441.png)



```bash
00000000 <_start>:
   0:	01400513          	addi	a0,zero,20
   4:	010000e7          	jalr	ra,16(zero) # 10 <fun>
   8:	00c000e7          	jalr	ra,12(zero) # c <halt>

0000000c <halt>:
   c:	00c00067          	jalr	zero,12(zero) # c <halt>

00000010 <fun>:
  10:	00a50513          	addi	a0,a0,10
  14:	00008067          	jalr	zero,0(ra)
```

