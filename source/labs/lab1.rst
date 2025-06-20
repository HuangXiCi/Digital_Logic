实验一 异或门
============================

异或门，英文为Exclusive-OR gate，简称XOR。当两个输入相同时，异或门的输出为0，反之为1。

下图是异或门符号和真值表，包括逻辑门的3种符号：形状特征型符号（ANSI/IEEE Std 91-1984）、IEC矩形国标符号（IEC 60617-12）和不再使用的DIN符号（DIN 40700）。

.. figure:: ../picture/lab1/XOR_truthtable.png
   :alt: 异或门符号和真值表
   :align: center

图 Fig.1 异或门符号和真值表

本次实验将使用Logisim仿真平台，通过与或非门电路相连实现，实现异或门等功能。最后在Logisim中自行设计搭建满足实验要求的电路，进行仿真测试，检查电路是否符合要求。

Logisim教程
~~~~~~~~~~~~~~~~~~~~~

Logisim 是用于设计和仿真数字逻辑电路的图形化教学仿真工具。你可以摆放逻辑门电路或者电路模块，连接它们，来设计数字电路并仿真运行。本次实验我们将使用 Logisim 完成实验，后续实验课程将不使用它，但依然鼓励你使用它设计并搭建电路。它以一种直观的方式展现数字电路的设计与功能，方便你熟悉各种电路设计，为学习数字逻辑以及后续的实验课程奠定良好的基础。

搭建 Logisim 环境
-----------------------

Logisim 一个开源的 (GPL) ，基于 Java 虚拟机运行的软件，需要 Java5 或更高版本，推荐 Java17 版本。它可以运行在Windows、MacOS、Linux上。

搭建 Logisim 环境可以在 `这里 <https://soc.ustc.edu.cn/Digital/2024/lab0/logisim/>`_ 参考USTC的搭建教程，里面有配置 Java 环境和下载 Logisim 相关内容。

你可以在 `这里 <https://github.com/logisim-evolution/logisim-evolution/releases>`_ 下载 logisim-evolution ，它是 Logisim 的升级版，提供了更丰富的模块。也可以在 `这里 <https://github.com/SecondCat/Logisim-Chinese-version/releases/>`_ 下载自带 Java 环境的Windows版本 Logisim 。本次实验课不会对 Logisim 的版本有要求，你可以自行选择安装的版本和方式。

熟悉 Logisim 界面
-----------------------

搭建你的第一个电路
-----------------------

