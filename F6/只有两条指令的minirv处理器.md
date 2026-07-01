
### **<font size="4"> Q1：查阅RISC-V手册, 找到`addi`指令的编码和相应的功能描述. 在第34章`RV32/64G Instruction Set Listings`中有一些指令表, 可以帮助你查阅`addi`指令的编码</font>**

 >**ADDI 指令：**
 >![](attachments/Pasted%20image%2020260630143808.png)
 >![](attachments/Pasted%20image%2020260630144148.png)


### **<font size="4">Q2:为了了解RISC-V对存储器的若干约定, 你需要阅读RISC-V手册第1.4节的第一段, 从ISA的层面了解存储器的规格, 尤其是宽度的定义.</font>**
> 一个 RISC-V hart 拥有单一的字节寻址地址空间，用于所有内存访问，一个字是4字节（4bytes），也就是32bits。所以64bits就是8字节，也就是2个字


### **<font size="4">Q3:查阅RISC-V手册, 找到`jalr`指令的编码和相应的功能描述.</font>**

> ![](attachments/Pasted%20image%2020260630145324.png)
> ![](attachments/Pasted%20image%2020260630145441.png)


#### jalr编码格式
```
31          20 19    15 14  12 11     7 6      0
[ imm[11:0]  ][  rs1  ][funct3][  rd  ][opcode]
     12          5        3       5       7
offset[11:0]   base     0      dest    JALR
```

- **opcode**：固定为 JALR 专用编码
- **funct3**：固定为 `000`
- **rs1（base）**：基址寄存器
- **imm[11:0]**：12 位有符号立即数偏移
- **rd（dest）**：保存返回地址的目标寄存器

##### 执行原理，分两步

**第一步：计算跳转目标地址**

```
target = (rs1 + sign_extend(imm)) & ~1
```

- 把 12 位立即数**符号扩展**到 32/64 位
- 与 `rs1` 寄存器的值相加，得到候选地址
- 将结果的**最低位强制清零**（保证跳转地址是偶数、对齐到 2 字节边界）

**第二步：保存返回地址并跳转**

```
rd = pc + 4
pc = target
```

- 把当前指令的下一条指令地址（`pc+4`）写入 `rd`
- 然后 PC 跳转到上一步算出的 `target`


### 测试

- PC是按字存储的，反汇编工具显示的 `<fun>` ,`<halt>`标签是 `00000010`,`0000000c`，这是**字节地址**的写法（RISC-V 手册的标准约定）。这两者之间正好差 4 倍（一条指令 4 字节）

```bash
PC 00000000 <_start>:                                                 
0  0:	01400513          	addi	a0,zero,20                     
1  4:	010000e7          	jalr	ra,16(zero) # 10 <fun>         
2  8:	00c000e7          	jalr	ra,12(zero) # c <halt>          

 0000000c <halt>:
3 c:	00c00067          	jalr	zero,12(zero) # c <halt>

 00000010 <fun>:
4  10:	00a50513          	addi	a0,a0,10
5  14:	00008067          	jalr	zero,0(ra)
```

```
01400513 -> |0000 0001 0100| 0000 0|000 |0101 0|001 0011 
               imm             rs1  funt3  rd     opcode
               20             zero         a0
               
010000e7 -> |0000 0001 0000| 0000 0|000 |0000 1|110 0111
                imm            rs1  funt3  rd     opcode
                16             zero        ra    
00c000e7 -> |0000 0000 1100| 0000 0|000 |0000 1|110 0111
                imm            rs1  funt3  rd     opcode
                12            zero         ra 
0000000c <halt>:                
00c00067 -> |0000 0000 1100| 0000 0|000 |0000 0|110 0111
                imm            rs1  funt3  rd     opcode
                12            zero         ra 
00000010 <fun>:
00a50513 -> |0000 0000 1010| 0101 0|000 |0101 0|001 0011
                imm            rs1  funt3  rd     opcode
                10            a0           a0  
00008067 -> |0000 0000 0000| 0000 1|000 |0000 0|110 0111              
                imm            rs1  funt3  rd     opcode 
                0               ra         zero
 ```
