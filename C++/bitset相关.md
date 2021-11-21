## `bitset`特性简介

- `bitset`是一个`bool`数组，空间复杂度上来说，会比同等长度的`bool`数组少一点

- 可以单独访问某个`bit`位，就跟使用数组一样，**注意索引是从后往前的**

  - 举例来说⬇️

    ```c++
    bs:    1 0 1 0 1
    index: 4 3 2 1 0
    ```

- 编译的时候`bitset`的大小就已经确定下来了，不能在运行的时候更改，所以我们在创建`bitset`的时候要用**常量**来初始化

## `bitset`的常用函数

- 常见初始化方式

  ```c++
  bitset<4> bs1(15);
  cout << bs1 << endl;     // 输出15的二进制表示——1111
  
  bitset<4> bs2(string("0101"));
  cout << bs2 << endl;     // 输出我们用于初始化的比特串
  ```

- 不同的`bitset`之间可以方便地位运算

  ```c++
  cout << (bs1 & bs2) << endl;    // 1111 & 0101 = 0101
  ```

- `bitset`可以方便地输出为其他的格式

  ```c++
  cout << bs2.to_string('Y', 'N') << endl;    // 0101 -> NYNY
  cout << bs2.to_ulong() << endl;     // 0101 -> 5
  ```

- 对`bitset`进行计数

  ```c++
  cout << bs2.count() << endl;    // 输出比特位一共有几个1，“0101”输出「2」
  cout << bs2.size() << endl;     // 输出长度，“0101”输出「4」
  ```

- 检查`bitset`的每一位

  ```c++
  cout << bs2.any() << endl;      // 检查是否有任意一个比特位为1，输出「1」
  cout << bs2.all() << endl;      // 检查是否所有比特位都为1，输出「0」
  cout << bs2.none() << endl;     // 检查是否所有比特位都不为1，输出「0」
  ```

- 修改`bitset`

  ```c++
  bs2.flip();             // 输出比特位的翻转
  cout << bs2 << endl;    // 输出「1010」
  
  bs2.set();              // 所有比特位设置为1
  cout << bs2 << endl;    // 输出「1111」
  
  bs2.reset();            // 所有比特位设置为0
  cout << bs2 << endl;    // 输出「0000」
  ```

  

