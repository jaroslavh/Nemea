# NEMEA USER GUIDE
This document serves as a guide for users who want to install and use Nemea system. Developers wanting to write their own modules should refer to README.md

NEMEA (NEtwork MEasurement Analysis) system is a **stream-wise**, **flow-based** and **modular** detection system for network traffic analysis. In most of it's use cases it can detect anomalies close to real-time which a must in today's network administration and analysis.
System is widely modular which allows the user to pick his own use case and not waste any computational capacity on processes that he does not want.

TODO (picture of basic NEMEA setup)

Nemea system recieves data in IPFIX or NetFlow format.

Detectors are able to detect malicious traffic like (D)DoS, port scanning, brute-force attacks etc. which is then sent to Reporting Modules which are exporting it to Warden. (TODO - mention Warden here)

Modules on the other hand are used for filtering, aggregating, saving, exporting flows. You can for example use them for creating graphs with different statistics, which are always good in any network monitoring.

To simplify starting and maintaining running detectors and modules you can use Supervisor. It is a simple terminal tool that you can use to start/stop modules. You can of course run each module separately, but we recommend using Supervisor at least for the first time.

For deeper understanding about how modules and detectors communicate with each other read README.md. (TODO - explain it here?)

## Installing NEMEA

There are three ways of installation NEMEA.
a) Easiest ways is to download [RPM packages](https://copr.fedorainfracloud.org/coprs/g/CESNET/NEMEA/). We are using COPR to distribute them. To install run the following commands and you are all set to use nemea.
```
# yum install copr
# yum install nemea
```
b) Using Vagrant can be used to try the system right away. You have to have [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://virtualbox.org) installed. Then you can use one command to create a virtual machine, that you can ssh into and try all Nemea features in a virtual enviromnent. Just move to a directory in `vagrant/` that is named after your desired OS and use:
```
$ vagrant up
$ vagrant ssh
```
It will ssh you into newly created machine. When you are done working with it just run:
```
$ vagrant destroy
```
which will destroy it.

c) Lastly you can install NEMEA from source.
### Dependencies

#### Building environment
The whole system is based on GNU/Autotools build system that makes dependency checking and
building process much more easier.

* autoconf
* automake
* gcc
* gcc-c++
* libtool
* libxml2-devel
* libxml2-utils (contains xmllint on Debian)
* make
* pkg-config
* libpcap (optional, used in [flow_meter](https://github.com/CESNET/Nemea-Modules/tree/master/flow_meter)
* [libnf](https://github.com/VUTBR/libnf) (optional, used in [nfreader](https://github.com/CESNET/Nemea-Modules/tree/master/nfreader))
* libidn (optional, used in [blacklistfilter](https://github.com/CESNET/Nemea-Detectors/tree/master/blacklistfilter)) TODO still?
* bison and flex (optional, used[unirecfilter](https://github.com/CESNET/Nemea-Modules/tree/master/unirecfilter))

#### Installation
Install dependencies:

```
# yum install -y autoconf automake gcc gcc-c++ libtool libxml2-devel make pkg-config libpcap libidn bison flex
```

Clone the NEMEA repositories:

```
$ git clone --recursive https://github.com/CESNET/nemea
$ cd nemea
$ ./bootstrap.sh
```

That will create `configure` scripts and other needed files.

The `configure` script supplies various possibilities of
configuration and it uses some environmental variables that influence the build
and compilation process. For more information see:

```
$ ./configure --help
```

We recommend to set paths according to the used operating system, e.g.:

```
$ ./configure --prefix=/usr --bindir=/usr/bin/nemea --sysconfdir=/etc/nemea --libdir=/usr/lib64
```

After finishing `./configure`, build and install by:

```
$ make
# make install
```
Once installed NEMEA can start monitoring right away.

## Usage of NEMEA
There are many good and efficient ways to use NEMEA. In this document we will show two examples how NEMEA can be used.
- simple usage of NEMEA-Probe, where you install our flow monitoring probe on an openWRT router to capture and monitor small networks traffic.
- advanced usage for companies that already have their monitoring devices, installing NEMEA Collector machine that can process your NetFlow/IPFIX traffic.

(TODO - both examples)
