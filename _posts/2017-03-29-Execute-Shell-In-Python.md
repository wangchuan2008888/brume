> 在python中运行shell命令的方法有很多，这里列举几个

[TOC]

## 使用os模块
os 模块中有两个命令可以与系统的shell进行交互：
* os.system() 用于向系统中发送命令，返回执行返回值，linux中一般以返回0当作执行成功。
例如
```python
import os
os.system("type afa.txt")
```
```
结果：256
```

```python
import os
os.system("ls")
```
```
结果：0
```
* os.popen() 可以连接shell的标准输入/输出流，其中默认连接的是标准输出流，当向popen传递w模式时，与标准输入流连接。
+ 执行shell命令
```python
import os
for line in os.popen("ls "):
    print line
```
```    
输出结果:
1612.02284.pdf
anaconda2
apps
bareos
bareos-db.odb
```
+ 写入标准输入流
```python
import os
stdin = os.popen('cat >test.txt2',"w")
stdin.write("test.txt")
stdin.close()
```
将会在当前目录下生成一个位test.txt2内容位test.txt的文件
## subprocess模块
subprocess模块
## commands 模块
commands 模块使用较为简单，可以获取输出的状态、标准输出、状态和标准输出结果一起获取三个API。
