# proj1-RustSBI 基本方案

> 03/02/2024

## 赛题具体实现功能

**实现和扩展RISC-V SBI运行时，使之能够支持并运行操作系统**

### 题目背景

RISC-V指令集的SBI标准规定了类Unix平台下，操作系统运行环境的规范。这个规范拥有多种实现，RustSBI是它的一种实现。

> RISC-V: RISC-V（“RISC five”）的目标是成为一个通用的指令集架构（ISA）
>
> > ISA: Instruction Set Architecture
>
> SBI: Supervisor Binary Interface, System call style calling convention between Supervisor (S-mode OS) and Supervisor Execution Environment (SEE)
>
> > SEE: A M-mode RUNTIME firmware for OS/Hypervisor running in HS-mode or a HS-mode Hypervisor for Guest OS running in VS-mode.

RISC-V架构中，存在着定义于操作系统之下的运行环境 (SBI) 。这个运行环境不仅将引导启动 RISC-V 下的操作系统，还将常驻后台，为操作系统提供一系列二进制接口，以便其获取和操作硬件信息。RISC-V 给出了此类环境和二进制接口的规范，称为“操作系统二进制接口”，即“SBI”。

SBI的实现是在M模式下运行的特定于平台的固件，它将管理S、U等特权上的程序或通用的操作系统。

> 固件（Firmware）：相对于Software，担任计算机最基础最底层工作的软件；特定于平台指针对于不同处理器，该固件是不一致的
>
> > 在本次竞赛中，SBI开发所针对固件？

> RISC-V 模式：
>
> - M 模式：machine mode，运行最可信代码，在 M 模式下运行的 hart 对内存，I/O 和一些对于启动和配 置系统来说必要的底层功能有着完全的使用权。因此它是唯一所有标准 RISC-V 处理器都 必须实现的权限模式。
>
>   > hart: hardware thread，硬件线程
>
> - U 模式：用户模式，受到一定限制
>
>   > PMP：实现了 M 和 U 模式的处理器具有一个叫做物理内存保护（PMP，Physical Memory Protection）的功能，允许 M 模式指定 U 模式可以访问的内存地址。PMP 包括几个地址寄存器（通常为 8 到 16 个）和相应的配置寄存器。这些配置寄存器可以授予或拒绝读、写和执行权限。
>
> - S模式：监管者（SuperVisor）模式。使用基于页面的虚拟内存。这个功能构成了监管者模式（S 模式）的核心，这是一种可选的权限模式，旨在支持现代类 Unix 操作系统，如 Linux，FreeBSD 和 Windows。

RustSBI项目发起于鹏城实验室的“rCore代码之夏-2020”活动，它是完全由Rust语言开发的SBI实现。 现在它能够在支持的RISC-V设备上运行rCore教程和其它操作系统内核。

> rCore: **从零开始** 用 **Rust** 语言写一个基于 **RISC-V** 架构的 **类 Unix 内核**
>
> 支持的RISC-V设备: ？按照题目要求？

RustSBI项目的目标是，制作一个从固件启动的最小Rust语言SBI实现，为可能的复杂实现提供参考和支持。 RustSBI也可以作为一个库使用，帮助更多的SBI开发者适配自己的平台，以支持更多处理器核和片上系统。 RustSBI已经被RISC-V SBI标准收录，作为官方推荐的实现之一，它的实现编号为4。

> 其他竞争品类：OpenSBI等

RustSBI的主要特点如下：

- 适配 RISC-V SBI 规范 v0.3
- 对类 Unix 操作系统有很好支持
- 完全使用 Rust 语言实现
- 是 OpenSBI 项目的有力竞争品
- 支持 QEMU 仿真器（RISC-V 特权级版本 v1.11）
- 向下兼容 RISC-V 特权级版本 v1.9
- 支持 Kendryte K210（含MMU和S模式）

### 平台实现的注意事项

- RustSBI 可以库的形式使用。通常情况下，RustSBI 的实现基于对 embedded-hal 的实现。

  > embedded-hal: https://docs.rs/embedded-hal/latest/embedded_hal/
  >
  > ？

