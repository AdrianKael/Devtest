Kernel
    List the modules installed in the system
        Commands:
            less /proc/modules                  //check all modules also possible with
            lsmod                               //check with columns more detailed
    Install Kernel headers and show their installation path
        Commands:
            sudo apt update                     //First update the packages index
            ls -l /usr/src/linux-headers-$(uname -r)    //check the # of modules installed
            sudo apt install linux-headers-$(uname -r)  
            ls -l /usr/src/linux-headers-$(uname -r)    //compare the actual # of modules installed