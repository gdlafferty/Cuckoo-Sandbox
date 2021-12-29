# Cuckoo-Sandbox
Notes on the process of installing and using Cuckoo Sandbox

# References
[Setting up Cuckoo Sandbox v2.0.7 on Ubuntu 18.04 - Part 1](https://www.youtube.com/watch?v=QlQS4gk_lFU)

[Setting Up Cuckoo Sandbox v2.0.7 on Ubuntu 18.04.4 - Part 2](https://www.youtube.com/watch?v=FsF56772ZvU)

[Cuckoo on VMware ESXi](https://alfaiacomlinux.blogspot.com/2017/05/cuckoo-com-vmware-esxi.html)

# Information
- Running ESXi v6.7 Enterprise Edition
- Ubuntu 18.04
- Network interfaces:
  - Bridged (Internet Access)
    - MAC Address: 00:0C:29:19:E6:1E
    - Part of Bridge Port Group in ESXi Networking
  - IPS1 (Monitoring)
    - MAC Address: 00:0C:29:19:E6:28
    - Part of IPS Port Group in ESXi Networking
