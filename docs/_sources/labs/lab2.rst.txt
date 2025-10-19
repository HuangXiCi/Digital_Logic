实验二 Verilog设计与仿真
==========================================

Verilog 是一种用于描述、设计电路的 **硬件描述语言 HDL (Hardware Description Language)** ，
在实验一，我们已经尝试在 logisim 中设计并绘制电路图，并通过输入输出测试电路的功能。
本次实验我们使用 Verilog 的各种基础语法完成一些电路设计，并通过仿真代码和波形图测试电路的功能。

为此，我们需要使用 Verilog 仿真器，常见的仿真器有很多：VCS 、modelsim 、Verilog-XL 、iverilog 、verilator 等。
考虑到后续实验我们将使用 Xilinx 的 FPGA 教学实验板，为此我们直接使用 Vivado 的 xsim 仿真器，能够在一体式软件中完成整个实验的流程。

本次实验课我们使用 Verilog HDL 设计加法器等一些简单的电路，并编写 Testbench 程序，
使用 Vivado 的仿真器测试我们设计的电路。

Vivado 教程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Vivado 是 Xilinx 公司的 FPGA 集成设计环境，本次实验我们将使用 Vivado 对 Verilog 设计进行仿真。
我们推荐使用 Vivado 2018.3 ，安装 WebPACK 版本。
该版本安装体积约 20 GB，而近几年的版本安装体积已经约 100 GB，WebPACK 是免费的，不需要许可证的版本，并且已经能够满足我们的实验需求。
如果你此前已经安装 2015 年之后的版本，都是能够满足完成实验的。Vivado 不同的版本之间功能差异也比较小，在学习和使用上也不会造成困难。

创建 adder 工程
-------------------------------

打开 Vivado 软件，会来到 Vivado 软件初始界面

.. figure:: ../picture/lab2/vivado_home.png
   :alt: vivado_home
   :align: center


Vivado 软件很复杂，我们一点点来了解它。创建一个名为 adder 的新项目，并保存在合适的位置， **一定要是全英文的目录** 。
勾选 ``Create project subdirectory`` 则会在保存新项目的地方创建一个项目名称的文件夹，用于存放项目的文件。
你也可以手动创建一个项目名称的文件夹，作为保存项目的位置，就不勾选该选项了。

.. figure:: ../picture/lab2/vivado_genprj.png
   :alt: vivado_genprj
   :align: center


来到器件选择页面， ``Family`` 系列选择 ``Artix-7`` ， ``Package`` 封装方式选择 ``fgg484`` ，
然后选择 ``xc7a100tfgg484`` ，完成器件选择，其余的步骤直接下一步即可。

.. figure:: ../picture/lab2/vivado_device.png
   :alt: vivado_device
   :align: center


完成创建项目，来到该项目初始页面，本次实验内容我们只需要关心红色方框标记出来的区域。
左侧是 ``Flow Navigator`` 流程导航，我们完整的实验整个流程就是依次从上往下进行的。
左上角是 ``项目管理`` ，目前由于正处于项目管理界面，因此 ``PROJECT MANAGER`` 是蓝色的。

.. figure:: ../picture/lab2/vivado_prj.png
   :alt: vivado_prj
   :align: center


Verilog 代码编写
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

工欲善其事，必先利其器。
一个好的代码 Coding 环境能极大的帮助你编写代码。

VS Code + Verilog-HDL/SystemVerilog/Bluespec SystemVerilog + ctags 插件
-----------------------------------------------------------------------------

Vivado 作为一个集成开发环境，拥有代码编辑能力。不过作为一个大型软件，响应速度相对较慢，
编写代码也没有插件那么方便。因此我们平时都是使用编辑器编写代码，当然有很多好用的编辑器，
这里介绍 ``VS Code`` + ``Verilog-HDL/SystemVerilog/Bluespec SystemVerilog`` + ``ctags`` 插件。

