


```
IDE：RT-Thread Studio v2.2.9 或更高版本。
RTT 版本：v4.1.0 或更高（支持 DMA 驱动）。
芯片：STM32F407IGT6
硬件：串口引脚已连接（e.g., UART3: PB10-TX, PB11-RX）
```


# <font size=4>RTT配置串口DMA模式收发</font>
## <font size=3>一、RT-Thread Settings配置</font>
<font size=2>

- 双击工程根目录下的 RT-Thread Setting（或在菜单中打开）
- 依次点击 `"组件"` > `"设备驱动程序"` > `"使用UART设备驱动程序"` > `"Choice Serial version" , 选择RT_USING_SERIAL_V1` > `使能串口DMA模式（勾选上）` > `设置RX缓冲区大小（1024）`

![RT-Thread Settings配置](./images/uart_v1_pic1.png)

</font>




## <font size=3>二、CubeMX配置</font>
<font size=2>

- 
- 

![RT-Thread Settings配置](./images/uart_v1_pic1.png)

</font>


