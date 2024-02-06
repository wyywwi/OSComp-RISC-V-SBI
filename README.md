# OSComp-RISC-V-SBI

OS Competition proj 1: an enhanced RISC-V SBI, by HUST RIRAM (Rust Implemented RISC-V Abstract Machine) team.

## Basic Plan

> Make RISC-V Better!
> 
> *Donald Trump*

competition challenge: [proj1-rustsbi](https://github.com/oscomp/proj1-rustsbi)

The basic knowledges can be found in this repository.

### development environment

- develop system: ubuntu(?)
- developer board: not determined
- programming language: Rust, C & assembly(RISC-V)

###  functionality to be implemented

**Our admission** is to provide specific SBI (Supervisor Binary Interface) support for a particular RISC-V hardware series, enabling it to run system platforms compliant with the RISC-V SBI standard.

**Based on** the functions mentioned above, we are interested in the following extended features:

- **Bootloader:** Write a bootloader program, based on the Oreboot project, to enhance its implementation on SBI-compatible hardware platforms.
- **Enhanced SBI Function:**
  - Develop an SBI firmware that is capable of downloading the operating system kernel and initiating boot-up. It should offer the option for downloading through various wired or wireless methods, such as serial, USB, and be capable of loading into RAM for rapid execution.
  - Develop an SBI firmware that can detect the presence of removable storage devices (e.g., SD cards) within the system. If detected, it should attempt to execute the operating system kernel located on the storage device.
  - Create an SBI firmware capable of connecting to a debugging host (e.g., a PC) and debugging the running operating system kernel in S-mode. It should allow inputting commands from the host and returning debugging results from the SBI firmware.
 
### implementation steps

1. 2.7 ~ x.x: As the board is not determined yet, we are going to use the RustSBI-qemu's code as our development reference material.
   1.0. Learn Rust language, acquire basic RISC-V knowledge, and study the *qemu code*.
   1.1. Attempt to implement the **enhanced functions** in RustSBI-qemu.
2. x.x ~ x.x: Based on the selected board, we will commence the adaptation of RustSBI to the specific development board model. (Refer to the appendix for boards with existing adaptations.)
   2.1. RustSBI can be used in the form of a library. Typically, RustSBI's implementation is based on the implementation of the **embedded-hal**. (the basic code implementation support)
   2.2. runtime：using riscv-rt or others.
4. x.x ~ x.x: Complete the remaining functionalities, including the bootloader program and enhancing SBI capabilities.

### Appendix

#### Platform support

- rustsbi-k510 - RISC-V SBI firmware support for Kendryte K510 platform
- **rustsbi-d1** - RustSBI bootloader firmware and debug suite for Allwinner D1 SoC boards, including Nezha, Lichee and more
- **rustsbi-hifive-unmatched** - RustSBI support on SiFive FU740 board; FU740 is a five-core heterogeneous processor with four SiFive U74 cores, and one SiFive S7 core
- rustsbi-qemu - QEMU platform SBI support implementation
- rustsbi-k210 - Kendryte K210 SBI support using RustSBI, provides privileged spec 1.12 environment by emulating it using 1.9.1

  (The bolded text represents the competition outputs of previous students.)

#### reference links

- [embedded-hal](https://docs.rs/embedded-hal/latest/embedded_hal/)
- [rustsbi-k510](https://github.com/Gstalker/rustsbi-k510)
- [rustsbi-d1](https://github.com/rustsbi/rustsbi-d1)
- [rustsbi-hifive-unmatched](https://github.com/rustsbi/rustsbi-hifive-unmatched)
- [rustsbi-qemu](https://github.com/rustsbi/rustsbi-qemu)
- [rustsbi-k210](https://github.com/rustsbi/rustsbi-k210)
- [RISC-V 手册](http://riscvbook.com/chinese/RISC-V-Reader-Chinese-v2p1.pdf)
- [RISC-V SBI specification](https://github.com/riscv-non-isa/riscv-sbi-doc/)
- [Rust学习](https://course.rs/about-book.html)
- [riscv-rt](https://crates.io/crates/riscv-rt)
- [rCore运行指导](https://rcore-os.cn/rCore-Tutorial-Book-v3/)

