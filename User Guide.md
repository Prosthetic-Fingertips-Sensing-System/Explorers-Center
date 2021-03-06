# 假肢手指尖传感系统 用户指南

## 1 概述

“假肢手指尖传感系统”（下称“系统”）是一款基于 Python 3.8 开发的单机程序，兼容 Windows、macOS、Linux 等多种操作系统。安装后，该系统可通过串行端口快速便捷地对假肢手微控制器、执行器、传感器等器件进行调试和操作，无需安装通信软件，无需配置环境参数。系统同假肢手进行串行数据交换，包括发送数据、接收数据、远程开关等，还可载入脚本或导出通信记录、日志（以文本文档（`*.txt`）或逗号分隔值（`*.csv`）等文件格式为例），以方便通信控制及后续处理。

运行该系统前，需要部署 Python 环境（适用于完全模式）或可执行文件（适用于快捷模式）进行安装。安装后启动系统，硬件连接假肢手控制器后即可使用。

该系统面向智能假肢手控制器开发，使用前应当先读取控制器参数，以确保通信准确快速进行。系统通过状态提示框向用户输出，具备完善的自查功能。

### 1.1 主要功能和技术特点

通过计算机与智能假肢手有线串行通信，为操作者提供指令集和用户界面，可以对假肢手控制器、执行器、传感器进行操作和调试；获取上述器件的运行结果和执行信息，并反馈给操作者，或一并收集提取到指定文件。

技术特点在于：

1. 通过串行端口通信和文本编码，将操作者指令转换为机器码并通过串口发送或读取；
2. 利用 Python 语言，实时、准确调度各项功能组件，从而实现信息交互；
3. 利用 PyQt 开发了清晰、友好的图形用户界面和窗口渲染；
4. 算法高效快速，并具备一定的可扩展性。

### 1.2 定义

本系统及相关手册可能出现以下术语，下面给出其定义。

- **假肢手**

    指医学中为患者用户提供手部支持的一类设备。本系统主要适用于智能假肢手，除特殊说明外，文中本名词均指智能假肢手。智能假肢手是指除纯机械装置外还具有微控制器、执行器和编程接口的一类假肢手。

- **（微）控制器 / MCU**

    指假肢手结构中对执行器、传感器等元件进行控制的器件单元。微控制器具有编程接口和通信接口。

- **执行器**

    指假肢手结构中控制机械装置、实际执行机械功能的一类器件。执行器接受微控制器控制。

- **传感器**

    指假肢手结构中探测本体和周围物理量的一类器件。传感器接受微控制器控制。

- **假肢手指尖传感系统**

  指利用 Python 开发、供用户调试和操作假肢手而使用的软件系统。

- **串口 / 端口**

    指假肢手连接到计算机时，供信息传输的特定信道。串口号/端口号由计算机和控制器硬件共同决定。

- **日志**

    指在系统运行期间，由系统根据用户操作自动生成的系统状态记录。

- **异常**

    指日志中记录、由于用户误操作或软件内部问题导致出现错误的一系列特殊情况。

- **通信记录**

    指自系统启动后，计算机和假肢手双方在系统中进行通信的信息记录。

## 2 系统安装与启动

### 2.1 快捷模式安装和启动

运行系统所在目录下的 `指尖传感.exe`，即可由程序自动部署相关环境并开始运行。再次运行时，重复上述过程即可。

注意系统目录下的其他文件不应删除，且保持和 `指尖传感.exe` 的相对位置不变。

### 2.2 完全模式部署和安装

完全安装适用于对 Python、串行通信、交互界面等有基本了解的专业用户。完全安装是程序代码的显式操作，可以在当前软件基础上扩展更多功能，方便用户使用。

完全安装所必要的环境包括：

    Python：推荐使用 Python 3.5 - 3.8 版本
    PyQt 5 及配套工具包
    PySerial

以上程序应当通过程序官方授权的平台和指定方式进行安装。

安装完毕后，在命令提示符中操作运行系统所在目录 `src` 文件夹下的 `main.py`，即完成软件的部署和启动。

若要查看和编辑系统程序代码进行扩展操作，需要安装编辑器和编译器。推荐的程序 / 插件包括：

    Microsoft® Visual Studio Code
    PyQt Integration
    PyQt Designer
    PyQt QSS Editor

以上程序应当通过程序官方授权的平台和指定方式进行安装。

## 3 系统界面

