# SRG093XW EVK Linux 开发指南

 [![Document Version](https://img.shields.io/badge/docs-v1.0.1-blue)]() [![Status](https://img.shields.io/badge/status-stable-green)]() [![Platform](https://img.shields.io/badge/platform-Linux-orange)]() 

> **适用硬件：** Quectel SRG093XW EVK
>
> **最后更新：** 2026-09-19
>
> **适用对象：** Linux BSP / 驱动 / 应用开发人员 
>
> **核心内容：** 快速上手 · SDK 编译 · 镜像烧录 · 功能验证 · 网络通信 · 高级功能

# 目录

- [文档概览](#文档概览)
- [快速上手](#快速上手)
  - [开发板介绍](#开发板介绍)
  - [开发准备](#开发准备)
  - [环境搭建](#环境搭建)
  - [镜像编译](#镜像编译)
  - [镜像烧录与系统启动](#镜像烧录与系统启动)
- [专项开发文档](#专项开发文档)
- [FAQ](#FAQ)
- [修订记录](#修订记录)

---

# 文档概览

本文档介绍 **SRG093XW EVK** 的基本信息、开发环境搭建、镜像编译与烧录、主要功能及相关开发文档，为开发人员进行 SRG093XW Linux BSP 开发及功能验证提供参考。

> [!TIP]
>
> 开发板已预烧录系统镜像，可直接上电使用。预烧录镜像用于基本功能体验，具体功能支持及软件版本请以当前发布的 SDK 为准。

### 开始之前

如果你是第一次使用 SRG093XW EVK，建议按以下顺序操作：

**准备硬件 → 准备开发环境 → 获取SDK → 搭建环境 → 编译镜像 → 烧录镜像 → 启动系统 → 验证功能**

> [!TIP]
>
> 如果仅希望快速体验开发板，可跳过编译步骤，直接使用预烧录镜像完成上电和系统启动。

# 快速上手

本章节用于完成 SRG093XW EVK 的首次使用。完成后，开发板应能够正常启动 Linux 系统，并可通过调试串口查看系统启动日志。

> [!NOTE]
>
> 本章节仅介绍基本操作流程。各功能的详细配置、开发及调试方法，请参考后续章节及对应专项开发文档。

## 开发板介绍

### 基本信息

SRG093XW EVK 是基于 NXP i.MX93 应用处理器设计的评估开发平台，用于验证 Linux BSP、显示、多媒体、无线连接、工业控制及边缘 AI 应用开发。

开发板集成 Quectel SRG093XW 核心模组，支持 Wi-Fi、Bluetooth、以太网、USB、MIPI-CSI Camera、MIPI-DSI/LVDS 显示等丰富外设接口，可满足工业 HMI、智能网关、边缘计算、视频分析及 IoT 终端等应用场景开发需求。

套件预览图：

![KIT套件及配件](./media/KIT套件及配件.jpg)

### 系统框图

![SRG093XW-EVK 系统框图](./media/SRG093XW-EVK系统框图.png)

### 主要特性

| 类别 | 特性 |
| --- | --- |
| 处理器 | NXP i.MX93 双核 Arm Cortex-A55 |
| 实时控制核心 | Cortex-M33 |
| 操作系统 | Linux |
| 无线 | Wi-Fi + Bluetooth |
| 有线网络 | Gigabit Ethernet |
| 摄像头 | MIPI CSI |
| 显示 | MIPI DSI、LVDS |
| USB | USB Host / Device |
| 存储 | SD Card、eMMC Flash |
| 扩展 | PCIe |
| 工业接口 | CAN FD |
| 音频 | Audio Codec |

### 套件及配件

标准套件通常包含以下内容。

> [!NOTE]
>
> KIT 套件的实际配置可能随版本变化，请以实际交付内容为准。

| 设备 | 数量 | 用途 |
| --- | ---: | --- |
| 移远通信 SRG093X-TE-A | 1 | 开发板主控板 |
| 移远通信 SR-IMXM-EVB | 1 | 开发板底板 |
| 电源适配器（5V） | 1 | 为 EVK 供电 |
| USB Type-C 数据线 | 2 | Debug UART、镜像烧录或 USB 调试 |
| 蜂窝通信模组 EG21-GL | 1 | 蜂窝网络功能测试 |
| 蜂窝天线 | 1 | 搭配蜂窝模组使用 |
| 耳机 | 1 | 音频测试 |
| 喇叭 | 1 | 音频播放测试 |
| 摄像头模组 | 1 | 图像/视频采集测试 |
| MIPI-DSI 显示屏 | 1 | MIPI 显示及触摸测试 |

硬件设计及板卡操作请分别参考：

- [Quectel_SRG093X系列_硬件设计手册](./files/Quectel_SRG093X系列_硬件设计手册.pdf)
- [Quectel_SR-IMXM_EVB_用户指导](./files/Quectel_SR-IMXM_EVB_用户指导.pdf)

##  开发准备

### 硬件准备

| 设备 | 数量 | 用途 | 必需 |
| --- | ---: | --- | :---: |
| SRG093XW-KIT 套件 | 1 | SRG093XW EVK 开发及功能验证 | 是 |
| 开发主机 | 1 | SDK 编译、镜像烧录及开发调试 | 是 |
| 网线 | 可选 | 以太网连接及网络功能测试 | 否 |
| SIM 卡 | 可选 | 蜂窝拨号上网及通话验证 | 否 |

### 软件准备

SRG093 系列 Linux BSP 开发主要需要：

- Linux 开发环境
- SDK 开发包
- 镜像烧录工具
- 可选的 Yocto 源码下载缓存包

#### Linux 开发环境

推荐使用 **Ubuntu 22.04 LTS 及以上版本**。具体环境要求请参考《Quectel_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》（TODO：待补充编译烧录指导文件）。

#### SDK 开发包

SDK 包包含 SRG093 系列平台 Linux BSP 开发所需的源码、Yocto Layer、构建脚本及配置文件。

获取 `SRG093XW_SDK.tar.zst` 后，将其复制到 Ubuntu 开发主机，并按 [环境搭建](#环境搭建) 完成配置。

> **SDK 下载：** [SRG093XW_SDK.tar.zst](https://developer.quectel.com/doc/files/SRG093XW/SRG093XW_SDK.tar.zst)

#### Yocto 源码下载缓存

源码下载缓存包为**可选**软件包，可根据实际网络环境决定是否使用。

| 软件包 | 说明 | 是否必需 |
| --- | --- | :---: |
| `SRG093XW_SDK.tar.zst` | SRG093 SDK，包含 Yocto BSP 源码、Yocto Layer、编译脚本及相关工具 | 是 |
| `SRG093XW_Yocto_Downloads.tar.zst` | Yocto 编译所需源码下载缓存，用于减少编译过程中对外部网络的依赖 | 否 |

> **缓存包下载：** [SRG093XW_Yocto_Downloads.tar.zst](https://developer.quectel.com/doc/files/SRG093XW/SRG093XW_Yocto_Downloads.tar.zst)

如果使用源码下载缓存，请将缓存包解压至 SDK 的 `yocto/` 目录：

```text
SR-IMX93/
└── yocto/
    ├── sources/
    ├── downloads/
    └── ...
```

> [!TIP]
>
> - 网络环境正常时，可以**不**下载源码缓存包。
> - 网络访问受限或希望减少重复下载时，建议使用源码缓存包。
> - 新增软件包、修改 Recipe 版本或缓存不完整时，编译过程中仍可能需要下载额外源码。

#### 镜像烧录工具

| 开发环境 | 推荐方式 |
| --- | --- |
| Windows | 使用 SDK 提供的 UUU 烧录工具 |
| Linux | 根据 SDK 支持情况使用对应 UUU 工具或其他烧录方式 |

详细方法请参考 [镜像烧录与系统启动](#镜像烧录与系统启动)。

## 环境搭建

### SDK 解压

将获取到的 SRG093 SDK 软件包复制到 Ubuntu 开发主机。

**步骤 1：创建工作目录**

```bash
mkdir -p ~/SR-IMX93
```

**步骤 2：复制并解压 SDK**

```bash
cp SRG093XW_SDK.tar.zst ~/SR-IMX93/
cd ~/SR-IMX93
tar --zstd -xf SRG093XW_SDK.tar.zst
```

**步骤 3：确认 SDK 解压结果**

```bash
cd ~/SR-IMX93/yocto
ls
```

> [!NOTE]
>
> 目录中应包含 `sources`、`setup-environment` 等文件和目录。

目录结构如下：

```shell
/srg093x/code/SR-IMX93/yocto$ tree -L2
.
├── build.sh # Build Script
├── imx-setup-release.sh -> sources/meta-imx/tools/imx-setup-release.sh # Environment Setup Script
├── README -> sources/base/README
├── README-IMXBSP -> sources/meta-imx/README
├── setup-environment -> sources/base/setup-environment # Environment SetupScript (General)
├── sources # All Source Code and Layers
│ ├── base
│ ├── meta-arm
│ ├── meta-browser
│ ├── meta-clang
│ ├── meta-freescale
│ ├── meta-freescale-3rdparty
│ ├── meta-freescale-distro
│ ├── meta-imx
│ ├── meta-imx-quectel-srg093x
│ ├── meta-nxp-connectivity
│ ├── meta-nxp-demo-experience
│ ├── meta-openembedded
│ ├── meta-qt6
│ ├── meta-security
│ ├── meta-timesys
│ ├── meta-virtualization
│ └── poky
└── uuu.exe # Windows Flashing Tool
└── uuu # Linux Flashing Tool
```

主要目录及文件说明：

| 目录/文件 | 说明 |
| --- | --- |
| `sources/` | Yocto BSP 源码及各 Yocto Layer |
| `sources/meta-imx/` | NXP i.MX BSP 相关 Yocto Layer |
| `sources/meta-imx-quectel-srg093x/` | SRG093 系列平台配置、Recipe、DTS 及构建脚本 |
| `sources/meta-openembedded/` | OpenEmbedded 扩展 Layer |
| `sources/meta-qt6/` | Qt 6 相关 Yocto Layer |
| `sources/poky/` | Yocto Project Poky 基础构建系统 |
| `setup-environment` | Yocto 编译环境初始化脚本 |
| `imx-setup-release.sh` | i.MX BSP 编译环境配置脚本 |
| `build.sh` | SRG093 SDK 构建辅助脚本 |
| `cst/` | NXP Code Signing Tool 相关文件 |
| `uuu.exe` | Windows 平台 UUU 烧录工具 |
| `README` / `README-IMXBSP` | BSP 相关说明文档 |

> [!NOTE]
>
> `build_srg093xw/` 是 SRG093XW 平台的 Yocto 编译目录，在初始化编译环境后生成，首次解压 SDK 时不存在。

## 镜像编译

> [!NOTE]
>
> 本章节仅介绍基本镜像编译流程。详细编译说明及错误处理请参考《Quectel_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》。
>
> TODO：待补充编译烧录指导文件

### 编译完整镜像

**步骤 1：进入 Yocto 工作目录**

```bash
cd ~/SR-IMX93/yocto
```

**步骤 2：初始化 SRG093XW 编译环境**

```bash
MACHINE=quectel-srg093xw DISTRO=fsl-imx-xwayland source sources/meta-imx-quectel-srg093x/tools/imx-quectel-srg093x-setup.sh -b build_srg093xw
```

**步骤 3：确认目标平台**

```bash
bitbake-getvar MACHINE
```

预期结果：

```text
MACHINE="quectel-srg093xw"
```

**步骤 4：编译系统镜像**

```bash
bitbake <image-name>
```

其中，`<image-name>` 为 SDK 提供的系统镜像 Recipe，请根据实际发布的 SDK 配置填写。

> [!TIP]
>
> 首次编译耗时相对较长，后续编译可复用已有构建缓存。

![1790066901578](media/1790066901578.png)

### 查看编译产物

镜像文件位于：

```text
~/SR-IMX93/yocto/build_srg093xw/tmp/deploy/images/quectel-srg093xw/
```

查看编译产物：

```bash
ls -lh tmp/deploy/images/quectel-srg093xw/
```

> [!NOTE]
>
> 该目录包含系统镜像及相关编译产物。实际用于烧录的镜像文件请以当前 SDK 版本及《Quectel_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》中的说明为准。

## 镜像烧录与系统启动

> [!CAUTION]
>
> 切换 DIP 开关前，请先将开发板供电拨码开关拨至 **OFF**，避免带电切换启动模式。

烧录镜像、烧录工具 `uuu.exe` 以及烧录脚本 `uuu.auto` 会被自动打包到 `yocto/image` 目录。

### Windows 环境烧录

**步骤 1：连接调试串口**

如下图所示，将 SRG093X-W-TE-A安装到 SR-IMXM-EVB上。然后使用两根 USB Type-C转 Type-A数据线，将EVB 的 USB_AP端口和 DEBUG_UART端口连接到 Wihdows PC。

<img src="media/1790067030677.png" alt="1790067030677" width="50%;" />

**步骤 2：设置下载模式**

将 TE-A 板上的 DIP 开关设置为 `1000`，使模块进入串行下载模式。

<img src="media/1790067041821.png" alt="1790067041821" width="50%;" />

**步骤 3：准备烧录文件**

将 `yocto/images/` 镜像目录下的烧录镜像、`uuu.exe` 和 `uuu.auto` 复制至 Windows PC 的同一目录。

**步骤 4：执行烧录**

在 PowerShell / CMD 中进入烧录文件所在目录，执行：

```bash
uuu.exe uuu.auto
```

<img src="media/1790067195805.png" alt="1790067195805"  />

### Linux 环境烧录

使用 Linux 平台 UUU 工具进行镜像烧录，具体方式请以当前 SDK 支持情况及《Quectel_SRG091X&SRG093X系列\_Linux\_编译&烧录指导》为准。

> [!NOTE]
>
> 在Linux构建环境中，无需切换到 Windows进行烧录。只需使用构建过程中生成的uuu.auto脚本，并配合Linux原生UUU工具即可完成烧录。

### 系统启动

**步骤 1：连接调试串口**

将开发板的 `DEBUG_UART` 端口连接到 Windows PC。系统将枚举出四个 COM 端口，其中 **COMA** 对应 A 核 UART 端口，是默认串口调试端口。

| 端口 | 功能 |
| --- | --- |
| USB-Enhanced-SERIAL-A | A 核 UART 端口 |
| USB-Enhanced-SERIAL-B | M 核 UART 端口 |
| USB-Enhanced-SERIAL-C | LTE UART 端口 |
| USB-Enhanced-SERIAL-D | 无其他功能 |

**步骤 2：设置启动模式**

将 TE-A 板上的 DIP 开关设置为 `0100`，进入 **eMMC Flash 启动模式**。

**步骤 3：断电重启**

打开串口终端工具，选择相应的调试串口，并将波特率设置为115200。然后将开发板断电后重新上电。如果开发板成功启动并显示控制台命令提示符，则说明镜像已正确烧录。默认登录用户名为`root`。

![1790068184843](media/1790068184843.png)

# 专项开发文档

SRG093XW EVK 提供显示、摄像头、音频、网络及无线通信等功能。各功能的详细配置及使用方法请参考对应专项开发文档。

根据你的开发目标选择对应文档：

| 🎯 如果你想……                          | 📖 推荐查看                                                   | 适用内容                                   |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------ |
| 👆了解SRG093X系列_短距离模块产品规格书 | [《Quectel_SRG093X系列_短距离模块产品规格书》](./files/Quectel_SRG093X系列_短距离模块产品规格书.pdf) | 产品规格、硬件资源、接口及主要功能         |
| 👆了解SRG093X系列_硬件设计             | [《Quectel_SRG093X系列_硬件设计手册》](./files/Quectel_SRG093X系列_硬件设计手册.pdf) | 引脚、电源、接口、射频及电气设计           |
| 👆了解EVB使用说明                      | [《Quectel_SR-IMXM_EVB_用户指导》](./files/Quectel_SR-IMXM_EVB_用户指导.pdf) | 板卡布局、接口、配件、按键及开关机         |
| 🚀 编译并烧录                          | 《Quectel_SRG091X&SRG093X系列\_Linux_编译&烧录指导》         | 开发环境、Linux 镜像编译及烧录             |
| 🖥️ 开发显示/触摸                       | [《Quectel_SRG091X&SRG093X系列\_Linux\_显示驱动_开发指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_显示驱动_开发指导.pdf) | MIPI、LVDS、触摸、背光及显示驱动           |
| 📷 开发 Camera                         | [《Quectel_SRG093X系列\_Linux\_摄像头开发指导》](./files/Quectel_SRG093X系列_Linux_摄像头开发指导.pdf) | 摄像头驱动、Device Tree 及 ISP             |
| 🔊 开发音频                            | [《Quectel_SRG091X&SRG093X系列\_Linux\_音频功能测试指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_音频功能测试指导.pdf) | Audio Codec 及音频测试                     |
| 🔌 开发通用外设                        | [《Quectel_SRG091X&SRG093X系列\_Linux\_外设用户指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_外设用户指导.pdf) | GPIO、I2C、SPI、Ethernet、USB、ADC、CAN    |
| 📡 开发 Wi-Fi                          | 《Quectel_SRG091X-W&SRG093X-W_Linux_Wi-Fi_功能验证指导》     | Wi-Fi 驱动、AP/STA 模式及功能验证          |
| 🔵 开发 BlueTooth                      | [《Quectel_SRG091X&SRG093X系列\_Linux\_蓝牙用户指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_蓝牙用户指导.pdf) | Bluetooth、音频及 BLE 功能验证             |
| 📶 接入蜂窝网络                        | [《SRG091X&SRG093X系列\_Linux\_蜂窝模块接入用户指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_蜂窝模块接入_用户指导.pdf)<br/>[《Quectel_UMTS_LTE_5G_Linux_USB_Driver_用户指导》](./files/reference/Quectel_UMTS_LTE_5G_Linux_USB_Driver_用户指导_V1.2.pdf)<br/>[《Quectel_EC2x&EG2x&EG9x&EM05_Series_AT_Commands_Manual》](./files/reference/Quectel_EC2x&EG2x&EG9x&EM05_Series_AT_Commands_Manual_V2.2.pdf)<br/>[《Quectel_QConnectManager_Linux_用户指导》](./files/Quectel_QConnectManager_Linux_用户指导_V1.0.pdf) | 驱动移植、网络连接及语音通话               |
| 🌐 开发 Thread                         | [《Quectel\_SRG091X&SRG093X系列\_Linux\_Thread\_用户指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_Thread_用户指导.pdf) | Thread 启动、组网及功能验证                |
| 🌐 开发 Matter                         | [《Quectel_SRG091X-W&SRG093X-W_Linux_Matter_用户指导》](./files/Quectel_SRG091X-W&SRG093X-W_Linux_Matter_用户指导.pdf) | Ethernet、BLE + Thread、BLE + Wi-Fi Matter |
| ⚙️ 开发 Cortex-M33                     | [Quectel\_SRG093X系列\_M33\_开发指导》](./files/Quectel_SRG093X系列_M33_开发指导.pdf) | M33、FreeRTOS、Zephyr、镜像编译及集成      |
| 🔐 开启 Secure Boot                    | [《Quectel\_SRG091X&SRG093X系列\_Linux\_Secure\_Boot\_应用指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_Secure_Boot_应用指导.pdf) | Secure Boot、AHAB、密钥及安全镜像          |
| 💾 调整系统存储分区                    | [《Quectel\_SRG091X&SRG093X系列\_Linux\_分区调整指导》](./files/Quectel_SRG091X&SRG093X系列_Linux_分区调整指导.pdf) | Rootfs、Boot 及 Flash 镜像分区             |

# FAQ

参考文档：[FAQ](./FAQ.md)

# 修订记录

| 版本 | 日期 | 修订内容 |
| --- | --- | --- |
| V1.0.0 | 2026-09-18 | 初始版本 |
| V1.0.1 | 2026-09-19 | 基于用户体验优化文档：<br/>- 增加文档目录<br/>- 按照开发者任务阅读路径重新组织文档信息 |
