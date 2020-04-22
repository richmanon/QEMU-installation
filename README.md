## QEMU-installation
this is for the installation of QEMU with host OS debian with CPU of mispel 32 bit little endian inside  QEMU</br>
QEMU on Linux hosts

This documentation is work in progress - more information needs to be added for different Linux distributions. 
Linux is QEMU's main host platform. Therefore it is the platform which gets most support. Both 32 and 64 bit Linux hosts are supported. Most of the following instructions are valid for both variants. 

## Building QEMU for Linux

Most Linux distributions already provide binary packages for [QEMU](https://wiki.qemu.org/Documentation/Networking) (or [KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine)). 
Usually they also include all packages which are needed to compile QEMU for Linux. The default installation of most distributions will not include everything, so you have to install some additional packages before you can build QEMU


Required additional packages</br>
                      <pre>• git (30 MiB), (version manager)</br>
                            • glib2.0-dev (9 MiB), (this automatically includes zlib1g-dev)</br> 
                            • libfdt-devel</pre>
For Ubuntu LTS Trusty (and maybe other Debian based distributions), all required additional packages can be installed like this: 

<pre><b>sudo apt-get install git libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev</br>
sudo apt-get install libnfs-dev libiscsi-dev</b></pre>

Getting the source code
If you want the latest code, follow the development of the code, work with several versions or maybe even contribute to the code, you will need a local copy of the QEMU code repository which is managed using git. 
Get the code like this: 
</br>
to copy the qemu repository [click](https://github.com/qemu/qemu)</br>

The resulting directory qemu is your QEMU root directory. 
Note that when building <b>QEMU </b> from GIT, 'make' will attempt to checkout various GIT submodules. 
Simple build and test
QEMU supports builds in this directory (not recommended) or in an extra directory (out-of-tree builds, recommended). There can be any number of out-of-tree builds, so if you plan to make cross builds, debug and release builds, out-of-tree builds are what you need. 
Here is my typical build scenario: 
## Switch to the QEMU root directory.
<pre>cd qemu</pre>
## Prepare a native debug build.
<pre>mkdir -p bin/debug/native</br>
cd bin/debug/native</pre>
## Configure QEMU and start the build.
<pre>../../../configure --enable-debug</br>
make</br>
## Return to the QEMU root directory.
cd ../../..</pre>
Now let's start a simple test: </br>
[Download](https://people.debian.org/~aurel32/qemu/mipsel/)
 
Before initiating the download go through the above mentioned link and inside this link go through README.txtfile and download the required kernel and qcow2 image because they are required for the QEMU stimulator installation.
After The installation get completed 
Go  inside qemu directory 
and run following command
<pre>
bin/debug/native/mipsel-softmmu/qemu-system-mipsel -L pc-bios -kernel ~/Downloads/vmlinux-3.2.0-4-4kc-malta
 </pre>

(note: here i downloaded  vmlinx-3.2.0-4-4kc-malta pass the path of the kernel which you downloaded in -kernel argument)

Simple build and test with KVM
This example will show an in-tree build. 
## Switch to the QEMU root directory
<pre>cd qemu</pre>
## Configure QEMU for mispel only - faster build
<pre>./configure --target-list=mipsel-softmmu --enable-debug</pre>
## Build in parallel - my system has 4 CPUs
<pre>make -j4</pre>
## Download an install ISO -from the link mentioned below

[Download_iso_image](https://cdimage.debian.org/debian-cd/current/mips/iso-cd/) 

(I have debian-10.3.0-mipsel-netinst.iso)
Run your QEMU using VNC
<pre> mipsel-softmmu/qemu-system-mipsel -hda ~/Downloads/debian_wheezy_mipsel_standard.qcow2 -M malta -kernel ~/Downloads/vmlinux-3.2.0-4-4kc-malta -append "root=/dev/sda1 console=tty0" -nic user,ipv6=off,model=e1000,mac=52:54:98:76:54:32</pre>

(here nic user,ipv6=off is a network card to enable networking you can skip this flag if you dont want internet)
