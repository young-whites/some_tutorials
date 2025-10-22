<style>
.highlight{
  color: white;
  background: linear-gradient(90deg, #ff6b6b, #4ecdc4);
  padding: 5px;
  border-radius: 5px;
}

.mint_green{
  color: white;
  background: #adcdadf2; 
  padding: 5px;
  border-radius: 5px;
}

.red {
  color: #ff0000;
}
.green {
  color:rgb(10, 162, 10);
}
.blue {
  color:rgb(17, 0, 255);
}

.wathet {
  color:rgb(0, 132, 255);
}
</style>


# <span class="wathet"><font size=4>RT-Thread串口框架</font></span>
## <font size=3>一、V1和V2版本的区别</font>
<font size=2>
<div style="background:#e8f5e8;padding:10px;border-radius:6px;color:#333;">
ℹ️ RT-Thread 的串口框架从 V1（RT_USING_SERIAL_V1）升级到 V2（RT_USING_SERIAL_V2）是一次架构级重构，目标是彻底解耦硬件实现与应用逻辑、统一收发模型、降低丢包率并提升可移植性。<br>
</div>

| 维度          | V1                                                     | V2                                                                             |
| ------------- | ------------------------------------------------------ | ------------------------------------------------------------------------------ |
| 缓冲区模型    | 只有 1 个接收环形缓冲区（`RT_SERIAL_RB_BUFSZ` 全局宏） | 每路 UART 独立 **RX + TX 双缓冲区**（`BSP_UARTx_RX_BUFSIZE / TX_BUFSIZE`）     |
| 硬件/应用模式 | 应用层需要显式指定“中断 / DMA / 轮询”硬件模式          | 框架不再感知硬件模式，只向应用暴露 **阻塞 / 非阻塞** 两种语义                  |
| 驱动接口      | `dma_transmit()` 仅用于 DMA                            | 统一成 `transmit()`，同时支持中断、DMA、轮询                                   |
| 缓冲区实现    | 自己写环形缓冲区代码                                   | 直接使用 RT-Thread 官方的 `struct rt_ringbuffer`                               |
| 打开标志      | `RT_DEVICE_FLAG_INT_RX / DMA_RX / INT_TX / DMA_TX`     | `RT_DEVICE_FLAG_RX_BLOCKING / RX_NON_BLOCKING / TX_BLOCKING / TX_NON_BLOCKING` |
| 配置项        | `struct serial_configure` 只有 `bufsz`                 | 拆成 `rx_bufsz` + `tx_bufsz`，可分别给 0（轮询）、非 0（中断/DMA）             |

</font>


## <font size=3>二、从应用层直接感受到的变化</font>
<font size=2>

1. 配置标志变动
``V1`` 用 ``RT_DEVICE_FLAG_INT_RX``,``DMA_RX`` 等硬件工作模式标志；
``V2`` 改用 ``RT_DEVICE_FLAG_RX_BLOCKING``,``RX_NON_BLOCKING`` 等应用操作模式标志
<br>

2. 每个串口都有独立的缓冲区
``V1`` 全部串口共用 ``RT_SERIAL_RB_BUFSZ``（默认 64 B）；
``V2`` 要求给每一个 ``UART`` 显式定义
BSP_UARTx_RX_BUFSIZE / BSP_UARTx_TX_BUFSIZE
否则按 RT_SERIAL_RX/TX_MINBUFSZ（≥64 B）取整
优点：高吞吐串口可以单独给 2 kB，低功耗串口保持 0 B（轮询），节省内存
<br>

3. V2新增了发送“缓冲区”
``V1`` 只有接收 FIFO，发送是没有缓冲层的``DMA/中断``，应用连续 write 容易丢尾包
``V2`` 收发都带 ``rt_ringbuffer``，阻塞写会等到数据全部进缓冲区再返回，零丢包（阻塞 / 非阻塞可自选）
``V2`` 版本同一设备可以实现``rt_device_open(..., RT_DEVICE_FLAG_RX_NON_BLOCKING | RT_DEVICE_FLAG_TX_BLOCKING);``
实现“接收用中断回调不阻塞，发送用阻塞保证发完”，V1 做不到这么细

</font>


## <font size=3>三、移植</font>

<font size=2>

打开``RT-Thread Settings``，在组件中找到``使用UART设备驱动程序``，然后把``Choice Serial version``中的``RT_USING_SERIAL_V1``改成``RT_USING_SERIAL_V2``，然后保存等待配置完成。


</font>

