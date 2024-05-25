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
- **Weekly Progress Report:** https://forum.beagleboard.org/t/weekly-progress-report-thread-upstreaming-zephyr-support-on-beaglebone-ai-64/38509/2

## Introduction <a name="intro"></a>

The [Beaglebone AI 64](https://www.beagleboard.org/boards/beaglebone-ai-64) platform is a formidable tool for developing embedded systems, Zephyr OS support will allow it to reach even greater potential. Zephyr is a lightweight real-time operating system that improves embedded applications’ scalability, flexibility, and dependability.

Part of the [Beaglebone AI 64](https://www.beagleboard.org/boards/beaglebone-ai-64) platform, the System-on-Chip (SoC) has an intricate architecture that includes two Arm Cortex-A72 cores and four Arm Cortex-R5F cores. An addditional two Arm Cortex-R5F cores are located on a different MCU island. Such configurations epitomize asymmetric multiprocessing (AMP), facilitating the concurrent operation of a full-fledged operating system (OS) alongside a real-time operating system (RTOS).

The suggested approach uses [remoteproc](https://gsoc.beagleboard.io/proposals/melta101.html#remoteproc) from the Linux-powered A72 core to load the Zephyr RTOS onto the BeagleBone AI 64. Upstreaming Zephyr support for the BeagleBone AI 64 platform is the main goal of this project, which builds on the foundation set by the previous Google Summer of Code (GSoC) contributor which already includes crucial support for features like the [VIM interrupt controller](https://github.com/zephyrproject-rtos/zephyr/pull/60856), [DM timer](https://github.com/zephyrproject-rtos/zephyr/pull/61020) for systick, and [Davinci](https://github.com/zephyrproject-rtos/zephyr/pull/61316) GPIO controller on the J721E SoC. Defining Kconfig based existing board support and using the current Hardware Model v2(HWMv2) instead of previous OOT(Out of the Tree) model while Adding major drivers to allow seemless interaction such as I2C, SPI, mailbox. In addition, the project attempts to show seamless communication across cores by introducing example instances of Inter-Process Communication (IPC) for example, we could use message queues to synchronize events.

##  But why Zephyr?

[Zephyr](https://docs.zephyrproject.org/latest/index.html) RTOS (Real time Operating System) is a robust and flexible option for developing embedded systems, including real-time functionality, scalability, portability, security, flexibility, and a vibrant community. Zephyr offers the essential capabilities and tools to develop embedded applications effectively and dependably, regardless of the complexity of the device—from basic sensor nodes to intricate IoT devices. The Cortex R5 processor core present on the TDS4VM are built to provide deeply embedded real-time and safety-critical systems. Thus, adding Zephyr RTOS support for R5 cores in TDA4VM will be very helpful for the product developers.

## remoteproc

The remote processor ([rproc](https://docs.kernel.org/staging/remoteproc.html)) framework serves as a robust solution for managing remote processor devices within modern System-on-Chips (SoCs) and is particularly geared towards heterogeneous multicore processing (HMP) setups. This innovative framework provides the means to control various aspects of remote processors, such as **loading and executing firmware**, all while effectively abstracting the underlying hardware differences. Furthermore, it extends its utility by offering services for monitoring remote coprocessors, thereby ensuring management and operation.

The Linux running on the Cortex-A72 uses the remoteproc framework to manage the Cortex-R5F co-processor. Therefore, the binary to be copied to the SD card to allow the A53 cores to load it while booting using remoteproc.

## Remote Processor Messaging (rpmsg) Framework

Typically AMP(Asymmetric Multiprocessing Processor) remote processors employ dedicated DSP codecs and multimedia hardware accelerators, and therefore are often used to offload CPU-intensive multimedia tasks from the main application processor. These remote processors could also be used to control latency-sensitive sensors, drive random hardware blocks, or just perform background tasks while the main CPU is idling. Rpmsg is a **virtio-based messaging bus** that allows kernel drivers to communicate with remote processors available on the system. In turn, drivers could then expose appropriate user space interfaces, if needed. is a message passing mechanism that requests resources through [remoteproc](https://gsoc.beagleboard.io/proposals/melta101.html#remoteproc) and builds on top of the virtio framework. Shared buffers are requested through the resource_table and provided by the remoteproc module during Zephyr firmware loading





