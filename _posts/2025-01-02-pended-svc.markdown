---
layout: post
title:  "Pended SVC"
date:   2025-01-02 00:08:00 -0800
categories: 365-days-of-code rtos-dev
---
Today I updated the LunaRTOS kernel to utilize the Pended SVC (PendSV) feature for context switching. [PendSV](https://developer.arm.com/documentation/107706/latest/System-exceptions/Pended-SVC---PendSV) is an ARM Cortex-M feature that allows the software to trigger an exception to perform a context switch from one thread to another. I originally used SysTick for context switching in LunaRTOS, but this can cause issues if SysTick is needed for other OS-related tasks.
 
Both PendSV and SysTick are system-level interrupts supported by the ARM Cortex-M. It's best to re