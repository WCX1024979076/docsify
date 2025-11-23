# ESP32_C3芯片上手使用指南

目前`bsp/ESP32_C3`芯片已支持使用`scons`进行编译，不再使用之前的`idf.py`编译。

## 主要改进点

为了实现利用`scons`来编译`RT-Thread`，主要进行了以下改进：

1、在[github.com/RT-Thread-packages/esp-idf](https://github.com/RT-Thread-packages/esp-idf)中增加`SConscript`编译配置文件。

2、修改了[github.com/RT-Thread/rt-thread/tree/master/bsp/ESP32_C3](https://github.com/RT-Thread/rt-thread/tree/master/bsp/ESP32_C3)中的`Sconscript`文件。

## 环境搭建及编译

1. 下载 RISC-V 工具链：

    ```
    wget https://github.com/espressif/crosstool-NG/releases/download/esp-2022r1-RC1/riscv32-esp-elf-gcc11_2_0-esp-2022r1-RC1-linux-amd64.tar.xz
    tar xf riscv32-esp-elf-gcc11_2_0-esp-2022r1-RC1-linux-amd64.tar.xz
    ```

2. 配置工具链的路径：

    在`rtconfig.py`文件中将`RISC-V`工具链的本地路径添加到`EXEC_PATH`变量中，或者通过设置 `RTT_EXEC_PATH`环境变量指定路径，例如：

    ```
    export RTT_EXEC_PATH=/opt/riscv32-esp-elf/bin
    ```

3. 编译

    安装 esptool 用于转换 ELF 文件为二进制烧录文件：

    ```
    pip install esptool
    ```

    在 Linux 平台下执行以下命令进行配置：

    ```
    scons --menuconfig
    ```

    它会自动下载env相关脚本到`~/.env`目录，然后执行：

    ```
    source ~/.env/env.sh
    
    cd bsp/ESP32_C3/
    pkgs --update
    ```

    它会自动下载`RT-Thread-packages/esp-idf`和`RT-Thread-packages/FreeRTOS-Wrapper`，更新完软件包后，执行 `scons` 来编译这个板级支持包。

    如果编译成功，将生成`rtthread.elf`、`rtthread.bin`文件。

## 下载烧录

1. 烧录工具下载

    当前bsp测试使用`flash_download_tool_3.9.4`工具进行烧录无误。

    烧录工具下载地址：https://www.espressif.com.cn/sites/default/files/tools/flash_download_tool_3.9.4_0.zip

2. 烧录工具配置

    芯片型号选择`ESP32-C3`。

    将二进制文件与偏移地址配置如下：

    | 二进制文件          | 偏移地址 |
    | ------------------- | -------- |
    | bootloader.bin      | 0x0      |
    | partition-table.bin | 0x8000   |
    | rtthread.bin        | 0x10000  |

    其中`bootloader.bin`和`partition-table.bin`可在`bsp/ESP32_C3/builtin_imgs`文件夹下找到，配置完成后截图如下，之后点击`START`即可下载。

    ![flash_download_tools](https://github.com/RT-Thread/rt-thread/raw/master/bsp/ESP32_C3/images/flash_download_tools.png)

## 运行截图

![shell截图](https://picup.tim-wcx.ltd/img/20230726122848.png)

## 问题与反馈

目前个人使用`ESP32-C3-MINI-1`开发板测试使用正常，但尚未在`LUATOS_ESP32C3`和`HX-DK-商`上进行测试。欢各位小伙伴帮忙测试一下。如果有任何问题，请在评论区私聊我或发送邮件至 [timwcx@qq.com](mailto:timwcx@qq.com)。
