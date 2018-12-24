# FT2232 IO配置问题

>> ftdi_layout_init 0x0f18 0x0fff

* 16+16 IO配置只针对一个通道,高8位、低8位对应FT2232原理图上GPIOL/H(X),只针对MPSSE模式
* 前一组为output data，后一组为direction，1为output，0 为input。

>> ftdi_layout_signal nSRST -data 0x0001 -noe 0x0100

* 设置system reset 信号线为GPIOL0,其高阻控制线为GPIOL2
* 复位线需要具有高阻态功能，由于FT2232HL不提供三态输出，不进行配置将会报错。
* 三态复位线应该参考100ask-JTAGv3设计图纸

>> reset_config srst_only

* 配置系统复位

>> reset_config none

* 在复位线有问题时屏蔽复位

>> reset_config srst_only srst_push_pull

* 复杂配置如此叠加

>> ftdi_layout_signal LED1 -data 0x0800 -ninput 0x0001

* 将高第3位配置为LED1,数据来源于低0位数据反相（驱动能力问题测试没有通过）


# 20180309 
* 可以单线复位 L0-L3是高位不是低位，rst设置推挽可以不用OE 

