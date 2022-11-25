# C++ primer

## 第一部分：C++ 基础
### 第二章-变量和基本类型
#### 基本内置类型
* 种类
  * 整型
  * 浮点
  * 字符
  * 布尔
  * 空型
* 类型转换
  * 非bool->bool: 0为false，否则为 true
  * bool->非bool: false为0, true为 1
  * float->int: 截取整数部分
  * int->float: 小数部分为0
  * 大数->unsigned: 截取整数部分
  * 大数->signed: 未定义
  * 切勿混用signed和unsigned
* 字面值常量 
  * 整型和浮点型字面值常量-0开头表八进制，0x开头表16进制
  * 字符和字符串字面值常量-单引号表字符，双引号表字符串，
    * 两字符串紧邻,为同一字符串。
  * 转义序列
    * 普通-\r
    * 八进制-\12, o为数字, 长度为3
    * 十六进制-\x12, 长度为4
  * 指定字面量类型
    * L'a': wchar_t
    * u8"hi!": utf-8
    * 42ULL: unsigned long long
    * 1E-3F: float
    * 3.14159L: long double
#### 变量
