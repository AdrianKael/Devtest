Kernel Parameters
    Protection from SYN flood attack
        Commands:
            //Detect SYN Flood Attack
                netstat -tuna | grep :80 | grep SYN_RECV
            //Reduce SYN timeout
                iptables -A FORWARD -p TCP –syn -m limit –limit 1/s -j ACCEPT
                iptables -A INPUT -i eth0 -m limit –limit 1/sec –limit-burst 5 -j ACCEPT
            //Up to 3 per packet SYN
                iptables -N syn-flood
                iptables -A INPUT -p tcp –syn -j syn-flood
                iptables -A syn-flood -p tcp –syn -m limit –limit 1/s –limit-burst 3 -j RETURN
                iptables -A syn-flood -j REJECT
            //Set SYN cookies *Most Important
                sysctl -w net.ipv4.tcp_syncookies=1
                sysctl -w net.ipv4.tcp_max_syn_backlog=3072
                sysctl -w net.ipv4.tcp_synack_retries=0
                sysctl -w net.ipv4.conf.all.send_redirects=0
                sysctl -w net.ipv4.conf.all.accept_redirects=0
                sysctl -w net.ipv4.conf.all.forwarding=0
                sysctl -w net.ipv4.icmp_echo_ignore_broadcasts=1
            // If needed Block specific IP range
                iptables -A INPUT -s 192.168.5.1/8 -i eth0 -j Drop

    Ignore all ICMP ECHO and TIMESTAMP requests sent to it via broadcast/multicast
        Commands:
            //Preventing the ping command
                sysctl -w net.ipv4.icmp_echo_ignore_all=1                       //Works for all broadcast
            // Ignore TIMESTAMP REQUESTS
                sysctl -w net.ipv4.tcp_timestamps=0                             //Specific for Timestamp responses
                sysctl -w net.ipv4.icmp_echo_ignore_broadcasts = 1              //specific for broadcast
    Discourage Linux from swapping idle server processes to disk (default = 60)
    Increase number of incoming connections that can queue up before dropping
        Commands:
                sysctl -w net.ipv4.tcp_max_syn_backlog="2048"                   //BackLog Queue Will be increased from default '256Kb' to '2048Kbs'
    Disable packet forwarding.
        Commands:
                sysctl -w net.ipv4.ip_forward=0
    Disconnect dead TCP connections after 1 minute
        Command:
                sysctl -w net.ipv4.tcp_fin_timeout = 60                         //the length of time an orphaned connection will wait before it is aborted (60 sec)