# Notes to Myself

## Updating Linux Kernel

Check the current kernel version with `uname -r`. Ubuntu Linux kernels are distributed from [https://kernel.ubuntu.com/~kernel-ppa/mainline/](https://kernel.ubuntu.com/~kernel-ppa/mainline/). Use the following steps to update:  

1. Make a directory in `~/Downloads` in which to download the new kernel package files:  

    ```
    mkdir ~/Downloads/kernel_update
    cd ~/Downloads/kernel_update
    ```

1. Download the package files for the desired kernel version. There should be ~4 `.deb` files listed under amd64. Use something like this:  

    ```
    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.15.86/amd64/linux-headers-5.15.86-051586-generic_5.15.86-051586.202212310833_amd64.deb

    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.15.86/amd64/linux-headers-5.15.86-051586_5.15.86-051586.202212310833_all.deb

    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.15.86/amd64/linux-image-unsigned-5.15.86-051586-generic_5.15.86-051586.202212310833_amd64.deb

    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.15.86/amd64/linux-modules-5.15.86-051586-generic_5.15.86-051586.202212310833_amd64.deb
    ```

1. Ensure the kernal package files are executable and install them:  
    ```
    sudo chmod 0766 ./*.deb
    sudo apt install ./*.deb
    ```

    Note: If there are any errors complaining about the `_apt` user not having permission, `chmod` the files to `0776`.

1. Reboot and check that the kernel updated to the desired version with `uname -r`

1. Clean up the directory made in the first step for downloading the package files:

    ```
    rm -rf ~/Downloads/kernel_update
    ```

