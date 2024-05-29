# Lab 7: Introduction to Operating Systems (OS/161 as a Case Study)

In this lab, you will get familiar with Operating Systems by exploring OS/161, an operating system made for teaching purposes. You will learn how to configure and build operating systems.

**Prerequisite**: You need to complete [LAB 5](https://github.com/cs-uob/COMS20012/blob/master/docs/labs/lab%205.md) before starting this lab.

**Remote Access**: Follow [these instructions](https://uob.sharepoint.com/sites/itservices/SitePages/fits-engineering-linux-x2go.aspx) for using the Linux Lab Machine remotely if needed.

## Installation

1. **On Windows**: Download and install [Git for Windows](https://git-scm.com/download/). This includes a `bash` shell which you should use.
2. **Install VirtualBox**: Ensure you have the latest version of [VirtualBox](https://www.virtualbox.org/).
3. **Install Vagrant**: Follow [these instructions](https://docs.vagrantup.com/v2/installation/).
4. **Install Vagrant Plugins**:
    ```sh
    vagrant plugin install vagrant-vbguest
    vagrant plugin install vagrant-timezone
    ```

## Setting up Vagrant Image

1. Clone the Vagrant setup repository and start the VM:
    ```sh
    git clone https://github.com/uob-jh/vagrant-os161.git
    cd vagrant-os161
    vagrant up
    vagrant reload
    ```

## Getting the Source

1. SSH into your guest virtual machine:
    ```sh
    vagrant ssh
    ```
2. Clone the OS/161 repository:
    ```sh
    cd ~
    git clone https://github.com/uob-jh/os161.git
    ```

### Exercise

1. Delete the repository and create your own fork on GitHub. You will need it to track your changes and share your code with classmates and TAs.

## Configure OS/161 Source Tree

1. Configure the OS/161 source tree:
    ```sh
    cd os161
    ./configure
    ```

## Building Userland

1. Build userland dependencies:
    ```sh
    cd os161
    bmake
    bmake install
    ```

## Configuration OS/161 Kernel

1. Configure the kernel using the `LAB5` configuration:
    ```sh
    cd kern/conf
    ./config LAB5
    ```

## Building OS/161 Kernel

1. Build the kernel:
    ```sh
    cd ../compile/LAB5
    bmake depend
    bmake
    bmake install
    ```

## Running OS/161 Kernel

1. Download the configuration file and place it in `~/os161/root/`:
    ```sh
    cd ~/os161/root/
    wget https://cs-uob.github.io/COMS20012/labs/sys161.conf
    ```
2. Run the kernel:
    ```sh
    sys161 kernel
    ```

### Exercises

1. **Boot and Shutdown Output Questions**:
    - **Which version of System/161 and OS/161 are you using?**
        - System/161 version 2.0.3, OS/161 base system version.
    - **Where was OS/161 developed and copyrighted?**
        - Harvard College.
    - **How much memory and how many CPU cores was System/161 configured to use?**
        - 16M of memory, 8 CPU cores.
    - **What configuration was used by your running kernel?**
        - Configuration specified in `sys161.conf`.
    - **How many times has your kernel been compiled?**
        - Check the compilation count in the build logs or file.

2. **Boot with Different Configurations**:
    - **Boot your OS/161 kernel with a different number of cores (e.g., 4)**:
        ```sh
        Modify sys161.conf and set cpus=4
        ```
    - **Try booting with 256K of memory. What happens?**
        - The system may fail to boot due to insufficient memory.

## Exploring OS/161

### Kernel Sources Overview

1. **Top-Level Source Directory**:
    - **Files**: `CHANGES`, `configure`, `Makefile`.
    - **Directories**: `common/`, `design/`, `kern/`, `man/`, `mk/`, `userland/`.

2. **Userland Directory**:
    - `bin/`, `include/`, `lib/`, `sbin/`, `testbin/`.

3. **Kernel Directory**:
    - `arch/`, `compile/`, `test/`, `dev/`, `include/`, `lib/`, `main/`, `thread/`, `syscall/`, `vm/`, `vfs/`, `fs/`.

### Exercises

1. **Kernel Initialization**:
    - **Function that initializes the kernel**: `main()`.
    - **Subsystems initialized**: Various kernel subsystems, e.g., memory management, process management.

2. **Debugging Messages**:
    - **System for printing debugging messages**: `kprintf()`.
    - **Utility**: Useful for tracking the execution flow and debugging.

3. **Copyin and Copyout Functions**:
    - **copyin**: Copies data from user space to kernel space.
    - **copyout**: Copies data from kernel space to user space.
    - **Special**: These functions handle memory in user space with necessary checks, unlike `memmove` which does not.

## Using GDB

1. **Boot Kernel and Wait for GDB Connection**:
    ```sh
    cd ~/os161/root/
    sys161 -w kernel
    ```

2. **Open Second Terminal and Start GDB**:
    ```sh
    vagrant ssh
    cd ~/os161/root/
    os161-gdb kernel
    ```

3. **Connect GDB**:
    ```sh
    target remote unix:.sockets/gdb
    ```

### Trying GDB

1. **Set Breakpoint at `kmain`**:
    ```sh
    break kmain
    ```

2. **Continue and Step Through Boot Function**:
    ```sh
    c
    s
    n
    ```

3. **Add Breakpoint to `sys_reboot` and Continue**:
    ```sh
    break sys_reboot
    c
    ```

4. **Shutdown the Machine**:
    ```sh
    p /sbin/poweroff
    ```

By following these steps, you should be able to configure, build, and run the OS/161 operating system, explore its source code, and debug it using GDB.
