# 通用平台V0.1型下载指北
## openocd

——————————————————————————————————————————————————————

### 新板子
* FT2232D的默认出厂VID和PID是 0x0403 0x6010，由于调试中驱动出现问题PID用FT_PROM改为0x6011
* portA功能设置为234FIFO
* portA驱动设置为D2XX
* 烧写EEPROM
* 设备名可以随意

----------------

### 统一设置
* 有可能需要先安装官方驱动
* zadig换驱动：将interface 0 的设备换为libusbK
* 一旦换驱动FT_PROM就无法识别FT2232了，就需要重装驱动
* openocd的interface文件可以使用我提供的文件 **ft2232D.cfg** ：


                >> interface ftdi

                >> #设置设备VID和PID要和设备设置一致
                >> #设备名可以缺省
                >> #FT2232D的通道号一定是0

                >> ftdi_vid_pid 0x0403 0x6011
                >> ftdi_channel 0

                >> #初始化IO,一定要初始化
                >> #设置复位IO，该版本并没有接复位线，但还需要假装接了
                >> ftdi_layout_init 0x0538 0x057b
                >> ftdi_layout_signal nSRST -data 0x0020 -noe 0x0100

* 之后下载基本参照之前的openocd的下载方式 : 

                >>(cmd openocd) openocd.exe -f ../scripts/board/frdm-k64.cfg 

* 需要指出 frdm-K64.cfg里的source [find interface/cmsis-dap.cfg]需要重新指向ft2232D.cfg : 

                >> (cmd gdb) arm-none-eabi-gdb.exe ./BUILD/K64F/GCC_ARM/mbed-os-example-blinky.elf
                >> (gdb) target remote localhost:3333

* 将mbed或者iar生成的bin文件放到OPENOCD文件夹下

* 由于没有接入复位线，需要手工复位
* 按住复位按钮，输入 : 

                >> (gdb) monitor program mbed-os-example-blinky.bin

* 回车之后释放复位按键即可下载
* 下载完成后再按一次复位启动

![FT_PROM设置PID](https://gitee.com/uploads/images/2018/0101/175656_18975b06_418895.png "FT_PROM设置PID.png")
![FT_PROM设置设备名](https://gitee.com/uploads/images/2018/0101/175720_156d00e7_418895.png "FT_PROM设置设备名.png")
![FT_PROM设置portA硬件](https://gitee.com/uploads/images/2018/0101/175732_ec3cf109_418895.png "FT_PROM设置portA硬件.png")
![FT_PROM设置portA驱动](https://gitee.com/uploads/images/2018/0101/175743_76afa586_418895.png "FT_PROM设置portA驱动.png")
![FT_PROM设置portB硬件](https://gitee.com/uploads/images/2018/0101/175754_be90749a_418895.png "FT_PROM设置portB硬件.png")
![FT_PROM设置portB驱动](https://gitee.com/uploads/images/2018/0101/175805_ed10ebe2_418895.png "FT_PROM设置portB驱动.png")
![FT_PROM烧写EEPROM](https://gitee.com/uploads/images/2018/0101/175813_94682342_418895.png "FT_PROM烧写EEPROM.png")
![zadig换驱动](https://gitee.com/uploads/images/2018/0101/175830_7b069273_418895.png "zadig换驱动.png")
![IAR生成BIN文件](https://gitee.com/uploads/images/2018/0101/175842_1918da5b_418895.png "IAR生成BIN文件.png")