安装完成后，即可通过指定方式（完全模式下运行 `main.py`，快捷模式下运行 `指尖传感.exe`）启动系统。还可在桌面双击 `假肢手指尖传感系统.lnk` 快捷方式启动系统。

本文以下部分除特殊说明外，均以快速模式下的系统为例。

进入系统，系统界面允许以常规窗口或全屏显示。常规窗口默认尺寸为 800×600 像素，若屏幕分辨率低于此值，部分画面可能显示不全影响您的正常使用，请升级您的显示器。
系统界面如图所示。

【图】

平台界面主要包括两部分，分别为菜单栏和主视窗。菜单栏提供一些扩展操作（如设置、串口通信、连接到 App 等），主视窗是程序的主要部分，提供对假肢手的控制和通信方法和界面，并反映当前用户的操作结果和程序状态。

## 4 平台操作

### 4.1 读取硬件参数

在启动平台前，首先需要对所配置的假肢手及其控制器的硬件参数具有基本的了解。

1. 假肢手基本参数

    用户应首先了解假肢手的品牌、型号、版本、适用范围、兼容性等，并在掌握上述信息的情况下合理使用，避免产生伤害。

2. 控制器参数

    在将假肢手与计算机连接前，请首先阅读假肢手控制说明文档，具体了解以下参数，供连接时使用：

        硬件接口（通用串行总线 USB，串行接口 RS-232、RS-422 等）、通信方式（串行通信 COM、并行通信 LPT 等）、通信速率（即波特率，Baudrate）、数据位（Bytesize）、停止位（Stopbits）、校验方式（Parity）等。
        
        必要时还应当了解控制器的供电方式、电压范围、硬件接线和故障排除等。

    常用的控制器包括 MCS-51 系列、STM-32 系列、AVR 系列等。本文以 Arduino UNO 系列开发板为例，其核心控制单元为 AT Mega 328 处理器，支持以 C 语言为基础的程序语言进行操作和控制及串行通信。

3. 传感器参数

    读取假肢手需要用到的传感器的参数，包括：

        受测量、地址、分辨率、编码格式、数据协议、控制协议、兼容性等。

    对于在使用前需要初始化设定的传感器，请先按照传感器说明完成相关步骤后再连接到控制器。

4. 执行器参数

    读取假肢手装载的执行器的参数，包括：

        控制量、地址、受控范围、编码格式、控制协议、兼容性等。

    对于在使用前需要初始化设定的执行器，请先按照执行器说明完成相关步骤后再连接到控制器。

### 4.2 将假肢手连接到计算机

根据步骤 4.1 读取的控制器硬件接口，选取一根适当的线缆，两端分别连接到假肢手控制器和计算机，连接成功后计算机会自动识别该设备，假肢手或控制器上的相关指示灯也会亮起。可通过计算机的设备管理器查看该设备的串行端口号。

### 4.3 启动平台和串口检测

上述步骤完成后，可以启动平台。启动后，系统会自动执行一次串口检测，状态提示框会显示检测结果：若有可用串口，状态提示框将显示串口号和设备名称，若无可用串口，状态提示框将显示“无可用串口”。

若未检测到已连接的串口，用户可在菜单栏中的 `设置——串口参数——检测串口` 中重新检测当前串口，若仍未检测到串口，请尝试重新连接线缆，更新驱动程序，或检查设备兼容性。

### 4.4 串口选择

单击主界面的串口选择框（默认为 `COM1`），子菜单项会出现 `COM1` ~ `COM12` 等 12 个串口。用光标滑过这些菜单项，若其中某些串口可用，对应菜单项的字体会加粗显示。

点击需要设定的串口号，若设置成功，串口选择框将显示选中的串口。

### 4.5 传输设置

单击菜单栏 `设置——串口参数`，其中有若干选项可供设置，包括 `波特率`、`校验位`、`数据位`、`停止位` 等。这些选项均为计算机同假肢手之间的数据传输设置，只有计算机和假肢手控制器的设置对应相同，数据和控制指令才可以准确传输。

单击菜单栏 `设置——串口参数——波特率`，子菜单项会出现 `110` ~ `460 800` 等 18 个波特率选项，默认选中 `9 600`。点击选择需要设定的波特率。

单击菜单栏 `设置——串口参数——校验位`，子菜单项会出现 `None`（无校验）、`Even`（奇校验）、`Odd`（偶校验）、`Mark`（标识符校验）和 `Space`（空格校验）等 5 个校验选项，默认选中 `None`。点击选择需要设定的校验位。

