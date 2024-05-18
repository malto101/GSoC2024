---
layout: default
title: Home
nav_order: 1
description: "The official Google Summer of Code 2024 blog that will be used to keep research, progress diaries, and documentation up to date."
permalink: /
has_children: false
---

# Upstream Zephyr Support on BeagleBone AI-64 R5

Introducing Zephyr RTOS support for the BeagleBone AI-64 platform, with the aim of upstreaming it to the Zephyr repository. Our focus will be on enhancing driver support for essential communication protocols on the TDA4VM SoC. Additionally, we plan to implement inter-process communication (IPC) mechanisms to facilitate message queuing between the A72 core and R5 cores, thereby enabling efficient communication and coordination within the system. This endeavor seeks to empower developers with a robust real-time operating system foundation and comprehensive hardware support for embedded applications on the BeagleBone AI-64 platform.

## About

- **Contributor:** [Dhruv Menon](https://github.com/malto101)
- **Mentors:** [Dhruva Gole](https://github.com/DhruvaG2000), [Nishanth Menon](https://github.com/nmenon), [Andrew Davis](https://github.com/glneo)  
- **Code:** 
- **Documentation:** https://malto101.github.io/GSoC2024/ 
- **GSoC Entry:** https://summerofcode.withgoogle.com/programs/2024/projects/IBlfhKXK
- **Weekly Progress Report:** 

## Introduction <a name="intro"></a>

The [Beaglebone AI 64](https://www.beagleboard.org/boards/beaglebone-ai-64) platform is a formidable tool for developing embedded systems, Zephyr OS support will allow it to reach even greater potential. Zephyr is a lightweight real-time operating system that improves embedded applications’ scalability, flexibility, and dependability.

Part of the [Beaglebone AI 64](https://www.beagleboard.org/boards/beaglebone-ai-64) platform, the System-on-Chip (SoC) has an intricate architecture that includes two Arm Cortex-A72 cores and four Arm Cortex-R5F cores. An addditional two Arm Cortex-R5F cores are located on a different MCU island. Such configurations epitomize asymmetric multiprocessing (AMP), facilitating the concurrent operation of a full-fledged operating system (OS) alongside a real-time operating system (RTOS).

The suggested approach uses [remoteproc](https://gsoc.beagleboard.io/proposals/melta101.html#remoteproc) from the Linux-powered A72 core to load the Zephyr RTOS onto the BeagleBone AI 64. Upstreaming Zephyr support for the BeagleBone AI 64 platform is the main goal of this project, which builds on the foundation set by the previous Google Summer of Code (GSoC) contributor which already includes crucial support for features like the [VIM interrupt controller](https://github.com/zephyrproject-rtos/zephyr/pull/60856), [DM timer](https://github.com/zephyrproject-rtos/zephyr/pull/61020) for systick, and [Davinci](https://github.com/zephyrproject-rtos/zephyr/pull/61316) GPIO controller on the J721E SoC. Defining Kconfig based existing board support and using the current Hardware Model v2(HWMv2) instead of previous OOT(Out of the Tree) model while Adding major drivers to allow seemless interaction such as I2C, SPI, mailbox. In addition, the project attempts to show seamless communication across cores by introducing example instances of Inter-Process Communication (IPC) for example, we could use message queues to synchronize events.