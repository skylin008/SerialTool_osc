# SerialTool_osc
---

SerialTool 是一个非常优秀的串口助手，使用Qt编写，可以跨平台使用，[项目地址](https://github.com/gztss/SerialTool)

[软件下载地址](https://github.com/gztss/SerialTool/releases/tag/v1.2.4)


C语言的用法作者已经写的很详细了，这里我主要说一下如何在microPython环境下使用SerialTool来显示波形

导入数据打包库 sendwave

    import sendwave
创建对象

    osc = sendwave.SendWave()

生成随机数组

	import urandom
    value = [urandom.randint(20, 2000) for x in range(16)]

创建数据流

	data = osc.ws_point(value[1]) 	#点模式,单条数据发送
	data = osc.ws_point(*value) 	#点模式，发送一个长度小于等于16的任意列表

	data = osc.ws_sync(*value) 		#同步模式，发送一个长度小于等于16的任意列表
调用串口发送

	uart.write(data)

整体程序

	from machine import UART
	import sendwave
	import urandom
	import time
	
	uart = UART(2, baudrate=115200, rx=13,tx=12,timeout=10)
	
	osc = sendwave.SendWave()
	
	for i in range(100):
	    value = [urandom.randint(20, 2000) for x in range(16)]
	    data = osc.ws_point(*value) 
	    #data = osc.ws_sync(*value)        
	    uart.write(data)
	    time.sleep(0.1)