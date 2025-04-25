## 写在前面  
本项目是为了让新手也能快速上手计算机体系结构相关的知识，同时介绍开源的数字EDA工具链，考虑现在all in ai的背景，本项目还会添加一些AI芯片的上手学习指南。  

## RISCV芯片设计  
推荐[一生一芯](https://ysyx.oscc.cc/docs/2306/preliminary/preliminary.html)。这里面包含了RISCV架构的芯片设计，以及如何运行一个操作系统在你自己设计的CPU上。  

## 开源EDA工具链  
HDL推荐verilog和chisel，verilog语法更简单，chisel其实是一个verilog生成器，但是包含了面向对象的内容在里面，开发时代码可读性以及参数化都更好，不过学习成本更大。  
仿真推荐verilator，不建议直接sudo apt install verilator。  最高从源码构建，保证最新版。  
波形查看推荐gtkwave，推荐保存fst格式，大小比vcd小很多。  
上述工具推荐自己写一个makefile脚本，减少不必要的命令行输入。  

## AI学习推荐  
首选[动手学深度学习](https://zh.d2l.ai/)，但是推荐只用看前七章，大概知道什么是全连接，什么是卷积，然后推荐看[李沐大神讲transformer](https://www.youtube.com/watch?v=nzqlFIcCSWQ)，虽然这些资料都是沐神写的，但是建议中间RNN那些就不急着看了，直接一步到位transformer。  

##写在最后  
其实现在有了大模型过后，更加推荐的是学一门语言，首先了解它的工程是怎么构建的，比如python是先搭虚拟环境，scala是一个工程文件夹对应一个环境，然后了解它的变量如何命名，类如何继承，函数如何调用，这些尽量在两个小时之内搞定，把这些大概了解清楚就直接上手做项目，避免前置学习花费过多时间，后续的问题可以在实现过程中与cursor或者github copilot等边解决问题边学。