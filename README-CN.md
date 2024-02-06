# OSComp-RISC-V-SBI

OS竞赛项目1：增强型RISC-V SBI，由华中科技大学RIRAM（Rust实现的RISC-V抽象机器）团队开发。

## 基本计划

> 让RISC-V变得更好！
> 
> *唐纳德·特朗普*

竞赛挑战：[proj1-rustsbi](https://github.com/oscomp/proj1-rustsbi)

基本知识可以在这个存储库中找到。

### 开发环境

- 开发系统：Ubuntu（？）
- 开发板：未确定
- 编程语言：Rust、C和汇编（RISC-V）

### 待实现的功能

**基本功能**是为特定的RISC-V硬件系列提供具体的SBI（Supervisor Binary Interface）支持，使其能够运行符合RISC-V SBI标准的系统平台。

基于上述功能，我们可能实现以下扩展功能：

- **引导程序：**编写一个引导程序，以Oreboot项目为蓝本，增加其在SBI兼容的硬件平台上的实现。
- **增强SBI功能：**
  - 开发一个SBI固件，它能下载操作系统内核并开机运行。可以选择从串口、USB等有线或无线方式下载，可以下载到RAM以实现快速运行。
  - 编写一个SBI固件，它能检测系统中是否插着可移除的存储设备（如SD卡），如果有，尝试运行存储设备上的操作系统内核。
  - 编写一个SBI固件，它能连接到调试用的宿主机（如PC机）并在S模式下调试正在运行的操作系统内核。它应允许从宿主机输入指令，并从SBI固件返回调试结果。

### 实施步骤

1. 2.7 ~ x.x：由于尚未确定开发板，我们将使用RustSBI-qemu的代码作为我们的开发参考资料。

   1.0. 学习Rust语言，获取基本的RISC-V知识，研究*qemu代码*。
   
   1.1. 尝试在RustSBI-qemu中实现**增强功能部分**。
   
3. x.x ~ x.x：根据选定的开发板，我们将开始将RustSBI适配到特定的开发板型号上。（请参阅附录以了解已有适配的开发板。）

   2.1. RustSBI可以以库的形式使用。通常情况下，RustSBI的实现基于对**embedded-hal**的实现。（基础代码实现支持）
   
   2.2. 运行时：使用riscv-rt或其他工具。
   
4. x.x ~ x.x：完成其余功能，包括引导程序和增强SBI功能。

### 附录

#### 平台支持

- rustsbi-k510 - 适用于Kendryte K510平台的RISC-V SBI固件支持
- **rustsbi-d1** - 适用于Allwinner D1 SoC板的RustSBI引导程序固件和调试套件，包括Nezha、Lichee等
- **rustsbi-hifive-unmatched** - 适用于SiFive FU740板的RustSBI支持；FU740是一个具有四个SiFive U74核心和一个SiFive S7核心的五核异构处理器。
- rustsbi-qemu - QEMU平台的SBI支持实现
- rustsbi-k210 - 使用RustSBI的Kendryte K210 SBI支持，通过模拟1.9.1来提供1.12特权规范环境

  （加粗的文字表示往届同学的竞赛产出。）

#### 参考链接

- [embedded-hal](https://docs.rs/embedded-hal/latest/embedded_hal/)
- [rustsbi-k510](https://github.com/Gstalker/rustsbi-k510)
- [rustsbi-d1](https://github.com/rustsbi/rustsbi-d1)
- [rustsbi-hifive-unmatched](https://github.com/rustsbi/rustsbi-hifive-unmatched)
- [rustsbi-qemu](https://github.com/rustsbi/rustsbi-qemu)
- [rustsbi-k210](https://github.com/rustsbi/rustsbi-k210)
- [RISC-V 手册](http://riscvbook.com/chinese/RISC-V-Reader-Chinese-v2p1.pdf)
- [RISC-V SBI规范](https://github.com/riscv-non-isa/riscv-sbi-doc/)
- [Rust学习](https://course.rs/about-book.html)
- [riscv-rt](https://crates.io/crates/riscv-rt)
- [rCore运行指导](https://rcore-os.cn/rCore-Tutorial-Book-v3/)
