# Project: PhoenixOS and Shia16

A tiny learning/hobby OS and programming language built from the ground up, named after my kids.

## Why this exists

This project is for learning purposes and is great for a portfolio project. I've named the OS and programming language after my sons Phoenix and Shia. This project is necessary to understand how hardware and software connect and is used towards understanding making systems for custom engineered CPUs, PCBs, and other devices such as robots.

## What's inside

### PhoenixOS
PhoenixOS is going to be the baby in the womb figuratively speaking. The very first step is getting to understand the very fundamentals of operating systems. It will later grow into a fully fledged operating system that will be mainly used on robotic or computer hardware.

### Shia16
Shia16 is going to be the programming language that will help create software for the OS. All this is gearing towards learning how to create a system for robotics.

## Status

- [ ] Milestone 0: learn/create a bootloader that will print a letter in QEMU
- [ ] Milestone 1: kernel prints "hello"
- [ ] Milestone 2: serial output for debugging
- [ ] Milestone 3: interrupts and keyboard input
- [ ] Milestone 4: memory manager
- [ ] Milestone 5: tiny shell
- [ ] Shia16: first interpreter

## Getting started

This project is in the design phase, so there's nothing to build yet.

## Repo layout

```text
phoenix-os/
    docs/       design notes and vision
    boot/       boot sector and bootloader
    kernel/     the PhoenixOS kernel
    shia16/     the Shia16 language
```

## Resources

- The C Programming Language by: Brian W. Kernighan & Dennis M. Ritchie
- Code: The Hidden Language of Computer Hardware and Software by: Charles Petzold
- Operating Systems: Three Easy Pieces by: Remzi H. Arpaci-Dusseau & Andrea C. Arpaci-Dusseau
- Low-Level Programming by: Igor Zhirkov
- The Elements of Computing Systems by: Noam Nisan & Shimon Schocken
- [OSDev wiki](https://wiki.osdev.org)
- [OSDev: Limine Bare Bones](https://wiki.osdev.org/Limine_Bare_Bones)