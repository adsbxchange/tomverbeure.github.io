---
layout: post
title:  "JTAG for FPGA Development - JTAG_GPIO and OpenOCD"
date:   2018-05-03 22:00:00 -0700
categories: JTAG
---

# Introduction

During the development of an FPGA project, you sometimes want to be able to control design internal nodes from your
host PC, or observe internal state.

Even if you plan to have a real control interface on your final board, such as a USB to serial port, that interface
may not yet be ready. Or maybe you want to debug the functionality of that interface itself, so you need another
side band way to access your information.

When you're using Altera tools (or the Xilinx equivalent), you could use In-System Sources and Probes: with a small
amount of effort, you can create a block with a user specified number of control and observability points, instantiate
that block in your design, and use a Quartus GUI to control and observe, or you can bypass the GUI and control and observe
the nodes through a TCL API.

The communication between the design and the host PC happens over JTAG.

You could also add a SignalTap logic analyzer to really see what's going on in the design.
Or add a Nios2 processor with a JTAG UART to transfer data from the PC to the FPGA and back.

The problem with all of this is that it's all very proprietary.

Altera doesn't publish the protocol by which it transfers various kinds of data over JTAG.  It works great in your development 
environment, but it 

what if you want to make all of this work on Altera, Xilinx, and Lattice
FPGAs? Or what if you want to make your own JTAG controlled block in our design? Maybe you create your own little CPU and 
want an way to download binaries to the CPU memory? Maybe you've even want to make your CPU debuggable over GDB?

For reasons like this, it may be useful to roll your own JTAG. 

And if you do that, one of the immediate question that follows is: how will I control those JTAG capable block in my design from
my host PC.

# JTAG_GPIO

To make things easy, I created a really simply JTAG block, one that is essentiall a copy of the functionality of the Altera In-System
Sources and Probes: JTAG_GPIO. You instantiate the block in your design, connect it on one side to a JTAG TAP (Test Access Port) and
connect the gpio_output signals to logic nodes you want to control, and the gpio_input signals to nodes you want to observe.

You can find the source of this block in [```jtag_gpios.v```](https://github.com/tomverbeure/jtag_gpios/blob/master/rtl/jtag_gpios.v) 
on GitHub.


