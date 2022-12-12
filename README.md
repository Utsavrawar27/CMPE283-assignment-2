# FA22: CMPE-283 Sec 48 - Virtual Technologies Assignment-3

## Start with your Assignment-2 Environment:
Below are the steps explaind to execute Assignment-3 on Ubuntu VM hosted on VMWare Workstation.

## Contributor:
Utsav Rawat

## System Specification Requirements:

* Processor	Intel(R) Core(TM) i5-10210U CPU @ 1.60GHz   2.11 GHz
* Installed RAM	8.00 GB (7.79 GB usable)
* System type	64-bit operating system, x64-based processor
* VMware® Workstation 16 Pro Trial/Download
* Download ubuntu-22.10-desktop-amd64
* Intel® 64 and IA-32 Architectures Software Developer Manuals (SDM).

## Enabling Nested Virtualization
To enable nested virtualization for VMs running on Windows 11
Setup an Ubuntu virtual machine using VMware® Workstation. Enable the Virtualize Intel VT-x/EPT setting under Virtual Machine Settings > Hardware > Device > Processor. 
By navigating to the virtual machine's settings, as shown below.

![Screenshot 2022-12-05 143552](https://user-images.githubusercontent.com/40047632/205778990-40bed71b-213b-4da0-bd61-e9982caabd5c.png)


### Steps:
**1. Install	git	using	following	commands:**
- `sudo	apt-get	update`
- `sudo	apt-get	upgrade`
- `sudo	apt-get	install	git`

**2. Clone linux	kernel	tree	from	github	using	following	command**
- `git clone	https://github.com/torvalds/linux.git`

**3. Change	directory	to linux**
- `cd linux`

![image](https://user-images.githubusercontent.com/40047632/205765171-b60f1bb9-6c82-4bbb-8047-7c372c59b7f5.png)

**4. Run Below Command before	trying to build	the	Linux	Kernel:**
- `apt-get install libncurses-dev`
- `apt-get install libssl-dev`
- `apt-get install bison`
- `apt-get install flex`
- `apt-get install libelf-dev`
- `apt-get install make`

**5. Change the .config file:Replace (uname -r)**
- `cp /boot/config-$(uname -r) ./.config` <br />
  example: cp /boot/config-5.19.0-26-generic ./.config

**6. Make oldconfig file. (Hold Enter,use the default for everything)**
- `sudo make oldconfig`

**7. Make yourself as root user run the following instruction in "Linux" folder one by one, using no of cores(4) VM has:**
- `make -j 4`
- `make modules -j 4`
- `make install -j 4`
- `make modules_install -j 4` <br />
  These steps might need 2-3hr hours to complete for the first time.

**8. Reboot the Ubuntu machine:**
- `sudo reboot`

**9. Verify your updated kernel after reboot:**
- `uname -a`

![image](https://user-images.githubusercontent.com/40047632/205770965-5f9fe0d1-08e6-4650-a2ce-06df7dfc2513.png)

## Modify the kernal code
We have to modified /linux/arch/x86/kvm/cpuid.c file and /linux/arch/x86/kvm/vmx/vmx.c file. Apply the required logic to support for CPUID leaf 0x4FFFFFFF, 0x4FFFFFFE, 0x4FFFFFFD, 0x4FFFFFFC

![image](https://user-images.githubusercontent.com/40047632/206939317-55ae651a-8e52-4d46-91d0-10f3f5b2e560.png)

![image](https://user-images.githubusercontent.com/40047632/206938333-a26837be-7a52-4c7c-b8f9-948b2121a881.png)

**10. After modification we need to rebuild the kernal using make command.**
- `make modules -j 4`

![image](https://user-images.githubusercontent.com/40047632/205773571-7342ea85-9cc2-4bdb-8f03-d0c8a4ab6ca5.png)

**11. Install Modules:**
- `make INSTALL_MOD_STRIP=1 modules_install -j 4`

**12. Load newly build KVM modules using below commands:**
- `lsmod | grep kvm`
- `rmmod kvm_intel`
- `rmmod kvm`
- `modprobe kvm_intel`
- `modprobe kvm`

![image](https://user-images.githubusercontent.com/40047632/205774425-7e06dbad-cf0b-44d8-bb01-14f7c8ac2431.png)

**13. To test our build, we need to create a new nested VM inside VMware® Workstation. The steps to create an nested VM are as follows:**
  First installed below few libraries
- `sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils`
- `sudo apt-get install qemu-system`

**14. Install the virt-manager using:**
- `sudo apt-get install virt-manager`

**15. Once virt-manager is installed we can launch the 'Virtual Machine Manager', located in Installed Apps.**

**16. Downloaded "Linux Lite" iso file. Use this iso file in 'Virtual Machine Manager' to launch the nested VM.**

![image](https://user-images.githubusercontent.com/40047632/205775791-e8c8c7a6-f3b4-4256-b31c-b70b390eda9f.png)

**17. Install below libraries inside new VM:**
- `sudo apt-get install cpuid`
- `sudo apt-get install gcc`

![image](https://user-images.githubusercontent.com/40047632/205776554-edb061d3-e55e-45cb-9638-7133b676a980.png)


**18. Test the leaf node eax=0x4FFFFFFE and eax=0x4FFFFFFF using cpuid tool**
- `cpuid -l 0x4FFFFFFE`
- `cpuid -l 0x4FFFFFFF`

# Question-3:
1. Does the number of exits increase at a stable rate? <br />
From the result of the first run and second run, not all exits increase at a stable rate. Most exits increase with distinct increment ratios depending on the exit type. Very few exits values remain the same. <br />

2. Are there more exits performed during certain VM operations? <br />
Yes, more exits are performed during certain VM operations; whenever any application or browser runs, the exit increase but not at a stable rate. <br />

3. Approximately how many exits does a full VM boot entail? <br />
Total number of exits after full VM boot entail are around 377k. <br />

# Question-4:
1. Of the exit types defined in the SDM, which are the most frequent? Least? <br />
Most frequent exit type is 48, which is 'EPT violation'. <br />
Least frequent exit type is 29 i.e 'MOV DR' and 47 i.e 'Access to LDTR or TR'

## Output Result:

![image](https://user-images.githubusercontent.com/40047632/206957198-efadd921-5363-401b-ba04-8131647c3983.png)

First Run Output:-
![image](https://user-images.githubusercontent.com/40047632/206957242-6406bb74-7887-4d05-a7b8-e509afcdd65a.png)

![image](https://user-images.githubusercontent.com/40047632/206957296-9f6cdbc9-6b14-4d23-a16f-4635a8a2fd13.png)

![image](https://user-images.githubusercontent.com/40047632/206957329-d6ea255f-8f8f-49ee-b9ad-5d8d69ff8d29.png)

![image](https://user-images.githubusercontent.com/40047632/206957349-80d33818-3f22-4d6f-8a35-393bb2d2b4fd.png)

![image](https://user-images.githubusercontent.com/40047632/206957375-6807b1e1-b18b-4cc7-94b2-9aaa6e920824.png)

After Boot Output:-
![image](https://user-images.githubusercontent.com/40047632/206957550-15c8989c-642a-4e00-9c6a-87620e9e8edf.png)

![image](https://user-images.githubusercontent.com/40047632/206957600-6d0a31b8-2aad-4429-b4ff-64adcf13f8f9.png)


