单击菜单栏 `设置——串口参数——数据位`，子菜单项会出现 `5` ~ `8` 等 4 个数据位选项，默认选中 `8`。点击选择需要设定的数据位。

单击菜单栏 `设置——串口参数——停止位`，子菜单项会出现 `1`、`2` 共 2 个停止位选项，默认选中 `1`。点击选择需要设定的停止位。值得注意的是，为了提高多端兼容性，本平台暂不支持 1.5 位停止位。

### 4.6 开启串口

在确认串口和传输设置均已按要求设置完毕后，可以开启串口,开启串口的滑动按钮在主视窗上部。点击滑动按钮，当按钮一侧出现 `ON` 字样时则开启成功。开启串口后，串口和传输设置将不能更改，当前串口处于监听和占用状态，可以随时接收和发送数据。若要修改串口或传输设置，请先关闭串口（4.10 部分）。

### 4.7 串行数据传输的格式

串行数据的发送和接收以二进制进行。发送和接收消息时，通信双方先将消息中的字符按双方指定的编码格式编码为若干位二进制数（如 ASCII 为 8 位），接收时再将这些二进制数解码为字符。本平台以 ASCII 为默认编码格式，因此只支持 127 个字符，如需中文和其他语言字符，请使用 Hex 发送 / 接收（4.8 / 4.9 部分），导出数据信息（ 4.13 部分）后在其他软件中解码，或在完全模式下更改编码格式。

为方便记述，本文按如下方式表达字符编码：以字母 `A` 举例，根据 ASCII，其编码为 `65`，对应 8 位二进制数为 `0100 0001`，十六进制数为 `41`。记 `' '` 为字符型数据，`b`、`d`、`h` 分别为二进制、十进制和十六进制整型数据的标识符，对应三种整型数据则记为 `b'A'＝0100 0001`，`d'A'＝65`，`h'A'＝0x41`。

### 4.8 接收数据

    说明：在最新版本中，Hex 接收 功能已被暂时取消。

在菜单栏 `串口通信` 中可以查看所有从串口接收到的信息。

默认状态下，接收框以解码后的 `'<data>'` 形式显示。在调试等特殊情况下、需要使用 16 进制接收数据的，平台提供 `Hex 接收` 切换的滑动按钮。 Hex 接收模式下，字符将显示为 `h'<data>'` 的形式，并以每 2 位为显示单位，单位与单位之间以空格隔开。

### 4.9 发送数据

    说明：在最新版本中，Hex 发送 和 定时发送 功能已被暂时取消。

在菜单栏 `串口通信` 中可以向串口发送信息。在发送框输入需要发送的信息，点击 `发送` 按钮，即可向串口发送内容。

默认状态下，点击发送时，发送框中的内容视为 `'<data>'`，发送时先进行编码再发送。在调试等特殊情况下、需要使用 16 进制发送数据的，平台提供 `Hex发送` 切换的滑动按钮。Hex 发送模式下，发送框的内容将视为 `h'<data>'`，若出现除 16 进制数外的字符，平台则会提示发送失败。我们建议用户以每 2 位为单位，单位与单位之间以空格隔开，以方便数据流的查看和阅读。

平台还支持定时发送数据。设定好定时发送定时器后，点击右侧的滑动按钮，当按钮一侧出现 `ON` 字样时启用定时发送，将按设定时间间隔将发送框中的数据向串口发送。定时发送启用后，定时器将不能更改，若要更改定时时长，请先关闭定时发送。

### 4.10 关闭串口

在串行通信完毕后，可以关闭串口。关闭串口是开启串口的逆操作，关闭串口的滑动按钮与开启按钮为同一个。点击滑动按钮，当按钮一侧出现 `OFF` 字样时则关闭成功。关闭串口后，串口将不再监听数据，也不再对串口反应。

### 4.11 窗框操作

    说明：在最新版本中，清除窗框、清除接收框 和 清除发送框 功能已被暂时取消。

接收框和发送框均为可编辑型，除输入外，还支持撤销（Undo）、还原（Redo）、剪切（Cut）、复制（Copy）、粘贴（Paste）、清除（Delete）、全选（Select All）等操作，可使用右键菜单指向上述命令。

除此之外，点击菜单栏中的 `视图`，在 `清除窗框` 组中可以快速选择 `清除接收框`、`清除发送框` 和 `清除窗框`（清除接收框和发送框）。

需要注意的是，清除相关框后，导出的通信记录（4.13 部分）也不再包含相关内容。

