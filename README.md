# 长连接服务器技术说明
## 一： socket发送数据协议：

协议头 | 校验 | 协议标识 | 数据类型 | 数据内容
---|---|---|---|---|---
4 Byte | 8 Byte | 64 Byte | 4 Byte | n Byte
 0 - 4 | 5 - 12 | 13 - 76 | 77 - 80 | 81 -n
   
 
  
 
序号 | 名称 | 类型 | 长度 | 说明
---|---|---|---|---
1 | 协议头 | 整数型 | 4 | 0-255 请求类型
2 | 校验 | 长整数型 | 8 | 默认为socket数据长度
3 | 协议标识 | 文本型 | 64 | 协议传输标识
4 | 数据类型 | 整数型 | 4 | 1-255 携带数据类型
5 | 数据内容 | 文本型 | n | 协议携带待处理数据
6 | 客户信息 | 文本型 | 20000 | Socket连接客户端信息
 
协议头：4字节    整数

校验：8字节    长整数

协议标识：64字节  文本

数据类型：   4字节  整数型 1-255

数据内容：无限
 


.程序集 Socket数据包, , 公开, 对socket接收数据进行拆包
.程序集变量 协议头, 整数型, , , 4Byte
.程序集变量 校验, 长整数型, , , 8Byte
.程序集变量 协议标识, 文本型, , , 64Byte
.程序集变量 数据类型, 整数型, , , 4Byte
.程序集变量 数据内容, 文本型, , , nByte
.程序集变量 客户信息, 文本型

 
 
####   1.协议头

 
    .常量 心跳连接, "0", , 保持socket不自动断开
    .常量 断开连接, "1", , 主动请求断开服务器，让服务器处理相应事务
    .常量 请求服务, "2", , 获取设备列表、其他数据请求（暂时）
    .常量 数据传送, "3", , IM推送等
    .常量 请求标识ID, "4", , 获得唯一标识义工使用
    .常量 得到标识ID, "5", , 获得服务器分配的标识
    .常量 请求终端列表, "21", , 获得当前服务器已连接设备
    .常量 请求终端列表反馈, "22"
    .常量 请求断开终端, "23", , 根据标识断开某一设备连接，需要权限（暂时测试使用）
    .常量 请求断开终端反馈, "24", , 数据类型1 成功 2失败 （暂时测试使用）


#### 2.协议详细


```
心跳连接 3s-100s,当服务器压力大时自动释放未主动发送数据客户端
协议头 ：协议头
校验 ：暂时为socket数据长度
协议标识 ：设备标识，登陆服务器会获得，请求标识不推荐，测试用
数据类型 ：作为反馈信息时默认1为成功2为失败3待定，作为数据请求传输时取值范围为11-255，保留十个数，不能设0，为了规避特殊原因0被屏蔽，使用0将无法传输数据。
数据内容  ：文本类json或xml，客户端和服务端要一至
客户信息  ：服务器存储信息，方便管理
```



## 二：客户端连接池
数据类型 ：ServerClient
 
序号 | 名称 | 类型 | 说明
---|---|---|---
1 | IP | 文本型 |客户端连接地址
2 | Flag | 文本型 | 协议传输标识
3 | Type | 整数型 | 1-255 携带数据类型,IM,push,其他
4 | LastTime | 日期时间型 | 当连接池过大，自动关闭较久未使用连接
5 | Tag | 文本型 | 扩展

    .   
    .成员 , 文本型, , , 127.0.0.1:8080
    .成员 , 文本型, , , 标识符
    .成员 , 整数型, , , 
    .成员 , 日期时间型   













## 三： 连接池模块：

 
ServerFunction
    
设置信息

创建ServerClient

获取信息

删除ServerClient

获取ServerClient

获取Client列表, 文本型 

获取Client数量, 整数型


 