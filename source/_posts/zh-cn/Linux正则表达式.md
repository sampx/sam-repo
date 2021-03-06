---
title: Linux正则表达式
date: 2017-04-09 17:29:23
categories: 技术笔记
tags:
- Linux
- 正则表达式
comments: false
---

# 一、基础正则表达式

## 1、`.` 点符号
匹配除换行符之外的的任意一个字符
例如：r.t 匹配 rot ， rut等
r..t 匹配 root, ruut, rt(中间两个空格)

## 2、`*` 星符号
匹配前一个字符0次或任意多次
例如：ab* 匹配 a，ab，abb等
      `.*` 代表任意长度的不包含换换的字符
注意：***`g*d` g可能匹配0项***      

## 3、`\{n,m\}` 符号
精确匹配前一个字符的重复次数
`\{n\}` 匹配前面的字符n次
`\{n,\}` 匹配前面的字符n次以上,含n次
`\{n,m\}` 匹配前面的字符n到m次
注意：***egrep下，不能输入“\”符号***

## 4、`^` 符号
匹配开头字符
例如：`^root` 匹配以root开头的行

## 5、`$` 符号
匹配行尾

## 6、`[]` 符号
匹配方括号内出现的任一字符，如果范围比较大，可以使用‘-’号进行范围限定，如[A-Z]匹配任意大写字符,[A-Za-z0-9]匹配任意字符和数字。
'[]'内出现'^'符号代表取反,代表不是

## 7、`\` 符号
转义字符
例如：`\.*` 代表匹配任意长度的'.',而不是任意字符

## 8、`\<` 和 `\>` 符号
分别界定单词的左边和右边
例如:`\<hello\>`精确匹配hello,而不是helloworld

## 9、`\d` 符号
匹配一个数字,等价于`[0-9]`
注意:`\d`有时有兼容性问题,如:`echo 123 | grep '\d'`
匹配不成功是因为`\d`是一种Perl兼容模式的表达式,这种情况下要加上`-P`参数:`echo 123 | grep -P '\d'`

## 10、`\b` 符号
匹配单词边界，等价于`\<\>`

## 11、`\B` 符号
匹配非单词的边界
例如:`hello\B` 匹配 helloworld,即hello后必须有字符

## 12、`\w` 符号
匹配字母,数字和下划线,等价于`[A-Za-z0-9]`
注意：比后者多匹配下划线

## 13、`\W` 符号
匹配非字母,数字和下划线
如：`@$%*^~\&#{}[]()<>+-/`等

## 14、`\n` 符号
匹配一个换换符

## 15、`\r` 符号
匹配一个回车符

## 16、`\t` 符号
匹配一个制表符

## 17、`\f` 符号
匹配一个换页符

## 18、`\s` 符号
匹配任何空白字符

## 19、`\S` 符号
匹配任何非空白字符
例如：`^\b\S+\b\s+\b\S+\b\s*$` 精确匹配两个单词（egrep）

# 二、扩展的正则表达式

以下这些必须要用egrep执行

## 1、`?` 符号
匹配前一个字符0次或1次

## 2、`+` 符号
匹配前一个字符1次以上

## 3、`|` 符号
“或”的意思，多种可能的罗列，彼此间是分支关系
例如：匹配不同的固定电话区号：
`^0[0-9]\{2,3\}-[0-9]\{8\}`
也可以这样写:`^0[0-9]\{2\}-[0-9]\{8\}|^0[0-9]\{3\}-[0-9]\{8\}`
    021-38749087
    0556-34538979
    010 66508749
    01066508749

## 4、`()` 符号
通常与`|`联合使用，用于枚举一系列可替换的字符。
例如：电话区号可用“-”或“ ”空格来表示：
`^0[0-9]{2,3}(-| |.*)[0-9]{8}`

# 三、通配符

通配符和正则表达式之间存在的一些差异，有些相同的字符既用在正则表达式中又用在通配符中，极易造成混淆和干扰，其中的区别要靠多记忆和练习去加深理解，简单的说，正则表达式主要用在对文件内容的匹配上，通配符主要用在对文件名的匹配上。

## 1、`*` 符号
代表0或多个字符
例如：`ls -l *.doc`
匹配扩展名为doc的任意文件

## 2、`?` 符号
代表任意一个字符
例如:`ls -l A?.doc` 匹配扩展名为doc,名称为A开头,后代一个字符的任意文件

## 3、`{}` 和 `[]` 符号
匹配所有括号内包含的以逗号隔开的字符
例如:`ls -l {a,b,c}.doc`
匹配名字是字母a,b,c,扩展名为doc的文件
再如:`ls -l [a-c].doc` 与上例意义相同
综合例子：`ls -l {a*,b*,c*}.{tmp,txt}`

## 4、`^`符号和`!`符号
这两个符号往往和`[]`一起使用,当出现在`[]`中时,代表取反
例如:`[^A]`或`[!A]`代表不是A

# 四、grep支持的POSIX字符

* `[:alnum:]` 文字数字字符
* `[:alpha:]` 文字字符
* `[:digit:]` 数字字符
* `[:graph:]` 非空字符(非空格,控制字符)
* `[:lower:]` 小写字母
* `[:upper:]` 大写字母
* `[:cntrl:]` 控制字符
* `[:print:]` 非空字符(包含空格)
* `[:punct:]` 标点符号
* `[:space:]` 所有空白字符(新行,空格,制表符)
* `[:xdigit:]` 十六进制数字(0-9,a-f)

例子：`grep ^[[:upper:]] greptest.txt`  
    The cat's name is Tom, what's the mouse's name?
    The mouse's NAME is Jerry
    They are good friends.
    Upper case
`grep ^[[:xdigit:]] greptest.txt`
    100% means pure
    021-38749087
    0556-34538979
    010 66508749
    01066508749