### 4.12 数据计数器操作

    说明：在最新版本中，清除数据计数器、清除接收数据 和 清除发送数据 功能已被暂时取消。

平台内置数据计数器，用以计算接收和发送的数据量。接收或发送数据后，菜单栏 `串口通信` 左下方状态显示栏将按照 `已接收 x | 已发送 x` 的形式显示，单位为字节（Byte）。

数据计数器在关闭和重新开启串口之后自动重置清零。此外，若需重置数据计数器，可点击菜单栏中的 `视图`，在 `清除数据计数器` 组中选择 `清除接收数据`、`清除发送数据` 和 `清除数据计数器`（清除接收数据和发送数据两个计数器）。

### 4.13 导出信息记录

    说明：在最新版本中，导出信息记录 功能已被暂时取消。请使用 4.11 部分提到的“窗框操作”复制通信信息。

单击菜单栏中的“文件”——“导出接收信息”，可以导出信息记录。导出文件支持文本文档（`*.txt`）或逗号分隔值（`*.csv`）等文件格式，可在必要时进行后续操作。

### 4.14 查看日志

单击菜单栏 `设置——日志——查看日志目录`，平台将启动日志所在的文件夹（6.1 部分）。

### 4.15 退出平台

单击窗口右上角的 `×` 号，即可退出平台。退出时将清除平台全部实例，包括串口、连接参数、接收和发送信息等，请务必在保存好相关内容后再退出。

## 5 快捷键

    说明：在最新版本中，部分快捷键被重新定义。

操作 | 快捷键
---- | ----
**菜单栏** |
文件 | `Alt＋F`
\- 导出接收信息 | `Alt＋F＋O`
\- 查看日志 | `Alt＋F＋L`
\- 退出 | `Alt＋F＋E` </br> `Ctrl＋Q`
设置 |
视图 |
帮助 |
\- 用户手册 | `Alt＋H＋U` `F1`
\- 关于 | `Alt＋H＋A`
**主视窗** |
发送 | `Alt＋S`

## 6 异常及故障排除

### 6.1 查看平台日志

单击菜单栏 `设置——日志——查看日志目录`，平台将启动日志所在的文件夹。

日志文件夹可能包括日志文件（`*.log`）和屏幕截图（`*.png`），其中中将记录软件出现异常时的环境、用户操作和调用情况等。排除异常时可能将需要上述文件。

我们建议用户定期清理日志文件夹，以避免占用空间过大。

### 6.2 基本异常及排除

异常描述 | 故障排除
---- | ----
无法完成安装 | 查看文件是否丢失或出错？ </br> 完全安装模式下安装环境是否已经安装？ </br> 是否具有管理员权限？
无法启动 | 查看安装是否成功？ </br> 尝试卸载后重新安装
程序乱码 | 查看版本是否兼容？
数据乱码 | 查看串口数据传输是否为 UTF-8 格式？ </br> 尝试开启 Hex 接收 和 Hex 发送
控制命令未产生如期效果 | 查看假肢手文档，检查控制命令？ </br> 尝试开启 Hex 接收 和 Hex 发送
报错“请输入 2 位 16 进制数，以空格分隔” | 出现不受支持的字符。 </br> Hex 发送仅支持 `0` ~ `9`、`A` ~ `F` 共 16 个字符
检测不到串口 | 重新连接线缆，通过“设备管理器”检查是否连接成功 </br> 更新串行设备驱动软件
报错“无法打开。该串口是否存在或已被占用？” | 串口是否存在？ </br> 是否有其他程序占用串口？
报错“无法关闭” | 串口是否存在？ </br> 串口是否已被强制关闭？
报错“无法获取” | 串口监听异常 </br> 串口是否存在？ </br> 串口是否已被强制关闭？
报错“无法读取” | 查看串口数据传输是否为 UTF-8 格式？ </br> 尝试开启 Hex 接收 和 Hex 发送
报错“无法发送” | 串口是否存在？ </br> 串口是否已被强制关闭？ </br> 查看串口数据传输是否为 UTF-8 格式？ </br> 尝试开启 Hex 接收 和 Hex 发送
报错“其他原因。请查看 log” | 用户操作是否符合使用说明？ </br> 是否出现非法操作？ </br> 串口是否成功连接？ </br> 请联系开发者
程序闪退 | 用户操作是否符合使用说明？ </br> 是否出现非法操作？ </br> 串口是否成功连接？ </br> 请联系开发者

### 6.3 反编译

本平台不支持反编译。
