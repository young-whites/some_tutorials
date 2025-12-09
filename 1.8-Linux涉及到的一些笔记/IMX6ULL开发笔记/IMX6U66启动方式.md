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




# <span class="wathet"><font size=4>IMX6ULL启动方式</font></span>
## <font size=3>一、设置IMX6ULL的Cortex-A处理去的运行模式</font>
<font size=2>

i.MX6ULL（以及整个 i.MX6 系列）处理器上电后的运行模式（Boot Mode） 由两个引脚 BOOT_MODE[1:0] 决定，一共有 4 种模式，但实际开发中最常用的是前两种。


| BOOT_MODE1 | BOOT_MODE0 | 模式名称          | 实际用途与行为 | 典型使用场景 |
| ---------- | ---------- | ----------------- | -------------- | ------------ |
| 0          | 0          | Boot From Fuses   | 完全按照芯片内部 eFuse 烧录的配置启动               |              |
| 0          | 1          | Serial Downloader | 进入串口下载模式（USB OTG 或 UART），等待 PC 端工具（如 imx_usb_loader、UUU、mfgtools）下载程序到 RAM 或直接烧写               |              |
| 1          | 0          | Internal Boot     |                |              |
| 1          | 1          | 保留（Reserved）  |                |              |



</font>
















