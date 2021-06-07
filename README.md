# os-project

Description of my system is :
CPU :Intel® Core™ i5 CPU M 450 @ 2.40GHz × 2 
cores :2
RAM :2 GiB
capacity :80 GB
Kernel version:linux-5.8.1


there are four steps to add a simple system call to the Linux kernel.

1-Preparation:
In this section, you will download all necessary tools to add a basic system call to the Linux kernel and run it.

    -Fully update your operating system bt "sudo apt update && sudo apt upgrade -y"
    -Download and install the essential packages to compile kernels. by "sudo apt install build-essential libncurses-dev libssl-dev libelf-dev bison flex -y"
    -Clean up your installed packages. by "Clean up your installed packages."
    -Download the source code of the latest stable version of the Linux kernel (which is 5.8.1 as of 12 August 2020) to your home folder.
      by "wget -P ~/ https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.8.1.tar.xz"
      
 ![download kernal](https://user-images.githubusercontent.com/83427972/120956663-53150980-c754-11eb-8bc9-46df18c93c07.png)
      
      
    -Unpack the tarball you just downloaded to your home folde by "tar -xvf ~/linux-5.8.1.tar.xz -C ~/"
    
 ![extract kernal](https://user-images.githubusercontent.com/83427972/120956567-19440300-c754-11eb-8110-a10f12c0c903.png)
    
    - Reboot your computer.
    
2-Creation:
In this section, you will write a basic system call in C and integrate it into the new kernel.

    - Change your working directory to the root directory of the recently unpacked source code.
      by "cd ~/linux-5.8.1/"
    - Create the home directory of your system call by "mkdir identity"
    - Create a C file for your system call by "nano identity/identity.c"

 ![the c program](https://user-images.githubusercontent.com/83427972/120956745-835ca800-c754-11eb-9c8f-005c09686108.png)

    - Create a Makefile for your system call by "nano identity/Makefile"
     Write the following code in it "obj-y := identity.o"
    
    - Add the home directory of your system call to the main Makefile of the kernel 
    by "nano Makefile" and then Search for core-y. In the second result, you will see a series of directories.
    kernel/ certs/ mm/ fs/ ipc/ security/ crypto/ block/  , Add the home directory of your system call at the end like the following.
    kernel/ certs/ mm/ fs/ ipc/ security/ crypto/ block/ identity/

![edit in the make file](https://user-images.githubusercontent.com/83427972/120957190-85733680-c755-11eb-9338-ce044cbc3784.png)


    - Add a corresponding function prototype for your system call to the header file of system calls
    by "nano include/linux/syscalls.h" and then 
    Navigate to the bottom of it and write the following code just above #endif.
    asmlinkage long sys_identity(void);
    
 ![prototype](https://user-images.githubusercontent.com/83427972/120957250-9fad1480-c755-11eb-8206-caf6675d54ce.png)

    - Add your system call to the kernel's system call table by "nano arch/x86/entry/syscalls/syscall_64.tbl" and then
    Navigate to the bottom of it. You will find a series of x32 system calls. Scroll to the section above it. 
    Add the following code at the end of this section respecting the chronology of the row as well as the format of the column. Use Tab for space.
    440     common  identity                sys_identity
    
![table](https://user-images.githubusercontent.com/83427972/120957303-b6ec0200-c755-11eb-8880-731f06e78c8a.png)


3 - Installation
In this section, you will install the new kernel and prepare your operating system to boot into it.

    - Configure the kernel.
    - Make sure the window of your terminal is maximized. Open the configuration window with the following command by "make menuconfig" .

![confi](https://user-images.githubusercontent.com/83427972/120957373-d3883a00-c755-11eb-9a41-d4d319281ec1.png)

    - Compile the kernel's source code by "make -j2"

    - Prepare the installer of the kernel by "sudo make modules_install -j12"
 
    - Install the kernel "sudo make install -j12"

    - Update the bootloader of the operating system with the new kernel.sudo update-grub

    - Reboot your computer.

4 - Result
In this section, you will write a C program to check whether your system call works or not. After that, you will see your system call in action.




references:

 https://dev.to/omergulen/how-to-add-system-call-syscall-to-the-kernel-compile-and-test-it-3e6p?fbclid=IwAR3qsB_FDOJFt_1rM7AaYvINFc1y2Nyi_cKJSgXAHUIkfCET0G_ByWpikZU
 
 https://tssurya.wordpress.com/2014/08/19/adding-a-hello-world-system-call-to-linux-kernel-3-16-0/?fbclid=IwAR1yPJ0xTrVeykKH8uBNOMU_EwV8AJy9NPcGlmOYzi8PywvgqVibcaXBMPI
 
 https://www.youtube.com/results?search_query=how+to+add+a+system+call+to+your+kernal
 
 


  
