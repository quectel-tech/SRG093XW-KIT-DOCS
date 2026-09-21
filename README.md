# 文档概述
本文档介绍 SRG093XW EVK 的基本信息、开发环境搭建、镜像编译与烧录、主要功能及相关开发文档，为开发人员进行 SRG093XW Linux BSP 开发及功能验证提供参考。
>**说明：** 开发板已预烧录系统镜像，可直接上电使用。预烧录镜像用于基本功能体验，具体功能支持及软件版本请以当前发布的 SDK 为准。
## 文档索引
SRG093XW EVK 提供配套的硬件及软件开发文档，硬件设计、开发环境、功能配置及验证方法请参考以下文档。
| 分类 | 文档名称 | 主要内容 |
| --- | --- | --- |
| 产品规格书 | Quectel_SRG093X系列_短距离模块产品规格书 | SRG093X 系列产品规格、硬件资源、接口及主要功能特性说明 |
| 硬件设计 | Quectel_SRG093X系列_硬件设计手册 | 引脚定义、电源设计、应用接口、射频及电气性能等硬件设计说明 |
| EVB 用户指导 | Quectel_SR-IMXM_EVB_用户指导 | EVB 板卡布局、配件装配、硬件接口、按键与开关及开关机操作说明 |
| 编译与烧录 | Quectel_SRG091X&SRG093X系列_Linux_编译&烧录指导 | 开发环境搭建、Linux 镜像编译及烧录 |
| 显示与触摸 | Quectel_SRG091X&SRG093X系列_Linux_显示驱动_开发指导 | MIPI、LVDS、触摸、背光及显示驱动配置 |
| 摄像头 | Quectel_SRG093X系列_Linux_摄像头开发指导 | 摄像头驱动、Device Tree 及 ISP 固件配置 |
| 音频 | Quectel_SRG091X&SRG093X系列_Linux_音频功能测试指导 | Audio Codec 及音频功能测试 |
| 通用外设 | Quectel_SRG091X&SRG093X系列_Linux_外设用户指导 | GPIO、I2C、SPI、Ethernet、USB、ADC、CAN 等外设 |
| Wi-Fi | Quectel_SRG091X-W&SRG093X-W_Linux_Wi-Fi_功能验证指导 | Wi-Fi 驱动加载、AP/STA 模式及功能验证 |
| Bluetooth | Quectel_SRG091X&SRG093X系列_Linux_蓝牙用户指导 | Bluetooth 基本功能、音频及 BLE 功能验证 |
| 蜂窝网络 | SRG091X&SRG093X系列_Linux_蜂窝模块接入用户指导 | 蜂窝模块驱动移植、网络连接及语音通话功能验证 |
| Thread | Quectel_SRG091X&SRG093X系列_Linux_Thread_用户指导 | Thread 启动、组网及功能验证 |
| Matter | Quectel_SRG091X-W&SRG093X-W_Linux_Matter_用户指导 | Ethernet、BLE + Thread、BLE + Wi-Fi Matter 功能验证 |
| Cortex-M33 | Quectel_SRG093X系列_M33_开发指导 | M33 启动、FreeRTOS、Zephyr、镜像编译及集成 |
| Secure Boot | Quectel_SRG091X&SRG093X系列_Linux_Secure_Boot_应用指导 | Secure Boot、AHAB、密钥及安全镜像配置 |
| 分区调整 | Quectel_SRG091X&SRG093X系列_Linux_分区调整指导 | Rootfs、Boot 及 Flash 镜像分区调整 |
# 1. 快速上手
本章节介绍 SRG093XW EVK 的基本使用流程，包括开发板及 KIT 套件介绍、软硬件环境准备、SDK 环境搭建、系统镜像编译与烧录以及开发板启动。
完成本章节操作后，开发板应能够正常启动 Linux 系统，并可通过调试串口查看系统启动日志，具备后续外设、网络通信及其他功能的开发与调试条件。
> **说明：** 本章节仅介绍基本操作流程，各功能的详细配置、开发及调试方法请参考后续章节及对应的专项开发文档。
## 1.1 开发板介绍
  ### 1.1.1 基本信息
   SRG093XW EVK 是基于 NXP i.MX93 应用处理器设计的评估开发平台，用于验证 Linux BSP、显示、多媒体、无线连接、工业控制及边缘 AI 应用开发。
    开发板集成了 Quectel SRG093XW 核心模组，支持 Wi-Fi、Bluetooth、以太网、USB、MIPI-CSI Camera、MIPI-DSI/LVDS 显示等丰富外设接口，可满足工业 HMI、智能网关、边缘计算、视频分析及 IoT 终端等应用场景开发需求。
    插入图片-套件预览图
