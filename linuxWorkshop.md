---
title: "Linux Essentials - Part 1"
author: "GDG Workshop"
theme:
  name: gruvbox-dark
  override:
    footer:
      style: template
      left:
        image: abc.png
      right: "{current_slide} / {total_slides}"
      height: 5
options:
  implicit_slide_ends: true
  command_prefix: "cmd"
---

# Part 1: Introduction & Foundations 🚀

## Today's Journey

**What We'll Cover:**
1. The Evolution: From Mainframes to Linux
2. Understanding the Terminology (CLI, Shell, Terminal, GUI)
3. Kernel Deep Dive
4. Why Linux is the Best
5. GNU & The Free Software Movement
6. Linux Distributions

<!-- end_slide -->

# The Beginning 🕰️
## How It All Started

## The 1960s: Birth of Computing

**The Mainframe Era**
* Computers were room-sized machines
* Cost millions of dollars
* Only universities and corporations had access
* No personal computers existed
* Operated using punch cards!

**The Problem:** Multiple users needed to share ONE computer.

<!-- end_slide -->

## 1969: UNIX is Born 🌟

**At Bell Labs (AT&T)**
* Ken Thompson & Dennis Ritchie
* Created for the PDP-7 minicomputer
* Revolutionary idea: Time-sharing system
* Written in C language (portable!)

**UNIX Philosophy:**
> "Do one thing and do it well"

<!-- end_slide -->

## UNIX's Key Innovations

**What Made UNIX Special:**
1. **Multi-user:** Multiple people could use it simultaneously
2. **Multi-tasking:** Run multiple programs at once
3. **Portable:** Could run on different hardware
4. **Hierarchical File System:** The `/` tree structure
5. **Everything is a file:** Even devices!

<!-- end_slide -->

## The Problem with UNIX

**The Catch:**
* UNIX was **proprietary**
* Extremely **expensive** (tens of thousands of dollars)
* Source code was **closed**
* Required expensive hardware
* Licensing restrictions were strict

Universities wanted to teach it, but couldn't afford it!

<!-- end_slide -->

## 1983: The GNU Project 🦬

**Richard Stallman's Vision**
* Started the Free Software Foundation
* Goal: Create a **free** UNIX-like OS
* GNU = "GNU's Not Unix" (recursive acronym!)

**The Problem:** They built all the tools but... no kernel!

**GNU Created:**
* GCC (compiler)
* bash (shell)
* Core utilities (ls, cp, mv, etc.)

<!-- end_slide -->

## 1987: MINIX Enters

**Andrew Tanenbaum** created MINIX
* Teaching tool for operating systems
* UNIX-like, but simplified
* Source code available (for education)

A Finnish student named **Linus Torvalds** was using it...

<!-- end_slide -->

## 1991: Linux is Born! 🐧

**August 25, 1991 - Linus Torvalds posts:**

> "I'm doing a (free) operating system (just a hobby, won't be big and professional like GNU)"

**What Linus Created:**
* The **kernel** - the core of the OS
* Released under GPL (completely free)
* Invited collaboration from anyone
* Combined with GNU tools = Complete OS!

<!-- end_slide -->

# Understanding the Terms 📚
## CLI, Shell, Terminal, GUI

<!-- end_slide -->

## GUI vs CLI

**GUI (Graphical User Interface)**
* Windows, icons, buttons, mouse
* What you're used to: Windows, macOS
* Easy to learn, visual feedback
* Limited automation capabilities

**CLI (Command Line Interface)**
* Text-based interaction
* Type commands, get text output
* Steeper learning curve
* Powerful automation & scripting

<!-- end_slide -->

## What is a Terminal? 🖥️

**Historical Context:**
* Originally a physical device
* Connected to mainframe computers
* Had a keyboard and display
* Multiple terminals → One computer

**Today:**
* **Terminal Emulator** = Software that mimics old terminals
* Examples: GNOME Terminal, iTerm2, Windows Terminal
* Just a window that runs a shell!

<!-- end_slide -->

## What is a Shell? 🐚

**The Shell is Your Interpreter**
* Takes your commands (text)
* Translates them for the kernel
* Returns the output to you

**Think of it as:**
* The shell is the **translator**
* You speak human → Shell speaks machine

<!-- end_slide -->

## The Relationship 🔗

