---
title: python3创建虚拟环境
tags: python,小书匠
grammar_cjkRuby: true
---
本文章默认在python3.7+上运行，不支持py2

Python 3.3以上使用venv来代替了原来Python2使用的virtualenv创建虚拟环境。

需要注意的是，在Python3.3中使用"venv"命令创建的环境不包含"pip"，你需要进行手动安装。在Python3.4中改进了这一个缺陷。

##### 1、为什么创建虚拟环境

 - 搭建独立的python运行环境，不与系统或其他项目产生冲突
 - 有助于包的管理和防止版本冲突
 - 方便在git clone别人的代码后，安装requirements.txt的版本
 - 删除卸载方便，直接删除对应的虚拟环境文件夹即可

##### 2、在Win/Mac/Linux下创建虚拟环境

###### 2.1 在Windows下创建

###### 2.2 在Linux下创建（待）
###### 2.3 在Mac下创建（待）

##### 3、在PyCharm下创建虚拟环境



【参考】
1、venv官网说明：https://docs.python.org/3/library/venv.html