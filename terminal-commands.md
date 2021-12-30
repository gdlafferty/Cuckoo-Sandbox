# Commands (in order)

## Post-install

### Upgrade packages
```
sudo apt-get update; sudo apt-get upgrade -y
```

### Install Cuckoo Dependencies
```
sudo apt-get install python python-pip mongodb python-sqlalchemy python-bson python-dpkt python-jinja2 python-magic python-pymongo python-gridfs python-bottle python-pefile python-chardet python-django libffi-dev libssl-dev -y
```

### Install & Compile libvirt w/ESXi drivers
```
sudo apt-get install gcc make pkg-config libxml2-dev libgnutls28-dev libdevmapper-dev libcurl4-gnutls-dev python-dev libpciaccess-dev libxen-dev libnl-3-dev uuid-dev xsltproc device-mapper device-mapper-devel libdevmapper-dev libdevmapper1.02.1 libpciaccess-dev libcurl4-gnutls-dev -y 

wget http://libvirt.org/sources/libvirt-1.3.1.tar.gz
tar –xvf libvirt
cd libvirt
./configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc --with-esx=yes
make
sudo make install
sudo pip install libvirt-python
```

When running ```sudo pip install libvirt-python```, get error "Command 'python setup.py egg_info' failed with error code 1 in /tmp/pip-build-4bhmlA/libvirt-python/"

Ran ```sudo pip install --upgrade setuptools```, received same error

Ran ```sudo apt-get install python3.8-dev libmysqlclient-dev```, received same error

Installed Python3.10 using ```sudo apt install software-properties-common -y```, ```sudo add-apt-repository ppa:deadsnakes/ppa```, ```sudo apt install python3.10```

Ran ```sudo pip install libvirt-python=3.10.0```, success

### Verify libvert functionality

```
virsh -v
virsh -c esx://root@<esx_ip>
```

Ran ```start [VM_Name]```, verified that the command worked by checking the ESXi UI

### Configure Networking

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo  
iface lo inet loopback

# The primary network interface
auto eth0  
iface eth0 inet static  
    address 192.168.0.110
    netmask 255.255.255.0
    gateway 192.168.0.1
    dns-nameservers 8.8.8.8

# The Monitor Network interface
auto eth1  
iface eth1 inet manual  
    up ip address add 0/0 dev $IFACE
    up ip link set $IFACE up
    up ip link set $IFACE promisc on
down ip link set $IFACE promisc off  
down ip link set $IFACE down
```

### Editing the rc.local file

sudo nano /etc/rc.local
```
ifconfig etc1 up
ifconfig etc1 promisc
exit 0
```

### Install additional dependencies
**tcpdump**
```
sudo apt-get install tcpdump libcap2-bin -y
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
```

**pydeep**
```
sudo apt-get install ssdeep python-pyrex libfuzzy-dev -y
wget https://github.com/kbandla/pydeep/archive/0.4.tar.gz
tar –zxvf 0.4.tar.gz
cd 0.4/
python setup.py build
sudo python setup.py install
```

**Yara**
```
sudo apt-get install libpcre3 libpcre3-dev autoconf libtool -y

git clone https://github.com/plusvic/yara.git
cd yara
./bootstrap.sh
./configure
make
sudo make install
```

*Build yara-python*

When running ```cd yara-python```, get error "bash: cd: yara-python: No such file or directory"

Ran ```python3 -m pip install python-dev-tools --user --upgrade```, no change

Instead got yara using ```wget https://github.com/VirusTotal/yara/archive/refs/tags/v4.1.3.tar.gz```, ```tar -zxvf v4.1.3.tar.gz```

Still not working, realized that it never installed yara-python during the above steps.

Installed using ```git clone --recursive https://github.com/VirusTotal/yara-python```, ```cd yara-python```, ```python setup.py build```, and ```sudo python setup.py install```

**Volatility**
```
git clone https://github.com/volatilityfoundation/volatility.git
cd volatility
sudo python setup.py installgit clone https://github.com/volatilityfoundation/volatility.git
cd volatility
sudo python setup.py install
```

# Install Cuckoo

```
sudo pip install -U pip setuptools
sudo pip install -U cuckoo
```


sudo pip2 install -U cuckoo==2.0.7

git clone https://github.com/cuckoosandbox/cuckoo.git
