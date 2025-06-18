hq-rtr(config)#ntp timezone utc+7 
hq-rtr(config)#do show ip interface brief
hq-rtr(config)#interface isp
hq-rtr(config-if)#ip address 172.16.4.14/28 
hq-rtr(config-if)#port te0
hq-rtr(config-port)#service-instance te0/isp
hq-rtr(config-service-instance)#encapsulation untagged 
hq-rtr(config-service-instance)#connect ip interface isp

2025-06-18 03:29:09      INFO      Interface isp changed state to up

[root@isp ~]# ip -c -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128 
ens18            UP             10.0.2.15/24 fe80::be24:11ff:fe2e:acb6/64 
ens19            DOWN           
ens20            DOWN           
BOOTPROTO=static
TYPE=eth
BOOTPROTO=static

[root@isp ~]# vim /etc/net/ifaces/ens20/options 
[root@isp ~]# echo 172.16.4.1/28 >> /etc/net/ifaces/ens19/ipv4address
[root@isp ~]# echo 172.16.4.1/28 >> /etc/n
[root@isp ~]# echo 172.16.5.1/28 >> /etc/net/ifaces/ens20/ipv4address
[root@isp ~]# 
[root@isp ~]# ls -la /etc/net/ifaces/
total 32
drwxr-xr-x 8 root root 4096 Jun 17 18:50 .
drwxr-xr-x 5 root root 4096 Oct 31  2024 ..
drwxr-xr-x 3 root root 4096 Oct 31  2024 default
drwxr-xr-x 2 root root 4096 Sep 12  2024 ens18
drwxr-xr-x 2 root root 4096 Jun 17 18:51 ens19
drwxr-xr-x 2 root root 4096 Jun 17 18:52 ens20
drwxr-xr-x 2 root root 4096 Oct 31  2024 lo
drwxr-xr-x 2 root root 4096 Oct 31  2024 unknown
[root@isp ~]# ip -c -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128 
ens18            UP             10.0.2.15/24 fe80::be24:11ff:fe2e:acb6/64 
ens19            DOWN           
ens20            DOWN           
[root@isp ~]# ip -c -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128 
ens18            UP             10.0.2.15/24 fe80::be24:11ff:fe2e:acb6/64 
ens19            DOWN           
ens20            DOWN           
[root@isp ~]# systemctl restart network
[root@isp ~]# ip -c -br a
lo               UNKNOWN        127.0.0.1/8 ::1/128 
ens18            UP             10.0.2.15/24 fe80::be24:11ff:fe2e:acb6/64 
ens19            UP             172.16.4.1/28 fe80::be24:11ff:fe83:937e/64 
ens20            UP             172.16.5.1/28 fe80::be24:11ff:fe63:8ab8/64 
[root@isp ~]# apt-get install iptables
Reading Package Lists... Done
Building Dependency Tree... Done
The following extra packages will be installed:
  libnetfilter_conntrack  libnfnetlink  libpcap0.8
The following NEW packages will be installed:
  iptables  libnetfilter_conntrack  libnfnetlink  libpcap0.8
0 upgraded, 4 newly installed, 0 removed and 115 not upgraded.
Need to get 490kB of archives.
After unpacking 2446kB of additional disk space will be used.
Do you want to continue? [Y/n] y

~
~
~
~
~
# This file was formerly part of /etc/sysctl.conf
### IPV4 networking options.

# IPv4 packet forwarding.
#
# This variable is special, its change resets all configuration
# parameters to their default state (RFC 1122 for hosts, RFC 1812 for
# routers).
#
net.ipv4.ip_forward = 1
