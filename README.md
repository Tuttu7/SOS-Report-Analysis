 #### First we have to navigate to the folder ( for example sosreport-ip-172-31-23-203-2022-07-09-thmuikm ) created during the SOS report generation process.


 #### Top 10 memory consuming processes:
 
```
root@ip-172-31-23-203 sosreport-ip-172-31-23-203-2022-07-09-thmuikm]# awk '{m[$11]+=$6}END{for(item in m){printf "%s %s MiB\n",item,m[item]/1024}}' ps | sort -nrk2 | head | column -t
/usr/libexec/platform-python       258.656  MiB
/usr/lib/systemd/systemd           20.5586  MiB
/usr/lib/polkit-1/polkitd          13.0898  MiB
/usr/sbin/NetworkManager           10.582   MiB
sshd:                              10.2617  MiB
/usr/libexec/sssd/sssd_nss         9.44922  MiB
/usr/lib/systemd/systemd-udevd     9.23047  MiB
/usr/lib/systemd/systemd-logind    9.01172  MiB
/usr/libexec/sssd/sssd_be          8.39844  MiB
/usr/lib/systemd/systemd-journald  7.81641  MiB
```

 #### Total RSS ( RSS is the Resident Set Size and is used to show how much memory is allocated to that process and is in RAM )

```
[root@ip-172-31-23-203 sosreport-ip-172-31-23-203-2022-07-09-thmuikm]# cat ps | gawk '{sum+=$6} END {print "Total RSS : " sum/1024^2 " GiB"}'
Total RSS : 0.425945 GiB

[root@ip-172-31-23-203 sosreport-ip-172-31-23-203-2022-07-09-thmuikm]# cat ps |  awk '{m[$11] += $6} END { for (item in m) { printf "%20s %10s MiB \n", item, m[item]/1024 } }' | sort -k 2 -r -n|head -20
/usr/libexec/platform-python    258.656 MiB 
/usr/lib/systemd/systemd    20.5586 MiB 
/usr/lib/polkit-1/polkitd    13.0898 MiB 
/usr/sbin/NetworkManager     10.582 MiB 
               sshd:    10.2617 MiB 
/usr/libexec/sssd/sssd_nss    9.44922 MiB 
/usr/lib/systemd/systemd-udevd    9.23047 MiB 
/usr/lib/systemd/systemd-logind    9.01172 MiB 
/usr/libexec/sssd/sssd_be    8.39844 MiB 
/usr/lib/systemd/systemd-journald    7.81641 MiB 
      /usr/sbin/sssd    7.59766 MiB 
/usr/lib/systemd/systemd-hostnamed    7.45312 MiB 
               -bash    7.42188 MiB 
  /usr/sbin/rsyslogd    6.10156 MiB 
                sudo     5.4375 MiB 
      /usr/sbin/sshd    5.17578 MiB 
            (sd-pam)          5 MiB 
                  su    4.60156 MiB 
/usr/bin/dbus-daemon     4.1875 MiB 
          unsquashfs    3.84766 MiB 

          
          
          
          
[root@ip-172-31-23-203 sosreport-ip-172-31-23-203-2022-07-09-thmuikm]# cat ps | awk '!/%CPU/ {sum[$11]+=$3} END {for (i in sum) print i" - "sum[i] " %" }' | sort -rnk3 | column -t|head
-                                     -  92   %
/usr/libexec/platform-python          -  75   %
/usr/lib/systemd/systemd              -  1.7  %
/usr/lib/systemd/systemd-hostnamed    -  0.6  %
unsquashfs                            -  0.5  %
/usr/libexec/sssd/sssd_nss            -  0.1  %
[kswapd0]                             -  0.1  %
[xfs-reclaim/xvd]                     -  0    %
[xfs_mru_cache]                       -  0    %
[xfs-log/xvda2]                       -  0    %


# cat proc/meminfo | grep "MemTotal\|MemFree\|MemAvailable\|Buffers\|Cached\|Slab\|SReclaimable\|SUnreclaim"
MemTotal:         821044 kB
MemFree:           62604 kB
MemAvailable:     305616 kB
Buffers:               4 kB
Cached:           333448 kB
SwapCached:            0 kB
Slab:             106156 kB
SReclaimable:      55236 kB
SUnreclaim:        50920 kB
```
#### To get Server information

```
root@ip-172-31-23-203 sosreport-ip-172-31-23-203-2022-07-09-thmuikm]# cat hostname ; cat uptime ; cat date ; cat etc/redhat-release ; cat uname ; cat dmidecode |grep 'System Information' -A3
ip-172-31-23-203.ec2.internal
 14:57:18 up 2 min,  1 user,  load average: 3.96, 1.14, 0.40
               Local time: Sat 2022-07-09 14:57:41 UTC
           Universal time: Sat 2022-07-09 14:57:41 UTC
                 RTC time: Sat 2022-07-09 14:57:41
                Time zone: UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
Red Hat Enterprise Linux release 8.6 (Ootpa)
Linux ip-172-31-23-203.ec2.internal 4.18.0-372.9.1.el8.x86_64 #1 SMP Fri Apr 15 22:12:19 EDT 2022 x86_64 x86_64 x86_64 GNU/Linux
System Information
	Manufacturer: Xen
	Product Name: HVM domU
	Version: 4.11.amazon
 ```
 
 #### Out of memory
 
 ```
 grep -i "out of memory\|oom-killer\|soft lockup\|allocation\|blocked for more than 120 seconds\|segfault\|abrt-hook-ccpp" var/log/messages*
 ```
 
  #### Finding 'D' state process ( Uninteruptable sleep state )
  
  ```
  less ps | awk '$8 ~ /^D/{print}'
  ```
  

