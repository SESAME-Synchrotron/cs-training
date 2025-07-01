# Installation Overview

An EPICS installation typically consists of multiple software modules.

EPICS Base will always be one of them. Base and additional modules that provide libraries or tools are often referred to as Support Modules, while the modules that produce your control system are often called IOC Application Modules.

EPICS Base and the Support Modules are usually common and shared between the IOC Applications of an installation. You can consider a stable and tested set of Base and Support Modules a release of your development environment.

As Support Modules are shared, have a longer life cycle and are held more stable than the IOC Applications that use them, it is a good idea to keep the Support Modules and IOC Applications separate.

This section will mostly cover installing EPICS Base and Support Modules. IOC Applications are too specific to be covered by general documentation.

# General workflow
The traditional way to install EPICS is by compiling from sources. While the specific instructions differ between Operating Systems on your host, the general steps are always the same:
* Install prerequisites
* Download, configure and install EPICS Base
* Download, configure and install Support Modules
* Create your IOC Application

# Homework - Installing EPICS on Rocky Linux
1. Install the following packages: `make`, `gcc-c++` and `readline-devel`
2. Create folder `epics` in your home directory.
3. Verify installation
```
make --version
gcc --version
g++ --version
```
4. Download EPICS base 7.0.9
```linux
wget https://epics-controls.org/download/base/base-7.0.9.tar.gz
```
**BONUS**: Clone EPICS base version 7.0.9 from GitHub https://github.com/epics-base/epics-base 

5. Extract the archive under `~/epics`
6. Build the sources
```
cd ~/epics/base-7.0.9
make
```
7. Export the following variables in `~/.bashrc`
```
export EPICS_BASE=${HOME}/epics/base-7.0.9
export EPICS_HOST_ARCH=$(${EPICS_BASE}/startup/EpicsHostArch)
export PATH=${EPICS_BASE}/bin/${EPICS_HOST_ARCH}:${PATH}
```
8. Source `~/.bashrc` and execute the following commands
```
caget
caput
camonitor
softIoc
```

**Submit the output**:
- Subject: HW1 - Preparing Linux - [NAME]
- Body: Output from steps #3 and #8