- 目前，在 QEMU 和 K210 平台上支持 CLINT 和 PLIC 外围设备。

  > 目前已有以下rustsbi支持：
  >
  > # Platform support
  >
  > - [rustsbi-k510](https://github.com/Gstalker/rustsbi-k510) - RISC-V SBI firmware support for Kendryte K510 platform
  > - [rustsbi-d1](https://github.com/rustsbi/rustsbi-d1) - RustSBI bootloader firmware and debug suite for Allwinner D1 SoC boards, including Nezha, Lichee and more
  > - [rustsbi-hifive-unmatched](https://github.com/rustsbi/rustsbi-hifive-unmatched) - RustSBI support on SiFive FU740 board; FU740 is a five-core heterogeneous processor with four SiFive U74 cores, and one SiFive S7 core
  > - [rustsbi-qemu](https://github.com/rustsbi/rustsbi-qemu) - QEMU platform SBI support implementation
  > - [rustsbi-k210](https://github.com/rustsbi/rustsbi-k210) - Kendryte K210 SBI support using RustSBI, provides privileged spec 1.12 environment by emulating it using 1.9.1

### 建议选择的开发板（2020年，待更新）：

- [VisionFive SBC](https://starfivetech.com/site/exploit), 2 * SiFive U74
- Allwinner D1/D1s boards, 有多个供应商, 1 * T-Head Xuantie C906
- HiFive Unmatched, 异构多核, 4 * SiFive U74, 1 * SiFive S7

---

---

### 第一题：RustSBI支持

- 挑选RISC-V硬件或模拟器，调查它具有哪些对RustSBI实现有帮助的外设。

  > SBI 实现基础，确定固件运行基础硬件

- 选择可用的Rust语言运行时。您可以从开源实现（如riscv-rt）中选择，也可以自己编写简单的运行时；

  > ?

- 使用RustSBI、HAL库和运行时，编写代码，使RustSBI支持挑选的RISC-V硬件或硬件模拟器。至少需要能运行rCore-Tutorial项目的操作系统内核。

  > 已有RustSBI，embedded-hal，runtime，可以直接运行其上。写外设驱动？

### 第二题：引导程序环境

- 挑选RISC-V硬件或模拟器。对它包含的中断处理器（如CLINT、PLIC等），阅读文档或逆向分析，了解此中断处理器的使用方法；

- Oreboot是一个引导程序框架。阅读Oreboot的文档，了解它的功能；

  > todo

- 分支Oreboot项目，为它增加一个平台实现，即您想要移植的开发板。

如果目标开发板具有闪存，建议将引导程序烧录到闪存中，然后由闪存启动系统。否则，可以使用SD卡分区的形式安装引导程序。

### 第三题：复杂的SBI实现

- 编写一个SBI固件，使之能下载操作系统内核并开机运行。可以选择从串口、USB等有线或无线方式下载，可以下载到RAM以实现快速运行（下载完毕后，请释放设备的所有权，以供操作系统使用）；

  > ?

- 编写一个SBI固件，它能检测系统中是否插着可移除的存储设备（如SD卡），如果有，尝试运行存储设备上的操作系统内核；

  > 

- 编写一个SBI固件，它能连接到调试用的宿主机（PC机等），并可在S模式下调试正在运行的操作系统内核。要求从宿主机输入指令，从SBI固件返回调试结果。

## 研究对象

- RISC-V: 了解 RISC-V 的指令集、特权级等知识

  见[RISC-V 手册](http://riscvbook.com/chinese/RISC-V-Reader-Chinese-v2p1.pdf)

- RISC-V SBI: RISC-V 的 SBI 标准，将对其进行实现

  见 [RISC-V SBI specification](https://github.com/riscv-non-isa/riscv-sbi-doc/)

- **已有的RustSBI：如rustsbi-qemu**

## 开发语言

Rust

- 安装rust：https://course.rs/first-try/installation.html
- rust练习：https://github.com/sunface/rust-by-practice/tree/master

RISC-V ISA

- RISC-V 手册

## 改善性能

todo

## 开发环境搭建

- Rust语言操作系统开发环境
  - ubuntu 22.04.2 LTS
  - GNU Make 4.3
  - QEMU emulator 7.0.0
  - rustc nightly-latest
  - rustup latest
  - cargo nightly-latest
  - code-server 4.10.1
  - code-server extensions
    - rust-analyzer 0.3.1435
    - Chinese (Simplified) Language Pack 1.75.0
  - https://tooldiy.ry.rs/one_click_deployment/docker_os_rust/
- 其他环境配置介绍
  - https://tooldiy.ry.rs/one_click_deployment/os/

## 运行环境

todo

## 测试

todo

