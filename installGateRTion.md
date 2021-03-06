~/rtion/# VM setup

Installed XUbuntu v18.04 LTS

uname: gate<br>
psswd: virtual

Install VirtualBox guest packages to enable things like copy-paste

> Start the Virtual Machine<br>
> From the menus at the top of the virtual machine container window<br>
> Select `Devices` -> `Install Guest Additions`<br>
> This will mount the Guest Additions CD<br>
> Open a terminal, navigate to the CD, then run<br>
> `sudo apt-get install virtualbox-guest-additions-iso`<br>
> `sudo ./VBoxLinuxAdditions.run <br>

Also remember to set things like clipboard to bidirectional in you VBox manager.

**_VBox setup is frozen after initialisation - all updates turned off!!!_**

Using instructions from<br>
<https://github.com/OpenGATE/Gate/wiki/Gate-RTion-project>

Configuration

- GEANT4-10.3
- ROOTv6.08.06
- Physics: GBBC_EMZ or QGSP_BIC_EMZ

# Basic preparation

making an install directory<br>

```console
cd ~/
mkdir rtion
```

## Required basic packages for installation

```console
sudo apt-get install gcc g++ cmake make
```

# GEANT4.10.03.p03

following instructions from:<br>
<https://scicomex.com/how-to-install-geant4-on-linux>

additional dependencies for geant4 - produce error messages:

```console
sudo apt-get install libexpat-dev qt5-default libx11-dev libxmu-dev
```

These are included due to the following error messages received when installing without:

> missing: EXPAT_LIBRARY EXPAT_INCLUDE_DIR

> Qt version "" from NOTFOUND, this code requires Qt 4.x<br>
> X11 Xmu library and/or headers

obtain geant4.10.03 with patch-03 from:<br>
<https://geant4.web.cern.ch/support/download_archive>

```console
cd ~/rtion/
mkdir geant4
cd geant4/
wget -c http://cern.ch/geant4-data/releases/geant4.10.03.p03.tar.gz
tar -zxf geant4.10.03.p03.tar.gz
mkdir geant4.10.03.p03-build
mkdir geant4.10.03.p03-install
mkdir geant4.10.03.p03-data
```

```console
cd geant4.10.03.p03-build/
```

generate install files with cmake and following options

```console
cmake \
-DCMAKE_INSTALL_PREFIX=~/rtion/geant4/geant4.10.03.p03-install/ \
-DGEANT4_BUILD_MULTITHREADED=OFF \
-DGEANT4_INSTALL_DATA=ON \
-DCMAKE_INSTALL_DATADIR=cdata/ \
-DGEANT4_USE_OPENGL_X11=ON \
-DGEANT4_USE_QT=ON \
-DGEANT4_USE_RAYTRACER_X11=ON \
~/rtion/geant4/geant4.10.03.p03/
```

if cmake successful

```console
make (-j<nCPU>)

make install
```

activate by sourcing the appropriate .sh file

```console
source ~/rtion/geant4/geant4.10.03.p03-install/bin/geant4.sh
```

also add into .bash_aliases for future useres

# ROOTv6.08.06

install required dependencies as listed at:<br>
<https://root.cern/install/dependencies>

```console
sudo apt-get install git dpkg-dev cmake g++ gcc \
binutils libx11-dev libxpm-dev libxext-dev libxft-dev
```

`libxft-dev` may not install properly, if so this is because it requires a specific version of libfreetype6.  In this case:
```console
sudo apt-get install libfreetype6=2.10.1-2
sudo apt-get install libxft-dev
```

install required version of ROOT from:<br>
<https://root.cern/install/all_releases/><br>
<https://root.cern/releases/release-60806/><br>

Building from source to ensure compatible gcc and other compilers.

```console
cd ~/rtion/
mkdir root/
cd root/

wget -c https://root.cern/download/root_v6.08.06.source.tar.gz
tar -xzf root_v6.08.06.source.tar.gz
mv root_v6.08.06.source.tar.gz root-6.08.06/
```

```console
mkdir root-6.08.06-build/ root-6.08.06-install/
cd root-6.08.06-build/
```

```console
cmake \
-DCMAKE_INSTALL_PREFIX=~/rtion/root/root-6.08.06-install/ \
~/rtion/root/root-6.08.06/
```

As long as cmake is successful,

```console
make [-- -j<nCPU>]

make install
```
The compilation instructions on the website which included make and install simultaneously failed repeatedly so separated to allow completion.

<`make install` requires `sudo` permissions to write to location.<br>
May be able to avoid this requirement by specifying an `install` directory.>

activate by sourcing the appropriate .sh file

```console
source ~/rtion/root/root-6.08.06/bin/thisroot.sh
```

also add into .bash_aliases for future useres

# GATE 8.1 RT-ion

Following instructions from these sources:<br>
<http://www.opengatecollaboration.org/GateRTion><br>
<https://github.com/OpenGATE/Gate/tree/GateRTion>

```console
cd ~/rtion/
mkdir gate
cd gate/
wget -c https://github.com/OpenGATE/Gate/archive/GateRTion.zip
unzip GateRTion.zip
```

then following the instructions from:<br>
<https://opengate.readthedocs.io/en/latest/compilation_instructions.html>

```console
mkdir gate-build
mkdir gate-install
cd gate-build/
```

generate install files with cmake and following options

```console
cmake \
-DCMAKE_INSTALL_PREFIX=~/rtion/gate/gate-install/ \
~/rtion/gate/Gate-GateRTion/
```

if cmake successful

```console
make (-j<nCPU>)

make install
```

finally add the location of the gate executable to the system path

```console
export PATH=$PATH:~/rtion/gate/gate-install/bin/
```

also add into .bash_aliases for future useres

## Issues

Currently no known issues but only very minimal testing has been performed.

# START HERE.txt

Paste the following into a text file on the desktop of the device

```
Welcome to the GATE RT-ion virtual machine

Created by Andrew Gosling on YYYY-MM-DD
For details contact andrew.gosling@nhs.net

This VM should meet the requirements of the GATE RT-ion project.
Setup is according to the instructions found here:
https://github.com/OpenGATE/Gate/wiki/Gate-RTion-project

This VM is supplied under GNU GPL-3.0
###   USE IS ENTIRELY AT YOUR OWN RISK   ###



uname:  gate
pword:  virtual



The installation was performed in line with the documentation found here:
https://github.com/UCLHp/how-to/blob/master/installGateRTion.md


 - The keyboard is currently set for UK standard but can be changed in settings
 - Ensure copy and paste are set to biderectional in your VM manager
 - Networking is set to NAT which should work on most systems
 - Processors and memory allocation should also be adjusted in your VM manager
 - All automatic updates have been disabled where possible
   - Take care when performing updates that they do not affect the underlying function of GATE
```
