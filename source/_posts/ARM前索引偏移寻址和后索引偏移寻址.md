---
title: ARM前索引偏移寻址和后索引偏移寻址
tags:
  - ARM
categories:
  - - ARM
  - - 所有
index_img: 'https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/arm.png'
date: 2022-04-01 11:48:31
---

![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401124111357.png)

### 寻址模式2  -加载和存储字或无符号字节

有九种寻址模式用于计算Load  and Store 字或无符号字节指令的地址。一般的指令语法是:

```assembly
LDR | STR{<cond>} {B} {T} <Rd>, <addressing_mode>
```

其中<addressing_mode>是下面列出的九个选项之一：

LDR、LDRB、STR和STRB均可使用以下9个选项。对于LDRBT、LDRT、STRBT和STRBT，只有post-indexed（后索引）选项（列表中的最后三个）可用。对于A10-14页的PLD中描述的PLD指令，只有 offset （偏移）选项（列表中的前三个）可用。

1. `[<Rn>, #+/-<offset_12>]`
Immediate offset
2. `[<Rn>, +/-<Rm>]`
Register offset
3. `[<Rn>, +/-<Rm>, <shift> #<shift_imm>]`
Scaled register offset
4. `[<Rn>, #+/-<offset_12>]!`
Immediate pre-indexed
5. `[<Rn>, +/-<Rm>]!`
Register pre-indexed
6. `[<Rn>, +/-<Rm>, <shift> #<shift_imm>]!`
Scaled register pre-indexed
7. `[<Rn>], #+/-<offset_12>`
Immediate post-indexed
8. `[<Rn>], +/-<Rm>`
Register post-indexed
9. `[<Rn>], +/-<Rm>, <shift> #<shift_imm>`
Scaled register post-indexed

### 前索引寻址

以Immediate pre-indexed举例：

![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401124224562.png)

This addressing mode calculates an address by adding or subtracting the value of an immediate offset to or from the value of the base register Rn.
If the condition specified in the instruction matches the condition code status, the calculated address is 
written back to the base register Rn.

这种寻址模式通过对基寄存器Rn的值加或减一个立即偏移量的值来计算地址。如果指令中指定的条件与条件代码状态匹配，则计算出的地址被写回基寄存器Rn。

![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401124928200.png)

```assembly
if U == 1 then 
    address = Rn + offset_12
else /* if U == 0 */
    address = Rn - offset_12
if ConditionPassed(cond) then
    Rn = address
```

这种寻址模式用于指针访问数组，并自动更新指针值。（类似于a[++i]）

### 后索引寻址

以Immediate post-indexed举例：

![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401125139620.png)

This addressing mode uses the value of the base register Rn as the address for the memory access.
If the condition specified in the instruction matches the condition code status, the value of the immediate offset is added to or subtracted from the value of the base register Rn and written back to the base register Rn.

这种寻址模式使用基寄存器Rn的值作为内存访问的地址。如果指令中指定的条件与条件代码状态相匹配，则立即偏移量的值将与基寄存器Rn的值相加或相减，并写入基寄存器Rn。

![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401125336156.png)

```assembly
address = Rn
if ConditionPassed(cond) then
    if U == 1 then 
        Rn = Rn + offset_12
    else /* U == 0 */
        Rn = Rn - offset_12
```

这种寻址模式用于指针访问数组，并自动更新指针值。（类似于a[i++]）