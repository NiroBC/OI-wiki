位运算就是把整数转换为二进制后，每位进行相应的运算得到结果。

常用的运算符共5 种

分别为 与（&），或（|），异或（^），左移(<<) 和 右移(>>)。

## 与，或，异或
与（&）或（|）和异或（^）这三者都是两者间的运算，因此在这里一起讲解

表示把2个整数分别转换为二进制后各位逐一比较

&  只有在两个（对应位数中）都为1时才为1

|    只要在两个（对应位数中）有一个1时就为1

^    只有两个（对应位数）不同时才为1

> 举例

![TIM截图20180817234032.png](https://i.loli.net/2018/08/17/5b76eca93d3b7.png)

## 左移和右移
与前面的3种运算相似，这两种运算仍是把整数转换为二进制后进行操作

左移（<<） 将转化为二进制后的数字整体向左移动
> num<<i  //表示将num转换为二进制后向左移动i位（所得的值）

右移（>>） 将转化为二进制后的数字整体向右移动
> num>>i  //表示将num转换为二进制后向左移动i位（所得的值）

> 举例

![TIM截图20180817235452.png](https://i.loli.net/2018/08/17/5b76efdd216fe.png)

* 如上图，右移操作中末尾多余的“1”将会被舍弃

* 注意，左移和右移是有返回值的，并非对num本身进行操作

***

## 位运算的应用

> num<<i相当于num乘以2的i次方，而num>>i相当于num/2 的i次方(位运算比"%"和"/"操作快得多

> num*10=(num<<1)+(num<<3)