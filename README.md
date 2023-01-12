# Notes to Myself

## Terminal PS1 Prompt Variable

System-wide definition in `/etc/bash.bashrc`. Can customize in `~/.bashrc`.  Prompt variables are:

  * `PS1`: Normal prompt
  * `PS2`: Prompt displayed when command goes past first line in terminal window
  * `PS3`: Prompt used by “select” inside shell script
  * `PS4`: Prompt displayed when you execute a shell script in debug mode  

There is also a `PROMPT_COMMAND` that can be useful.

Reference: [https://www.thegeekstuff.com/2008/09/bash-shell-take-control-of-ps1-ps2-ps3-ps4-and-prompt_command/](https://www.thegeekstuff.com/2008/09/bash-shell-take-control-of-ps1-ps2-ps3-ps4-and-prompt_command/)

### Colors

Colors are delimited by `\[\e[` and `m\]`. In between, there are multiple possible parameters, all separated by a semicolon:  
  * Font weight:  
    * `0` = normal,  
    * `1` for bold,  
    * `2` for faded,  
  * Foreground color: 2-digit number beginning with 3 or 9,  
    | Code | Color |
    | ---- | ----- |
    | 30   | Black |
    | 31   | Red   |
    | 32   | Green   |
    | 33   | Yellow   |
    | 34   | Blue   |
    | 35   | Magenta   |
    | 36   | Cyan   |
    | 37   | Light gray   |
    | 39   | Default foreground color|
    | 90   | Dark Gray |
    | 91   | Light red |
    | 92   | Light green |
    | 93   | Light yellow |
    | 94   | Light blue |
    | 95   | Light magenta |
    | 96   | Light cyan |
    | 97   | White |
  * Background color: 2- or 3-digit number beginning with 4 or 10,
    | Code | Color |
    | ---- | ----- |
    | 40   | Black |
    | 41   | Red   |
    | 42   | Green   |
    | 43   | Yellow   |
    | 44   | Blue   |
    | 45   | Magenta   |
    | 46   | Cyan   |
    | 47   | Light gray   |
    | 49   | Default background color|
    | 100   | Dark Gray |
    | 101   | Light red |
    | 102   | Light green |
    | 103   | Light yellow |
    | 104   | Light blue |
    | 105   | Light magenta |
    | 106   | Light cyan |
    | 107   | White |
  * Font type:  
    * `0` = reset everything,  
    * `3` = italics, 
    * `4` = underlined, 
    * `5` = blinking, 
    * `7` = invert background and foreground

Example: `\[\e[1;36;4m\]` creates bold, cyan, underlined text.

Everything after declaring a color/text type will be what you declared, until you change it. To reset to default, use `\[\e[00m\]`

## Prompts I Like to Use

I usually emulate the Git Bash functionality of displaying the current branch if in a Git repo, which requires the following function accessible to the `PS1` variable:

```{sh}
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
```

* Single line:
  <pre>[<font color="#C01C28"><b>tom</b></font>:<font color="#E9AD0C"><b>~/Documents/portfolio</b></font><font color="#D0CFCC"><b> (awk)</b></font>]$</pre>
  ```
  export PS1="[\[\e[1;31m\]\u\[\e[00m\]:\[\e[1;93m\]\w\[\e[1;37m\]\$(parse_git_branch)\[\e[00m\]]$ "
  ```


* Full date on top of multiline prompt:
  <pre>Thu 12 Jan 2023 01:09:40 PM EST
  [<font color="#C01C28"><b>tom</b></font>:<font color="#E9AD0C"><b>~/Documents/portfolio</b></font><font color="#D0CFCC"><b> (awk)</b></font>]$ </pre>
  ```
  export PS1="\n\D{%c}\n[\[\e[1;31m\]\u\[\e[00m\]:\[\e[1;93m\]\w\[\e[1;37m\]\$(parse_git_branch)\[\e[00m\]]$ "
  ```


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

