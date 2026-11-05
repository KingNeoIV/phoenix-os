# Vision

## 1. Why I'm doing this

I'm building this because I want to learn how to create my own operating systems along with my own PCB boards and CPUs. The main goal is to create a business of my own.

The way that I see it is that the operating system is the soul or personality of the machine and the CPU is the brains. I'm currently in college now getting my Bachelor of Science in Computer Science and I'm projected to graduate in Spring 2028.

The whole purpose of building it myself is to learn how to create something that I can call my own. Maybe along the way too I'll be able to invent or discover something. All while leaving my mark and contributions to the world.

## 2. The ladder

### Step 1: Boot sector and tiny kernel

- **What I think this is**: The boot sector is the first 512 bytes of a disk, programmed to load the kernel and begin its operations. The tiny kernel is going to be able to perform minimal tasks such as printing text, taking keyboard input, and keeping track of free memory. Along with all necessary tools in order to run a simple shell.

- **What I don't understand yet**: I have a very minimal theoretical practice/approach to this because I've only read and studied this topic for my COSC 4337 Operating Systems along with the textbook that was required for it Operating System Concepts by A. Silberschatz, P.B. Galvin, and G. Gagne. Some things I still want to figure out:

  - What happens between pressing the power button and the first instruction running?
  - How does the BIOS find the boot sector?
  - How do you print a character when there is no operating system to ask?

- **When it works, I'll know**:

  - First, a letter prints to the screen from my own boot sector, running in QEMU.
  - Then my own custom kernel PhoenixOS prints "hello."
  - Later, it can handle I/O, starting with the keyboard.

## 3. What I'm borrowing for now

- **gcc (compiler)**: borrowed until I build my own, after Shia16 works
- **NASM (assembler)**: borrowed
- **QEMU (emulator)**: borrowed, since I don't plan to replace it
- **Limine (bootloader)**: borrowed from M1 on. I write my own boot sector in M0 to learn how it works.