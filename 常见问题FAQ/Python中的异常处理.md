---
title: Python中的异常处理
description: 
published: true
date: 2022-11-23T03:22:48.471Z
tags: 
editor: markdown
dateCreated: 2022-11-23T03:22:48.471Z
---

# python中的异常处理
## 1、异常处理

​     在程序运行时，如果代码引发了错误，python就会中断程序，并输出错误提示，这样的问题在编写代码的过程中经常碰到当然这类问题也可以避免。但是在实际的应用中，很多错误是编写代码的人员无法控制的，比如用户输入不合规定的数据或者需要打开的文件不存在，这些情况被称作“异常”。      

​      一个好的程序需要能处理可能发生的异常，避免程序因此中断。即使程序执行过程中使出现异常，程序也能正常的执行下去，这种情况下就需要用到python中的异常处理：

```python
示例：
while True:
    num = input('input a number')
    print(1000/int(num))
```

​      以上，输入0会导致当前程序的异常退出，但正常情况，我们是希望程序不要退出，而是提示用户输入错误，让用户重新输入数字，继续执行。

### 1.1 常见的python异常类型

| BaseException     | 所有异常的基类                                               |
| ----------------- | ------------------------------------------------------------ |
| Exception         | 常规错误的基类                                               |
| AttributeError    | 特性引用或赋值失败是引发                                     |
| IOError           | 试图打开不存在文件时引发                                     |
| IndexError        | 在使用系列中不存在的索引时引发                               |
| KeyError          | 映射中没有这个键                                             |
| NameError         | 在找不到名字时引发（未声明/初始化对象）                      |
| SyntaxError       | 在代码为错误形式时引发                                       |
| TypeError         | 函数或方法接受了不适当类型的参数，如sum('nick')，sum函数不接受字符串类型 |
| ValueError        | 函数或方法接受了正确类型的参数，但该参数的值不适当，比如int('nick') |
| ZeroDivisionError | 在除法或者模除操作的第二个参数为0时引发                      |
| ImportError       | 无法引入模块或者包，基本是路径问题                           |
| Warning           | 警告的基类                                                   |

##  2、异常捕获

### 2.1 捕获所有异常

​     在python中使try...except语句来处理异常，具体方法是将可能引发异常的语句放到try块中执行，当发生异常时，跳过try块中剩余的语句，直接执行except中的语句：

```python
示例：
while True:
    num = input('input a number')
    try:
        print(1000/int(num))
    except:
        print('输入错误，请重新输入')
----------------------------------------------------------------------------------------------------------------------
    程序执行结果：
    input a number 0
    输入错误，请重新输入       # 除以0的异常被try...except捕获，并执行except里面的代码
    input a number 5
    200.0
```

```python
实例：
def find_image(widget, rate=0.9, num=3):
    # 在屏幕中区寻找小图，返回坐标，
    # 如果找不到，再匹配3次，每次间隔1秒
    try:
        for i in range(num):  
              # 图像识别，匹配小图在屏幕中的坐标x, y
            locate = match_image_by_opencv(widget, rate)    
            logger.info(f"{widget}的坐标是{locate}")
            if not locate:
                sleep(1)
            else:
                return locate
        return False
    except:
        print('图片资源：data/picture_source/{}.png 文件不存在!'.format(widget))
        
if __name__ == '__main__':       
    find_image('screen11')
    find_image('screen')
--------------------------------------------------------------------------------------------------------------------
    程序执行结果：
    图片资源：data/picture_source/screen11.png 文件不存在!
-----------------------------------------------------------------------------
    2021-09-02 18:03:51,079 - test - test.py[line:160] - INFO - screen的坐标是(960, 540)
```

​      ◆  try代码块指明作用域

​      ◆  except 代码块是异常处理的代码



### 2.2 捕获指定异常

except语句也可以专门处理指定的异常，在except语句后跟异常名称，如果不指定异常名称则表示处理所有异常；

```Python
示例：
try:
    print('请输入一个数字：',end='')
    num = int(input())
    num += 1
    print(num)
except ValueError:     # 指定异常类名
    print('输入错误，只能输入数字')   # 异常说明
--------------------------------------------------------------------------------------------------------------------
    程序执行结果：
    请输入一个数字：y
    输入错误，只能输入数字
```

```python
实例：
def click_main_menu_exit():
    # 点击顶部菜单的退出按钮
    try:
          # 匹配图片album/main_menu_exit
        position = find_img('album/main_menu_exit')     
        mk.click(*position)     
          # 若找不到匹配的文件，则抛出此异常
    except TypeError:       
        print('找不到图片')    

if __name__ == '__main__':        
    click_main_menu_exit()    
 ---------------------------------------------------------------------------------------------------------------------
	程序执行结果：
    找不到图片 album/main_menu_exit
    找不到图片
```

### 2.3 捕获多个异常

​      ◆  except同时处理多个异常：

```python
示例：
try:
    print('请输入一个数字：', end='')
    num = int(input())
    num =1 / num
    print(num)
except ValueError:    
    print('输入错误，只能输入数字')
except ZeroDivisionError:   
    print('除数不能为0')
--------------------------------------------------------------------------------------------------------------------
    程序执行结果：
    请输入一个数字：y
    输入错误，只能输入数字
    请输入一个数字：0
    除数不能为0
```

```python
实例：
def click_main_menu_exit():
    # 点击顶部菜单的退出按钮
    try:
         # 匹配图片album/main_menu_exit
        position = find_img('album/main_menu_exit')
        mk.click(*position)
    except TypeError:      
        print('111')
    except ValueError:
        print('222')
        
if __name__ == '__main__':        
    click_main_menu_exit()  
 ------------------------------------------------------------------------------------------------------------------
   程序执行结果：
   找不到图片 album/main_menu_exit
   111          # 捕获到TypeError异常
```

