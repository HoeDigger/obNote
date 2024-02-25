# 准备工作

## 1. 交叉编译工具链
    

要交叉编译arm64的镜像，必须安装aarch64-linux-gnu-gcc，但他的apt包名字比较吊诡(gcc在前）

```Bash
sudo apt update
sudo apt install gcc-aarch64-linux-gnu
```

## 2. Qemu
    

GitLab Repository: https://gitlab.com/qemu-project/qemu

下载qemu仓库到本地后，切换到较新的stable分支stable-8.0 (apt install的方式装的qemu可能版本太老，没有尝试）

```Bash
git clone https://gitlab.com/qemu-project/qemu.git
git checkout stable-8.0
```

编译安装(可能有一些库需要提前安装，后续在新电脑上再装时再补充）

```Bash
./configure --target-list=x86_64-softmmu,x86_64-linux-user,arm-softmmu,arm-linux-user,aarch64-softmmu,aarch64-linux-user --enable-kvm --enable-virtfs --enable-debug
make -j16
sudo make install
# 查看版本
qemu-system-aarch64 --version
```

## 3. U-Boot
    

下载u-boot

```Bash
git clone https://source.denx.de/u-boot/u-boot.git
```

若uboot需用semihosting方式加载内核和dtb，则在configs/qemu_arm64_defconfig中添加以下配置：

```Plain
 CONFIG_SEMIHOSTING=y
```

编译u-boot，成功后当前目录下会生成u-boot, u-boot.bin等文件。

```Bash
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu- 
make qemu_arm64_defconfig
make
```

## 4. Kernel
    

进入linux kernel官网, 选择一个希望下载的分支，点击browse，然后点击summary, 拉到底，复制git地址xxx,下载代码

```Bash
https://www.kernel.org/
...
git clone xxx
```

编译内核，生成的Image位置在`arch/arm64/boot`

```Bash
export ARCH=arm64
export CROSS_COMPILE=xxx/bin/aarch64-linux-gnu-
make defconfig
make
```

## 5. Rootfs
    

进入官网下载最新的稳定版本，然后解压

```Bash
https://buildroot.org/download.html
# 解压
xz -d xz -d buildroot-2023.02.8.tar.xz
tar xvf buildroot-2023.02.8.tar
```

配置

```Bash
make ARCH=arm64 menuconfig
```

- Target Options Target Architecture：AArch64（little endian） Target Architecture Variant：cortex-A53 其他默认不变。
    
- Toolchain 这里让buildroot自己去下载适配的工具链
    
- System configuration 这里默认不变
    
- Filesystem images ext2/3/4 root filesystem：y ex2/3/4 variant：ext4
    

编译

```Bash
sudo make #注意不能-j进行多核编译
```

## 6. ATF
    

下载atf后编译

```Bash
git clone https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
export CFLAGS='-O0 -gdwarf-2' # 默认ATF编译开优化，需要打开调试 #但这里是应该用export的方式指定吗？
#- 指定BL33的位置，会生成FIP包
make PLAT=qemu BL33=../u-boot/u-boot.bin all fip DEBUG=1 LOG_LEVEL=50 
```

## 7. qemu启动ATF
    

拷贝`bl1.bin, bl2.bin, bl31.bin`到test目录下，拷贝`u-boot.bin`到test目录下并重命名为bl33.bin。然后qemu启动atf

```Bash
mkdir test
cd test
cp ../trusted-firmware-a/build/qemu/debug/*.bin ./
cp ../u-boot/u-boot.bin ./bl33.bin
# qemu指令参数较多，且bash不支持用\来分行，可写入sh文件中，通过./start.sh执行
qemu-system-aarch64 \
    -nographic -machine virt,secure=on \
    -cpu cortex-a53 \
    -smp 2 -m 2048 \
    -d unimp \
    -bios bl1.bin \
    -semihosting-config enable=on,target=native \
    -s -S
```

-S：表示QEMU虚拟机会冻结CPU,直到远程的GDB输入相应控制命令

-s：表示在1234端口接受GDB的调试连接，其与-gdb tcp::1234参数相同

## 8. vscode配置gdb
    

调试arm64必须使用gdb-multiarch，

```Bash
sudo apt install gdb-multiarch
```

.vscode/launch.json配置如下

```JSON
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "atf debug",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/trusted-firmware-a/build/qemu/debug/bl1/bl1.elf",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "targetArchitecture": "arm64",
            "miDebuggerPath": "/usr/bin/gdb-multiarch",
            "miDebuggerServerAddress": "localhost:1234",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "Set Disassembly Flavor to Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ]
        }

    ]
}
```

此时在qemu启动了ATF后，即可在vscode中开始调试，在.S和.c文件中打断点，步进调试代码。