### 1.1.2 系统框图
SRG093XW EVK 系统框图如下：
  （备注：插入图1 框架图）
### 1.1.3 主要特性
-   NXP i.MX93 双核 Arm Cortex-A55
-   Cortex-M33 实时控制核心
-   Linux BSP 支持
-   内置 Wi-Fi + Bluetooth
-   Gigabit Ethernet
-   MIPI CSI Camera
-   MIPI DSI 显示接口
-   LVDS 显示接口
-   USB Host / Device
-   SD Card
-   eMMC Flash
-   PCIe 扩展接口
-   CAN FD 接口
-   Audio Codec
### 1.1.4 KIT 套件及配件
标准套件通常包含以下内容：（备注：放KIT套件实拍图）
| 设备 | 数量 | 说明 |
| --- | --- | --- |
| 移远通信 SRG093X-TE-A | 1 | 开发板主控板 |
| 移远通信 SR-IMXM-EVB | 1 | 开发板底板 |
| 电源适配器(5V) | 1 | 为 EVK 供电，规格以实际 KIT 配置为准 |
| USB Type-C 数据线 | 2 | 用于连接 Debug UART，镜像烧录或 USB 调试 |
| 蜂窝通信模组 EG21-GL | 1 | 用于蜂窝网络功能测试 |
| 蜂窝天线 | 1 | 搭配蜂窝模组使用 |
| 耳机 | 1 | 用于音频测试 |
| 喇叭 | 1 | 用于音频播放测试 |
| 摄像头模组 | 1 | 用于图像/视频采集测试 |
| MIPI-DSI 显示屏 | 1 | 用于 MIPI 显示功能测试（含触摸） |
> **说明：**
> - SRG093X 模组引脚定义、电源设计、应用接口、射频及电气性能等硬件设计信息，请参考《Quectel_SRG093X系列_硬件设计手册》。
> - SR-IMXM EVB 板卡布局、接口位置、配件装配、按键与开关及开关机操作等信息，请参考《Quectel_SR-IMXM_EVB_用户指导》。
## 1.2 开发准备
### 1.2.1 硬件准备
SRG093XW-KIT 套件包含开发板、电源适配器及相关调试线缆，具体内容请参考“KIT 套件及配件”章节。(备注：放接线连接图)
| 设备 | 数量 | 用途 |
| --- | --- | --- |
| SRG093XW-KIT 套件 | 1 | SRG093XW EVK 开发及功能验证 |
| 开发主机 | 1 | 用于 SDK 编译、镜像烧录及开发调试 |
| 网线 | 可选 | 用于以太网连接及网络功能测试 |
| SIM 卡 | 可选 | 用于蜂窝拨号上网及通话验证 |
### 1.2.2 软件准备
SRG093 系列 Linux BSP 开发主要需要 Linux 开发环境、SDK 开发包以及相关镜像烧录工具。此外，可根据实际网络环境选择是否使用 Yocto 源码下载缓存包。
#### Linux 开发环境
SRG093 SDK 的编译及软件开发需要使用 Linux 开发环境，推荐使用 Ubuntu 22.04 LTS及以上版本。具体环境要求请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》。
#### SDK 开发包
SDK 包中包含 SRG093 系列平台 Linux BSP 开发所需的相关源码、Yocto Layer、构建脚本及配置文件，开发者无需分别下载各软件组件。
获取 SDK 软件包 `SRG093XW_SDK.tar.zst` 后，将其复制到 Ubuntu 开发主机，并参考后续“环境搭建”章节完成 SDK 解压及编译环境配置。
#### Yocto 源码下载缓存
除 SDK 软件包外，同时提供 Yocto 编译所需的源码下载缓存包。源码下载缓存包为可选软件包，开发者可根据实际网络环境选择是否使用。
| 软件包 | 说明 | 是否必需 |
| --- | --- | --- |
| `SRG093XW_SDK..zst` | SRG093 SDK，包含 Yocto BSP 源码、Yocto Layer、编译脚本及相关工具 | 是 |
| `SRG093XW_Yocto_Downloads..zst` | Yocto 编译所需的源码下载缓存，用于减少编译过程中对外部网络的依赖 | 否 |
如需使用源码下载缓存，将缓存包解压至 SDK 的 `yocto/` 目录，使目录结构如下：
```text
SR-IMX93/
└── yocto/
    ├── sources/
    ├── downloads/
    └── ...
```
> **说明：**
> -   网络环境正常时，可不下载源码缓存包，Yocto 将在编译过程中根据需要获取相关源码。
> -   在网络访问受限或希望减少重复下载的情况下，建议下载并使用源码缓存包。
> -   若新增软件包、修改 Recipe 版本或当前缓存不完整，编译过程中仍可能需要下载额外源码。
#### 镜像烧录工具
编译生成系统镜像后，可根据开发环境选择对应的镜像烧录方式。
-   **Windows 环境**：使用 SDK 提供的 UUU 烧录工具进行镜像烧录。
-   **Linux 环境**：根据 SDK 支持情况使用对应的 UUU 工具或其他烧录方式。
具体烧录方法请参考“镜像烧录”。
## 1.3 环境搭建
### 1.3.1 SDK 解压
将获取到的 SRG093 SDK 软件包复制到 Ubuntu 开发主机。
1. **创建工作目录：**
```bash
mkdir -p ~/SR-IMX93
```
2. **将 SDK 软件包复制到工作目录并解压：**
```bash
cp SRG093XW_SDK.tar.zst ~/SR-IMX93/
cd ~/SR-IMX93
tar --zstd -xf SRG093XW_SDK.tar.zst
```
解压完成后，进入 Yocto 工作目录，通过以下命令确认 SDK 已正确解压：
```bash
cd ~/SR-IMX93/yocto
ls
```
目录中应包含 `sources`、`setup-environment` 等文件和目录。
### 1.3.2 SDK 目录介绍
SRG093 SDK 解压后的主要目录结构如下：
```text
SR-IMX93/
└── yocto/
    ├── build.sh
    ├── cst/
    ├── imx-setup-release.sh
    ├── README
    ├── README-IMXBSP
    ├── setup-environment
    ├── sources/
    │   ├── base/
    │   ├── meta-arm/
    │   ├── meta-freescale/
    │   ├── meta-freescale-distro/
    │   ├── meta-imx/
    │   ├── meta-imx-quectel-srg093x/
    │   ├── meta-nxp-connectivity/
    │   ├── meta-openembedded/
    │   ├── meta-qt6/
    │   ├── poky/
    │   └── ...
    └── uuu.exe
```
  主要目录及文件说明：
