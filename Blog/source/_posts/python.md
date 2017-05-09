# python碎碎念

## 基础
### 中文编码问题编码
```python
# !/usr/bom/env python
# coding:utf8 

# or 
import sys 
reload(sys)
sys.setdefaultencoding('utf8')
```

### 基础知识
变量： 弱类型 无需声明
Number
String 拼接a+b 长度len(a) 切片c[0:2] 
List [] 添加append 长度 切片 删除del a[0] 
Tuple read only  类似List
Dictionary a={'key':1}  a.has_key('key')
set 不能序列化 可以转成list再进行序列化

条件：
if...
if…else...
if…elif…else

循环：
while
for
```python
#for遍历list和dict
for option in list
for key,value in dict
```
循环控制：
break 跳出整个循环
continue 结束本轮循环继续下轮
pass 

时间：
```python
import time 

print time.time()

print time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) 

```

文件：
```python 
fw = open('data.txt','w')
for obj in xrange(1,10):
	fw.write(str(obj))
fw.close()

fr = open('data.txt','r')
for line in fr:
	print line 
fr.close()
```

异常：
```python
try:
<语句>        #运行别的代码
except <名字>：
<语句>        #如果在try部份引发了异常
else:
<语句>        #如果try里没有异常发生
finally:	  #一定会执行
<语句>	
```
函数 def：
```python 
def hello():
	print 'Hello'
hello()
```

## 爬虫

url： 协议 域名/ip 端口 路由(/college/scorelist 二级) 参数?后的内容
http://kaoshi.edu.sina.com.cn/college/scorelist?tab=batch&wl=1&local=2&batch=&syear=2013

ping www.baidu.com 
可以获取ip 测试网络通不通

找到汇总页面 下钻到详情页 

[全栈数据工程师养成攻略](https://github.com/Honlan/fullstack-data-engineer)
[笔记](http://note.youdao.com/share/?id=2155cf875395e84d92ef80baeae7c3c0&type=notebook#/)

## debug
测试某个函数

```
python manage.py shell
from dbinabox.dbaas.component.application import Pool
p= Pool('5368dc29e4b0635f5fb8d664')
p.get_pool_build_information()
```
## test

### -s 打印出被测试函数里的print
```python 
py.test -s  test/test_comp/test_application.py::test_get_dal_attrs
```

### --pdb 调试

```bash
/home/zhifan/db-in-box/' && py.test --pdb -v -s --fulltrace /home/zhifan/db-in-box/DBInBoxWeb/dbinabox/dbaas/test/test_database/test_mongo/test_t.py::test_a

bt # 查看调用堆栈
up # 上移动 前往想查看的错误地
```

[pdb](https://docs.python.org/3.2/library/pdb.html)
[python pdb调试](http://www.cnblogs.com/chencheng/p/3161778.html)