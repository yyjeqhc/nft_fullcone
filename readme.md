# This project is based on nft_fullcone, sourced from https://github.com/fullcone-nat-nftables.
# NFT Fullcone 模块和用户空间工具

## 中文版本

### 项目简介

本项目提供编译和安装 `nft_fullcone` 内核模块的说明，以及启用 `nftables` 中 Fullcone NAT 支持所需的 `libnftnl` 和 `nftables` 用户空间工具。Fullcone NAT 允许外部主机通过映射端口直接访问内部主机，提升 NAT 的灵活性。

### 前置条件

- 支持 `nf_tables` 的 Linux 内核（建议 5.10 或更高版本）。
- 构建工具：`make`、`gcc`、内核头文件（`linux-headers-$(uname -r)`）。
- Root 或 sudo 权限。

### 构建和安装内核模块

1. **编译模块**：

   - 确保位于包含 `nft_fullcone` 源代码的目录。

   - 运行：

     ```bash
     make
     ```

这将生成 nft_fullcone.ko 内核模块。

1. 加载模块

   - 将模块加载到运行中的内核：

     `sudo insmod nft_fullcone.ko`

2. 验证加载

   - 检查模块是否成功加载：

     `lsmod | grep nft_fullcone`

   - 如果输出中显示 nft_fullcone，则加载成功。

### 启用 nftables 的 Fullcone 支持

要在 nftables 规则中使用 fullcone 关键字，需要编译并安装更新的 libnftnl 和 nftables。

#### 第一步：安装 libnftnl-1.2.8

1. 进入源码目录

   `cd libnftnl-1.2.8`

2. 编译和安装

   ```shell
   ./autogen.sh
   ./configure
   make -j$(nproc)
   make install
   
   大体流程是这样，部分配置可以自定义
   ```

#### 第二步：安装 nftables

1. 进入源码目录

   `cd nft-1.1.1`

2. 编译和安装

   ```shell
   ./autogen.sh
   ./configure
   make -j$(nproc)
   make install
   
   大体流程是这样，部分配置可以自定义
   ```

3. 验证安装

   `nft --version`

   - ##### 预期输出：nftables v1.1.1 (Commodore Bullmoose #2)

### 使用示例

加载 nft_fullcone 模块并安装支持 fullcone 的 nftables 后，可以在 NAT 规则中使用 fullcone 关键字。

以下是示例：

```
nft add rule ip nat PREROUTING iifname "ens33" fullcone

nft add rule ip nat POSTROUTING oifname "ens33" fullcone
```

#### 异常情况：

```shell
nft add rule ip nat POSTROUTING oifname "ens33" fullcone
Error: syntax error, unexpected newline
```

##### 请进入nft-1.1.1，然后运行

```shell
rm src/parser_bison.c
rm src/parser_bison.h
rm src/scanner.c
make clean
make -j$(nproc)
make install
```



------

## English Version

### Project Overview

This project provides instructions to build and install the nft_fullcone kernel module, along with the necessary userspace tools (libnftnl and nftables) to enable Fullcone NAT support in nftables. Fullcone NAT allows external hosts to access internal hosts through mapped ports, enhancing NAT flexibility.

### Prerequisites

- Linux kernel with nf_tables support (version 5.10 or higher recommended).
- Build tools: make, gcc, kernel headers (linux-headers-$(uname -r)).
- Root or sudo privileges.

### Building and Installing the Kernel Module

1. Compile the Module

   - Ensure you are in the directory containing the nft_fullcone source code.

   - Run:

     `make`

   - This will generate the nft_fullcone.ko kernel module.

2. Load the Module

   - Install the module into the running kernel:

     `sudo insmod nft_fullcone.ko`

3. Verify Loading

   - Check if the module is loaded successfully:

     `lsmod | grep nft_fullcone`

   - If nft_fullcone appears in the output, the module is loaded correctly.

### Enabling Fullcone Support in nftables

To use the fullcone keyword in nftables rules, you need to compile and install updated versions of libnftnl and nftables.

#### Step 1: Install libnftnl

1. Navigate to the Source Directory

   `cd libnftnl-1.2.8`

2. Compile and Install

   ```shell
   ./autogen.sh
   ./configure
   make -j$(nproc)
   make install
   ```

   

#### Step 2: Install nftables

1. Navigate to the Source Directory

   ```shell
   cd nft-1.1.1
   ```

2. Compile and Install

   ```shell
   ./autogen.sh
   ./configure
   make -j$(nproc)
   make install
   ```

3. Verify Installation

   `nft --version`

   - Expected output: nftables v1.1.1 (Commodore Bullmoose #2).

### Usage Examples

Once the nft_fullcone module is loaded and nftables is installed with fullcone support, you can use the fullcone keyword in NAT rules. Below are examples:

```shell
nft add rule ip nat PREROUTING iifname "ens33" fullcone

nft add rule ip nat POSTROUTING oifname "ens33" fullcone
```

#### Exception situation：

```shell
nft add rule ip nat POSTROUTING oifname "ens33" fullcone
Error: syntax error, unexpected newline
```

#### Enter the dir nft-1.1.1,then run 

```shell
rm src/parser_bison.c
rm src/parser_bison.h
rm src/scanner.c
make clean
make -j$(nproc)
make install
```

### Acknowledgments

- Based on the nftables project: https://netfilter.org/projects/nftables/

- libnftnl project: https://netfilter.org/projects/libnftnl/

- # nft_fullcone project: https://github.com/fullcone-nat-nftables/