| 目录/文件 | 说明 |
| --- | --- |
| `sources/` | Yocto BSP 源码及各 Yocto Layer |
| `sources/meta-imx/` | NXP i.MX BSP 相关 Yocto Layer |
| `sources/meta-imx-quectel-srg093x/` | SRG093 系列平台相关配置、Recipe、DTS 及构建脚本 |
| `sources/meta-openembedded/` | OpenEmbedded 扩展 Layer |
| `sources/meta-qt6/` | Qt 6 相关 Yocto Layer |
| `sources/poky/` | Yocto Project Poky 基础构建系统 |
| `setup-environment` | Yocto 编译环境初始化脚本 |
| `imx-setup-release.sh` | i.MX BSP 编译环境配置脚本 |
| `build.sh` | SRG093 SDK 构建辅助脚本 |
| `cst/` | NXP Code Signing Tool 相关文件 |
| `uuu.exe` | Windows 平台 UUU 烧录工具 |
| `README` / `README-IMXBSP` | BSP 相关说明文档 |
**说明：** `build_srg093xw/` 为 SRG093XW 平台的 Yocto 编译目录，在初始化编译环境后生成，因此首次解压 SDK 时该目录不存在。
## 1.4 镜像编译
>**说明：** 本章节仅介绍基本镜像编译流程，详细编译说明及错误处理请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》。
### 1.4.1 编译完整镜像
完整的 SDK 编译流程请参考《Quectel\_SRG091X&SRG093X系列\_编译&烧录指导》。除文档中提供的编译方式外，也可通过以下方法快速完成 SRG093XW 系统镜像编译。
1. **进入 SRG093 SDK 的 Yocto 工作目录：**
```bash
cd ~/SR-IMX93/yocto
```
2. **初始化 SRG093XW 编译环境：**
```bash
MACHINE=quectel-srg093xw DISTRO=fsl-imx-xwayland source sources/meta-imx-quectel-srg093x/tools/imx-quectel-srg093x-setup.sh -b build_srg093xw
```
3. **执行以下命令确认目标平台：**
```bash
bitbake-getvar MACHINE
```
预期结果：
```text
MACHINE="quectel-srg093xw"
```
4. **完成编译环境初始化后，执行对应的系统镜像编译命令：**
```bash
bitbake <image-name>
```
其中 `<image-name>` 为 SDK 提供的系统镜像 Recipe，请根据实际发布的 SDK 配置填写。
首次编译耗时相对较长，后续编译可复用已有的构建缓存。
### 1.4.2 查看编译产物
编译完成后，SRG093XW 平台生成的镜像文件位于：
```text
~/SR-IMX93/yocto/build_srg093xw/tmp/deploy/images/quectel-srg093xw/
```
可执行以下命令查看：
```bash
ls -lh tmp/deploy/images/quectel-srg093xw/
```
>**说明：** 该目录包含系统镜像及相关编译产物。实际用于烧录的镜像文件请以当前 SDK 版本及《Quectel\_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》中的说明为准。
## 1.5 烧录镜像
>**说明：** 本章节仅介绍基本镜像烧录流程，详细烧录说明及错误处理请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》。
>**注意：** 切换 DIP 开关前，请先将开发板供电拨码开关拨至 **OFF**，避免带电切换启动模式。
烧录镜像、烧录工具（uuu.exe）以及烧录脚本（uuu.auto）会被自动打包到yocto/image 目录下。
### 1.5.1 Windows 环境烧录：
1. **连接调试串口**
将开发板的`USB-AP`端口连接到Windows PC。
2. **设置下载模式**
将TE-A 板上的DIP 开关设置为`1000`，使模块进入串行下载模式。
3. **准备烧录文件**
将 `yocto/image/` 目录下的烧录镜像、`uuu.exe` 和 `uuu.auto` 复制至 Windows PC 的同一目录。
4. **烧录镜像**
PC端打开powershell/CMD 到存放镜像和工具的目录，执行以下命令烧录镜像
```bash
uuu.exe uuu.auto
```
###  1.5.2 Linux 环境烧录：
- **Windows 环境**：使用 SDK 提供的 UUU 烧录工具进行镜像烧录。
- **Linux 环境**：使用 Linux 平台 UUU 工具进行镜像烧录。
### 1.5.3 系统启动
1. **连接调试串口**
将开发板的`DEBUG_UART` 端口连接到Windows PC 后，系统将枚举出四个COM端口。其中，COMA 对应A 核的UART 端口，是默认的串口调试端口。
```text
USB-Enhanced-SERIAL-A：A 核UART 端口
USB-Enhanced-SERIAL-B：M核UART 端口
USB-Enhanced-SERIAL-C：LTE UART 端口
USB-Enhanced-SERIAL-D：无其他功能
```
2. **设置启动模式**
将TE-A 板上的DIP 开关设置为`0100`（从eMMC Flash 启动模式）。
# 2. 功能列表与外设
SRG093XW EVK 提供显示、摄像头、音频、网络及无线通信等功能。各功能的详细配置及使用方法请参考对应的专项开发文档。
### 2.1 功能支持列表
| 功能 | 支持情况 | 说明 |
| --- | --- | --- |
| Ethernet | 支持 | 千兆以太网 |
| Wi-Fi | 支持 | 无线网络功能(内置) |
| Bluetooth | 支持 | Bluetooth 功能(内置) |
| 蜂窝网络 | 支持 | 支持外接蜂窝通信模块 |
| MIPI DSI | 支持 | 显示接口 |
| LVDS(不提供) | 选配 | LVDS 显示 |
| Touch | 支持 | 电容触摸 |
| MIPI CSI | 支持 | Camera 输入 |
| Audio | 支持 | 音频输入/输出 |
| USB | 支持 | USB 接口 |
| UART | 支持 | 串口通信 |
| I2C | 支持 | I2C 外设通信 |
| SPI | 支持 | SPI 外设通信 |
| GPIO | 支持 | GPIO 控制 |
| ADC | 支持 | 模拟信号采集 |
| CAN | 支持 | CAN 总线通信 |
## 2.2 显示与触摸
SRG093XW EVK 支持多种显示接口及触摸功能，可用于图形界面、工业 HMI 等应用开发。
主要支持：
-   MIPI DSI 显示
-   LVDS 显示
-   Touch 触摸功能
-   背光控制
-   Logo 配置
显示开发指导主要包含以下内容：
-   DRM 显示子系统介绍
-   MIPI、RGB、LVDS 面板适配
-   Device Tree 及面板驱动配置
-   背光及启动 Logo 配置
-   图像、彩条及视频显示验证
> **说明：** 显示接口配置、显示驱动及相关调试方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_显示驱动\_开发指导》。
## 2.3 摄像头
SRG093XW EVK 支持 MIPI CSI 摄像头接口，可用于图像采集、视频处理及视觉应用开发。
摄像头开发指导主要包含：
-   摄像头驱动源码
-   Device Tree 配置
-   AP1302 ISP 固件配置
> **说明：** 摄像头驱动配置、Camera 使用及测试方法请参考《Quectel\_SRG093X系列\_Linux\_摄像头开发指导》。
## 2.4 音频
SRG093XW EVK 支持音频输入/输出功能，可用于音频播放、录音及相关应用开发。
音频功能指导主要包含：
-   音频相关软件编译与烧录
-   Audio Codec 功能测试
> **说明：** 音频功能配置及测试方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_音频功能测试指导》。
## 2.5 通用外设
SRG093XW EVK 提供多种通用外设接口，可用于设备连接、数据采集及通信等应用开发。
主要包括：
-   GPIO
-   I2C
-   SPI
-   Ethernet
-   USB
-   ADC
-   CAN
外设用户指导主要包含：
-   GPIO 配置及功能测试
-   I2C 接口使用
-   SPI 配置及功能测试
-   Ethernet 功能使用
-   USB 配置及工作模式
-   ADC 配置及电压采集
-   CAN 功能使用
> **说明：** 各外设接口的配置、使用及测试方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_外设用户指导》。
# 3.  网络与通信
SRG093XW EVK 支持 Wi-Fi、Bluetooth、蜂窝网络等网络与无线通信功能，并支持 Thread、Matter 等物联网应用。
各功能的详细配置及使用方法请参考对应的专项开发文档。
## 3.1 Wi-Fi
SRG093XW EVK 内置 Wi-Fi 功能，可用于无线网络连接及相关应用开发。
Wi-Fi 功能验证指导主要包含：
-   Wi-Fi 驱动加载
-   Wi-Fi AP 模式配置及功能验证
-   IEEE 802.11n/802.11ac 无线标准模式配置
-   Wi-Fi 加密模式配置
-   Wi-Fi STA 模式配置及功能验证
-   STA 模式下连接加密及非加密 AP
> **说明：** SRG093XW Wi-Fi 功能配置及验证方法请参考《Quectel\_SRG091X-W&SRG093X-W\_Linux\_Wi-Fi\_功能验证指导》。
## 3.2 Bluetooth
SRG093XW EVK 内置 Bluetooth 功能，可用于蓝牙设备连接、音频传输及低功耗蓝牙等应用开发。
Bluetooth 用户指导主要包含：
-   Bluetooth 驱动加载及基本功能验证
-   Bluetooth 音频 Sink/Source 功能验证
-   PipeWire 蓝牙音频服务配置
-   BLE Server 功能验证
-   BLE Client 功能验证
-   BLE 数据读写测试
> **说明：** Bluetooth 功能配置及使用方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_蓝牙用户指导》。
## 3.3 蜂窝网络
SRG093XW EVK 支持外接蜂窝通信模块，可用于蜂窝网络连接、数据通信及语音通话等应用开发。
蜂窝功能开发指导主要包含：
-   蜂窝模块 USB 转串口及 USB 网络驱动移植
-   QMI\_WWAN、ECM 网络驱动配置
-   QConnectManager 移植及配置
-   蜂窝网络上网功能验证
-   语音通话功能配置及验证
> **说明：** 蜂窝模块驱动移植、网络连接及语音通话等功能的配置和验证方法请参考《SRG091X&SRG093X系列\_Linux\_蜂窝模块接入用户指导》。
相关参考文档：
- 《Quectel_UMTS_LTE_5G_Linux_USB_Driver_用户指导》
- 《Quectel_EC2x&EG2x&EG9x&EM05_Series_AT_Commands_Manual》
- 《Quectel_QConnectManager_Linux_用户指导》
## 3.4 Thread
SRG093XW EVK 支持 Thread 网络功能，可用于低功耗物联网设备组网及相关应用开发。
Thread 用户指导主要包含：
-   Thread 功能启动
-   Thread 网络创建及配置
-   Thread 网络连接及功能验证
> **说明：** Thread 功能配置及使用方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_Thread\_用户指导》。
## 3.5 Matter
SRG093XW EVK 支持 Matter 相关应用开发，可通过多种网络连接方式进行 Matter 设备配网及功能验证。
Matter 用户指导主要包含：
-   基于 Ethernet 的 Matter 功能验证
-   基于 BLE + Thread 的 Matter 功能验证
-   基于 BLE + Wi-Fi 的 Matter 功能验证
> **说明：** Matter 环境配置、功能使用及相关示例请参考《Quectel\_SRG091X-W&SRG093X-W\_Linux\_Matter\_用户指导》。
# 4.  高级功能
SRG093XW 平台支持 Cortex-M33 实时处理、安全启动及系统分区调整等高级功能，可满足实时控制、系统安全及存储配置等应用需求。
## 4.1 Cortex-M33
SRG093XW 平台集成 Cortex-M33 实时处理器，可用于实时控制及相关应用开发，并支持 FreeRTOS 和 Zephyr 开发环境。
M33 开发指导主要包含：
-   M33 核硬件及调试串口介绍
-   U-Boot 及 Linux 内核阶段启动 M33 核
-   FreeRTOS 开发及 M33 镜像编译
-   Zephyr 开发环境搭建及镜像编译
-   M33 镜像集成及启动验证
> **说明：** Cortex-M33 开发环境、工程编译及运行方法请参考《Quectel\_SRG093X系列\_M33\_开发指导》。
## 4.2 Secure Boot
SRG093XW 平台支持 Secure Boot，可通过镜像认证机制提升系统启动过程的安全性。
Secure Boot 应用指导主要包含：
-   AHAB 安全启动机制介绍
-   Secure Boot 使能及镜像生成
-   PKI 密钥树及 SRK 表生成
-   安全镜像烧录及启动验证
-   SRK 哈希值 eFuse 烧录
-   CST 工具使用及密钥生成
> **说明：** Secure Boot 配置及使用方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_Secure\_Boot\_应用指导》。
## 4.3 分区调整
SRG093XW Linux BSP 支持根据实际应用需求调整系统镜像分区及分区大小，以满足不同存储空间配置需求。
分区调整指导主要包含：
-   系统镜像布局介绍
-   Rootfs 分区大小调整
-   Boot 分区大小调整
-   Flash 镜像分区配置
> **说明：** 分区配置及调整方法请参考《Quectel\_SRG091X&SRG093X系列\_Linux\_分区调整指导》。
# 5.  FAQ
（备注：移远联系方式；论坛版块和github）
本章节汇总 SRG093XW EVK 开发及使用过程中常见的问题及解决方法，相关内容将根据实际开发及使用情况持续更新。
# 修订记录
| 版本 | 日期 | 修订内容 |
| --- | --- | --- |
| V1.0 | 2026-09-18 | 初始版本 |