安装好 ``Verilog-HDL/SystemVerilog/Bluespec SystemVerilog`` 和 ``ctags`` 插件后，可以为 Verilog 等
语言提供基础的高亮和语法框架支持，还可以提供 ``动态语法检查`` 、鼠标悬停查看信号定义、跳转信号和模块等功能。相信对经常写代码的你们
来说，这些功能不会陌生。你们可以在 `这里 <https://dphweb.cn/index.php/2023/08/22/verilog-hdl%e6%8f%92%e4%bb%b6%e9%85%8d%e7%bd%ae%e6%95%99%e7%a8%8b/>`_ 学习
配置教程，并不复杂。

Ripple-carry Adder 行波进位加法器
------------------------------------------------------

行波进位加法器是最基本的数字电路加法器实现，它通过串联多个全加器来实现多位二进制数的加法操作。
这里我们给出一种 Full Adder 全加器的 Verilog 实现方式参考:

.. code-block:: v
   :caption: Full Adder 全加器的 Verilog 实现方式参考
   :emphasize-lines: 12
   :linenos:

   module ref_fa (
       a       // <<i<<
      ,b       // <<i<<
      ,cin     // <<i<<
      ,sum     // >>o>>
      ,cout    // >>o>>
   );
      input a, b, cin;
      output sum, cout;
      wire a, b, cin;
      wire sum, cout;
      assign {cout, sum} = a + b + cin;
   endmodule


.. raw:: html

   <div class="admonition mytodo">
      <p class="admonition-title">全加器</p >
      <p>按照门电路的方式用 Verilog 描述全加器电路，即不使用 + 运算符。
      完成编写 fa.v 文件， module 名字为 fa ，与文件名保持一致，
      端口名也可与参考代码一致。</p >
   </div>


第一个 Testbench 程序
----------------------------------------

Verilog 代码设计完成后，还需要进行重要的步骤，即逻辑功能仿真，仿真激励文件称之为 testbench，
就像你之前使用 Logisim 设计完电路之后，需要你输入信号，观察输出，检查你的电路设计功能是否符合你的要求。

我们从一个全加器的简单 Testbench 入手，编写你自己的第一个 Testbench 程序。

.. code-block:: v
   :caption: 一个全加器的简单 Testbench
   :emphasize-lines: 1, 6-12, 21
   :linenos:

   `timescale 1ns/1ps
   module ref_tb ();
      reg [2:0] in;
      wire sum, cout;

      ref_fa u_ref_fa (
          .a       (in[0]) // <<i<<
         ,.b       (in[1]) // <<i<<
         ,.cin     (in[2]) // <<i<<
         ,.sum     (sum)   // >>o>>
         ,.cout    (cout)  // >>o>>
      );

      initial  begin
         in = 3'b0;
         #100;
         for (integer i = 0; i < 8; i = i + 1)  begin
            in = in + 1;
            #100;
         end
         $stop;
      end

   endmodule


相信你理解这个程序并不会感到困难，
在 Logisim 中，你可以把一个画布中的电路引出输入输出口，封装成模块，然后在另一个画布中放置这个模块。
其中高亮的一大段，代表对 ``ref_fa`` 进行实例化，即在另一个模块中使用这个模块，然后将这个模块的信号与
对应的信号相连。 ``.a`` 代表 ref_fa 中的 a 端口，后面括号中的 in[0] 是 ref_tb 中的信号，这样就完成了相连的操作。

然后我们对 in[2:0] 信号进行驱动，也就是对它进行赋值操作，我们直接使用 for 循环进行遍历所有的输入情况，然后你可以对比
所有的输入是否会得到正确的输出，即可完成 Testbench 仿真测试。

Vivado 功能仿真验证
-------------------------------------

完成了加法器和 Testbench 程序的编写，我们就可以进行仿真验证了。

首先需要将编写好的源代码添加到工程中，我们可以通过这两个地方添加源文件。

.. figure:: ../picture/lab2/add_source.png
   :alt: add_source
   :align: center


