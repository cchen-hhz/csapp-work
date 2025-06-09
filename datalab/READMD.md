# DataLab

想起了某年 thuwc 。

## bitXor

使用 `&` 和 `~` 完成异或运算 `^` 。

使用 `~(~x & ~y)` 可以计算出 $x$ 和 $y$ 的或运算，此时需要把 `1^1` 的情况处理了，设定一个 `mask=~(x&y)` 就能使得除了 `1 1` 其余全 `1` 的位置，再与起来就行。


## tmin

32位补码表示的最小负数 $1000\cdots$ 

`1<<31`

## isTmax

判断 x 是不是32位补码表示的最大值 $0111\cdots$ ，不能使用位运算。

我的做法比较麻烦，考虑计算 `x^(x+1)` 在 x 是 Tmin 或 Tmax 时该值为 -1 ($1111\cdots$) ，对其取反再取非就能唯一判断这个值。

排除 Tmin 考虑 Tmin + 1 得到 0 ，设一个位面特判一下。

## allOddBits

判断一个数的所有奇数为是否都为 $1$ 。

先以 `0xAA` (0101010101) 为基础，构造出奇数位全是 $1$ 的数字，结果为判断 $ x\&sum==sum $ ，两个值异或一下即可判断。

## negate

实现 `-x` 的功能。

`(~x)+1` ，注意 Tmin 会溢出到 Tmin ，是正确的。

## isAsciiDigit

判断 `0x30 <= x <= 0x39` 。

比较丑陋：

抓住只有最后一位不同的性质，先检查前 7 位是否完全一致，把最后一位抠出来，由于判断区间从 $0$ 开始，可以简单的判断末尾值加 6 是否会导致进位，表现为第 $4$ 位（从 $0$ 标号）是否是 $1$ 。

## conditional

完成三目运算符 `x ? y : z` 。

先判断 x 是不是 0 ，得到 `0/1` 。

设定一个 mask 为 `111...` ，则 `mask+1=0,mask+0=mask`，以此得到对 y 和 z 的掩码，最掩码与值加起来就行。

## isLessOrEqual

判断 `x<=y` 。

计算 y+ (-x) 是否是负数（判断 31 位是否为 1）。

在 x 是 Tmin 时有问题，单独特判出来。

## LogicalNeg

实现逻辑非 `!` 。

一个观察是若 x 不是 0 那么 x 和 -x 的符号位至少有一个是 1，这对于 Tmin 也是成立的（Tmin 溢出还是 Tmin）。

## howManyBits

x 至少需要多少个位来表示，在补码表示下。

先将 x 表示成正数，若负数则取反。

现在要确定最大的 1 的位置，采用二分法二分出这个位置，最后加一位符号位。

## floatScale2

给定一个单精度小数的 32 位表示，求其乘 2 后的小数表示。

浮点数计算的限制放缓了许多，把符号位，指数为，小数位全部分离出来，最普遍的情况是指数位加一。

还要特判不规范的情况，此时需要把小数位左移一位，若小数位以 1 打头，指数位还要改成 1 (0x01) 。

## floatFloat2Int

给定 32 位小数的 unsigned 表示，输出其 int 表示，并特判超出 int 范围的值

若指数位为 0xff 则为超出范围。

否则若指数位为 0，由于小数位以 0 开头，答案就是 0。

否则得到真实的指数位 $exp - 127$ ，这个值小于 0 则答案是 0 ，这个值过大 (大于 $31$ )特判出来，否则直接计算即可，注意加上符号位。 

## floatPower2

最后一题

输出 $2.0^x$ 的 32 位小数表示，若是不标准型则输出 0，超出限制输出 +INF 。

实时只有指数位需要改变，注意指数表示是 $base+127$ 的结果。


## dlc 数据

```plain
dlc:bits.c:149:bitXor: 7 operators
dlc:bits.c:160:tmin: 1 operators
dlc:bits.c:174:isTmax: 7 operators
dlc:bits.c:188:allOddBits: 7 operators
dlc:bits.c:198:negate: 2 operators
dlc:bits.c:216:isAsciiDigit: 10 operators
dlc:bits.c:228:conditional: 7 operators
dlc:bits.c:242:isLessOrEqual: 10 operators
dlc:bits.c:257:logicalNeg: 9 operators
dlc:bits.c:280:howManyBits: 35 operators
dlc:bits.c:310:floatScale2: 25 operators
dlc:bits.c:341:floatFloat2Int: 21 operators
dlc:bits.c:362:floatPower2: 5 operators
```




