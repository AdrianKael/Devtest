Please create a SWAP file and enable it, following the rules according the size of the virtual RAM memory of the VM
Steps:
    Htop //check actual swap memory
    fallocate -l 1G /swapfile       //reserve 1G in /swapfile
    du -sh /swapfile                //check the size of the /swapfile is correct
    ls -l /swapfile                 //check the permissions of the file /swapfile
    chmod 600 /swapfile             //change the permissions of the file to have full Read and Write permission for the owner of the file
    ls -l /swapfile                 //check permissions again on the file should be: -rw------- root root 'UUID' 'date' /swapfile
    mkswap /swapfile                //make swap area with the specific file /swapfile
    swapon /swapfile                //enable the specific swapfile
    #Make the swap file to be enable each time the VM boot - create an automatic mount point
    nano /etc/fstab                 // add the /swapfile here
    // /swapfile        swap    swap    defaults    0   0
    sudo reboot

    htop                            // now should be possible to see the Swap partition on Swp[         0K/1024M]
    
        