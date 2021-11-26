1.
```buildoutcfg
route-views>sho ip route 213.87.195.146
Routing entry for 213.87.195.0/24
  Known via "bgp 6447", distance 20, metric 0
  Tag 49788, type external
  Last update from 91.218.184.60 1w1d ago
  Routing Descriptor Blocks:
  * 91.218.184.60, from 91.218.184.60, 1w1d ago
      Route metric is 0, traffic share count is 1
      AS Hops 4
      Route tag 49788
      MPLS label: none
route-views>show bgp 213.87.195.146
BGP routing table entry for 213.87.195.0/24, version 1374048330
Paths: (1 available, best #1, table default)
  Not advertised to any peer
  Refresh Epoch 1
  49788 12552 8359 42115
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin IGP, localpref 100, valid, external, best
      Community: 12552:12000 12552:12100 12552:12101 12552:22000
      Extended Community: 0x43:100:1
      path 7FE0FB5B4E50 RPKI State invalid
      rx pathid: 0, tx pathid: 0x0
```

2.
```buildoutcfg
root@vagrant:~# modprobe dummy
root@vagrant:~# ip link add dummy0 type dummy
root@vagrant:~# ip -c -br link
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0             UP             08:00:27:73:60:cf <BROADCAST,MULTICAST,UP,LOWER_UP>
dummy0           UNKNOWN        0e:da:93:b5:a5:6c <BROADCAST,NOARP,UP,LOWER_UP>
root@vagrant:~# ip route add 172.16.10.0/24 dev dummy0
root@vagrant:~# ip route add 172.16.20.0/24 dev dummy0 metric 100
root@vagrant:~# ip -br route
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
172.16.10.0/24 dev dummy0 scope link
172.16.20.0/24 dev dummy0 scope link metric 100
192.168.100.0/24 dev dummy0 proto kernel scope link src 192.168.100.199
```

3.

```buildoutcfg
root@vagrant:~# ss -tlpn
State      Recv-Q     Send-Q         Local Address:Port         Peer Address:Port     Process
LISTEN     0          128                  0.0.0.0:22                0.0.0.0:*         users:(("sshd",pid=746,fd=3))
LISTEN     0          4096                 0.0.0.0:111               0.0.0.0:*         users:(("rpcbind",pid=622,fd=4),("systemd",pid=1,fd=35))
LISTEN     0          4096           127.0.0.53%lo:53                0.0.0.0:*         users:(("systemd-resolve",pid=623,fd=13))
LISTEN     0          128                     [::]:22                   [::]:*         users:(("sshd",pid=746,fd=4))
LISTEN     0          4096                    [::]:111                  [::]:*         users:(("rpcbind",pid=622,fd=6),("systemd",pid=1,fd=37))
```
- TCP 22 ssh
- TCP 111 SUNRPC
- TCP 53 DNS

4.

```
root@vagrant:~# ss -ulpn
State      Recv-Q     Send-Q          Local Address:Port         Peer Address:Port    Process
UNCONN     0          0               127.0.0.53%lo:53                0.0.0.0:*        users:(("systemd-resolve",pid=623,fd=12))
UNCONN     0          0              10.0.2.15%eth0:68                0.0.0.0:*        users:(("systemd-network",pid=402,fd=19))
UNCONN     0          0                     0.0.0.0:111               0.0.0.0:*        users:(("rpcbind",pid=622,fd=5),("systemd",pid=1,fd=36))
UNCONN     0          0                        [::]:111                  [::]:*        users:(("rpcbind",pid=622,fd=7),("systemd",pid=1,fd=38))
```
- UDP 53 DNS
- UDP 68 Bootstrap Protocol Client (DHCP)
- UDP 111 SUNRPC 

5.