### 2.4 打印异常详细信息

```python
示例：
try:
    ohmy
except NameError as e:
    print('handle NameError:',e) 
-------------------------------------------------------------------------------------------------------------------
  程序执行结果：
  handle NameErro: name 'ohmy' is not defined   # ‘ohmy’未定义
```

```python
实例：
def click_album_name_delete_btn():
    """
    点击相册按钮的删除按钮
    :return:
    """
    try:
        x, y = find_img('album/album_021_3')
        mk.click(x, y)
    except TypeError as e:
        print(f"TypeError:{e}")

click_album_name_delete_btn()
-------------------------------------------------------------------------------------------------------------------
   程序执行结果：
   找不到图片 album/album_021_3  
   TypeError:cannot unpack non-iterable NoneType object   # 无法解压缩不可迭代的noneType对象
```

​      ◆  e就是异常对象，可以打印出里面存储的具体错误信息。

### 2.5 异常中的else

​     ◆  try...except 语句，也可以结合else来实现没有发生的场景：

```python
示例：
try:
    num = int(input('请输入一个数字:'))
    print(1000/num)
    print('abc')
except ZeroDivisionError:
    print('除数不能为0')
except ValueError:
    print('字符类型转换有误')
except:
    print('其他错误')
else:                 # try语句中没有错误则执行此段代码
    print('没有错误')    
-----------------------------------------------------------------------------------------------------------------------
   程序执行结果：
   请输入一个数字:1
   1000.0
   abc
   没有错误
```

```python
实例：
def open_image_viewer():
    # 打开看图
    try:
        pid1 = os.popen('pidof deepin-image-viewer').read()    
        # 初始化环境输入法
        os.system("fcitx -r")
        if pid1 == "":
             os.popen("deepin-image-viewer")
        else:
            os.popen("kill -9 $(pidof deepin-image-viewer)")
            os.popen("deepin-image-viewer")
    except Exception as e:
        logging.error(e)
    else:  
        print('结束')

if __name__ == '__main__':
    open_image_viewer()
-----------------------------------------------------------------------------------------------------------------------
    程序执行结果:
    结束         
```

### 2.6 异常中的finally

​      ◆   try...finally...语句，表示无论是否发生异常都需要执行最后的代码

```python
示例：
try:
    num = int(input('请输入一个数字：'))
    print(1000/num)
    print('abc')
except ZeroDivisionError:
    print('除数不能为0')
except ValueError:
    print('字符类型转换有误')
except:
    print('其他错误')
else:
    print('没有错误')
finally:            # 无论异常与否都会执行此段代码
    print('结束')     
------------------------------------------------------------------------------------------------------------------
    程序执行结果：
    请输入一个数字：y
    字符类型转换有误
    结束
--------------------------
   请输入一个数字：10
   100.0
   abc
   没有错误
   结束
```

```python
实例：
def find_image(widget, rate=0.9, num=3):
    # 在屏幕中区寻找小图，返回坐标，
    # 如果找不到，再匹配3次，每次间隔1秒
    try:
        for i in range(num):
            locate = match_image_by_opencv(widget, rate)
            logger.info(f"{widget}的坐标是{locate}")
            if not locate:
                sleep(1)
            else:
                return locate
        return False
    except:
        print('图片资源：data/picture_source/{}.png 文件不存在!'.format(widget))
    finally:                # 无论异常与否，最后都会执行此段代码
        print('结束')
if __name__ == '__main__':
    find_image('screen111')     # 输入不存在的图片名称
    find_image('screen')        # 输入已存在的图片名称
------------------------------------------------------------------------------------------------------------------
   程序执行结果： 
   图片资源：data/picture_source/screen111.png 文件不存在!
   结束     
---------------------
 结束
```

## 3、raise抛出异常

通常情况下，我们的代码不会主动抛出异常，而是通过返回一个错误码来告知调用者这里出现了不该出现的错误。其实我们也可以在代码中抛出异常，通过异常将相关的错误信息发送出去。使用raise语句可以主动抛出异常，用于处理一些因用户错误操作和输入而产生的问题。

```python
示例：
while True:
    try:
        name = input('请输入姓名（不能小于两位）:')
        if len(name) < 2:          # 如果名字的长度 < 2
            raise Exception('短了')      # 主动抛出异常
        else:          
            pwd = input('请输入密码：')  
        print(name,pwd)  
    except Exception as e:
        print(e)
-----------------------------------------------------------------------------------------------------------------
    程序执行结果:
    请输入姓名(不能小于两位)：y
    短了
    请输入姓名(不能小于两位)：ymx
    请输入密码：123
    ymx 123
    请输入姓名(不能小于两位)：
```

```python
实例：   
 def find_img(widget):
    try:
         # 若未匹配到图片，raise主动抛出异常
        if find_image(widget) is False:   
            raise (u'图片匹配错误')
        else:
            return find_image(widget)
    except:
        print('找不到图片 {}'.format(widget))
        
 if __name__ == '__main__':
    find_img()
```

## 4、自定义异常

​    自定义异常类型，只要该类继承Exception类即可，类的主题内容可以自定义	

```python
示例：
class AgeException(Exception):     # 自定义异常类AgeException
    def __init__(self,err='123'):      
        Exception.__init__(self,err)

def verify_age(age):         
    if int(age) < 18:
        raise AgeException
    else:
        print('Age:'+ str(age))

if __name__ == '__main__':
    verify_age(1)
    verify_age(30)
----------------------------------------------------------------------------------------------------------------
    程序执行结果：
    AgeException: 123
--------------------------    
    Age：30
```

