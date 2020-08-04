# free5GC-UPF

## standalone build without free5gc
```bash
wget https://dl.google.com/go/go1.12.9.linux-amd64.tar.gz
sudo tar -C /usr/local -zxvf go1.12.9.linux-amd64.tar.gz
mkdir free5gc
cd free5gc
mkdir go
vi env.sh

  export GOPATH=/home/ubuntu/free5gc/go
  export GOROOT=/usr/local/go
  export PATH=$PATH:$GOPATH/bin:$GOROOT/bin
  export GO111MODULE=off

. ./env.sh

sudo apt-get -y update
sudo apt-get -y install git gcc g++ cmake libmnl-dev autoconf libtool libyaml-dev pkg-config
go get github.com/sirupsen/logrus

mkdir -p ${GOPATH}/src/free5gc/lib

cd ${GOPATH}/src/free5gc/lib
git clone https://github.com/free5gc/path_util.git && \
git clone https://github.com/free5gc/logger_util.git && \
git clone https://github.com/free5gc/logger_conf.git

cd /home/ubuntu/free5gs
git clone https://github.com/bjoern-r/upf.git
mkdir build
cd build
cmake ..
make -j`nproc`
```

## Get Started
### Prerequisites
Libraries used in UPF
```bash
sudo apt-get -y update
sudo apt-get -y install git gcc cmake go libmnl-dev autoconf libtool libyaml-dev
go get github.com/sirupsen/logrus
```

Linux kernel module 5G GTP-U (Linux kernel version = 5.0.0-23-generic)
```bash
git clone https://github.com/PrinzOwO/gtp5g.git
cd gtp5g
make
sudo make install
```

### Build
```bash
mkdir build
cd build
cmake ..
make -j`nproc`
```

### Test
```bash
cd build/bin
./testutlt
```

### Edit configuration file
After building from sources, edit `./build/config/upfcfg.yaml`

### Setup environment
```bash
sh -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
iptables -t nat -A POSTROUTING -o {DN_Interface_Name} -j MASQUERADE
```

### Run
```bash
cd build
sudo -E ./bin/free5gc-upfd
```
To show usage: `./bin/free5gc-upfd -h`


## Clean the Environment (if needed)
### Remove POSIX message queues
```bash
ls /dev/mqueue/
rm /dev/mqueue/*
```

### Remove gtp devices (using tools in libgtp5gnl)
```bash
cd lib/libgtp5gnl/tools
sudo ./gtp5g-link del {Dev-Name}
```