```text
┌─────────────────────────────────┐
│      Terminal Emulator          │  ← The window you see
│  ┌───────────────────────────┐  │
│  │        Shell (bash)       │  │  ← Interprets commands
│  │  ┌─────────────────────┐  │  │
│  │  │   Your Command      │  │  │  ← What you type
│  │  └─────────────────────┘  │  │
│  │           ↓                │  │
│  │  ┌─────────────────────┐  │  │
│  │  │    Kernel           │  │  │  ← Executes on hardware
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

<!-- end_slide -->

## CLI in Action

**Example Workflow:**

1. You open **Terminal** (the app)
2. **Shell** (bash) starts and waits
3. You type: `ls -la`
4. **Shell** interprets this
5. **Kernel** executes the command
6. Output returns through Shell to Terminal
7. You see the result!

<!-- end_slide -->

# Kernel vs Operating System 🧠
## What's the Difference?

<!-- end_slide -->

## What is a Kernel? ⚙️

**The Kernel is the CORE**
* Sits between hardware and software
* Manages system resources
* Direct communication with hardware
* Most privileged software component

**Key Responsibilities:**
1. Process management
2. Memory management
3. Device drivers
4. System calls
5. Security & permissions

<!-- end_slide -->

## Kernel's Job Explained

**Think of the Kernel as a Restaurant Manager:**

* **Hardware** = Kitchen (CPU, RAM, Disk)
* **Applications** = Customers (programs)
* **Kernel** = Manager coordinating everything

**Example:**
When you open a browser:
* Kernel allocates RAM
* Kernel schedules CPU time
* Kernel handles network access
* Kernel manages disk I/O

<!-- end_slide -->

## Linux: The Kernel 🐧

**Technically Speaking:**
* "Linux" is JUST the kernel
* Created by Linus Torvalds
* Manages hardware resources
* ~30 million lines of code!

**But everyone says "Linux OS" because:**
* Linux kernel + GNU tools = Complete system
* Common shorthand
* "GNU/Linux" is technically correct but rarely used

<!-- end_slide -->

## Kernel Architecture

```text
┌────────────────────────────────────┐
│      User Space (Applications)     │
├────────────────────────────────────┤
│         System Call Interface      │
├────────────────────────────────────┤
│                                    │
│         Linux Kernel               │
│  ┌──────────┬──────────┬────────┐ │
│  │ Process  │  Memory  │  File  │ │
│  │   Mgmt   │   Mgmt   │ System │ │
│  ├──────────┼──────────┼────────┤ │
│  │      Device Drivers            │ │
│  └────────────────────────────────┘ │
├────────────────────────────────────┤
│    Hardware (CPU, RAM, Disk, etc)  │
└────────────────────────────────────┘
```

<!-- end_slide -->

## System Calls: The Bridge 🌉

**How Programs Talk to Kernel:**
* Applications can't directly access hardware
* They make "system calls" to the kernel
* Kernel validates and executes safely

**Example: Opening a File**
```c
// Your program does this:
int fd = open("/home/user/data.txt", O_RDONLY);

// Behind the scenes:
// 1. System call to kernel
// 2. Kernel checks permissions
// 3. Kernel accesses disk
// 4. Returns file descriptor
```

<!-- end_slide -->



# Why Linux is the Best 🏆
## Dominating the Tech World

<!-- end_slide -->

## Linux Runs Everything

**Where Linux Dominates:**
* **96.3%** of top 1 million web servers
* **100%** of top 500 supercomputers
* **Billions** of Android devices (Linux kernel)
* Most cloud infrastructure (AWS, Google Cloud, Azure)
* Mars rovers, ISS systems, CERN particle accelerators
* Your smart TV, router, car infotainment system
* IoT devices everywhere

**If you used the internet today, you used Linux!**

<!-- end_slide -->

## Why Linux is Superior

**1. Freedom & Cost**
* Completely free (no licensing fees)
* Full access to source code
* Modify and redistribute as you wish
* No vendor lock-in or forced upgrades

**2. Security & Privacy**
* Open source = thousands review the code
* Fast security patches from global community
* Granular permission system
* No telemetry or data collection by default
* Less malware targeting Linux

<!-- end_slide -->

## Why Linux is Superior (cont.)

**3. Stability & Performance**
* Servers run for years without reboot
* Efficient resource usage
* Scales from Raspberry Pi to supercomputers
* No forced updates interrupting your work
* Choose when and what to update

**4. Total Customization**
* Change absolutely everything
* Pick your desktop environment
* Mix and match components
* Build your own distribution
* Make it look and work exactly how you want

<!-- end_slide -->

## Why Linux is Superior (cont.)

**5. Developer Paradise**
* Best programming tools included free
* Package managers for instant software installs
* Native Docker and container support
* Every major language available
* Built-in remote access (SSH)
* Version control (git) integrated everywhere

**6. Community Power**
* Massive global community helping each other
* Free support on forums, IRC, Discord
* Extensive documentation and tutorials
* Thousands of contributors improving it daily

<!-- end_slide -->

## The Numbers Don't Lie 📊

**Linux Development:**
* 30+ million lines of code
* 20,000+ contributors worldwide
* 15,000+ companies involved
* New kernel version every 9-10 weeks
* Most actively developed OS in history

**The Internet Runs on Linux:**
* Google, Facebook, Amazon - all Linux
* Netflix, Spotify, YouTube - Linux servers
* Stock exchanges - Linux
* Banking systems - Linux

<!-- end_slide -->

## Real World Impact 🌍

**Science & Research:**
* CERN's Large Hadron Collider
* Human Genome Project
* Climate modeling systems
* Space exploration (NASA, SpaceX)

**Entertainment:**
* Hollywood render farms (Pixar, Disney)
* Game servers
* Streaming infrastructure

**Critical Infrastructure:**
* Air traffic control
* Power grids
* Telecommunications
* Financial systems

**Linux literally runs the modern world!**

<!-- end_slide -->

# GNU & The Movement 🦬
## The Missing Piece

<!-- end_slide -->

## What is GNU?

**GNU = GNU's Not Unix** (recursive acronym!)
* Started 1983 by Richard Stallman
* Goal: Free UNIX replacement
* Built all the essential tools

**The GNU Toolkit:**
* bash, GCC, coreutils (ls, cp, mv)
* glibc, Make, grep, sed, awk
* Everything except... the kernel!

<!-- end_slide -->

## The Four Freedoms 🕊️

**Free Software means:**

0. **Run** - Use for any purpose
1. **Study** - Examine source code
2. **Share** - Redistribute copies
3. **Improve** - Modify and share changes

**"Free as in freedom, not just free beer"**

<!-- end_slide -->

## GNU + Linux = Perfect Match 🤝

**The Problem (1991):**
* GNU had: compiler, shell, tools ✅
* GNU needed: a working kernel ❌
* Linus had: a working kernel ✅
* Linux needed: all the tools ❌

**The Solution:**
```
Linux Kernel + GNU Tools = Complete Free OS
```

Most people call it "Linux", technically it's "GNU/Linux"

<!-- end_slide -->

# Let's Get Hands-On! 🚀

**Next Section:** Navigation & File System Basics
<!-- end_slide -->
