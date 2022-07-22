# Linux SWAP AND SWAPPINESS BY AJAY
 <a class="header-badge" target="_blank" href="https://www.linkedin.com/in/ajay-kc31">
  <img src="https://img.shields.io/badge/style--5eba00.svg?label=LinkedIn&logo=linkedin&style=social">
  </a>



<b>What is swap?</b>

Swap space is the area on a hard disk. It is a part of your machine's Virtual Memory, which is a combination of accessible physical memory (RAM) and the swap space. Swap holds memory pages that are temporarily inactive. Swap space is used when your operating system decides that it needs physical memory for active processes and the amount of available (unused) physical memory is insufficient. When this happens, inactive pages from the physical memory are then moved into the swap space, freeing up that physical memory for other uses. Note that the access time for swap is slower, depending on the speed of the hard drive. Do not consider it to be a complete replacement for the physical memory. Swap space can be a dedicated swap partition (recommended), a swap file, or a combination of swap partitions and swap file(s).

<b>How much swap do I need?</b>
For less than 1GB of physical memory (RAM), it's highly recommended that the swap space should, as a base minimum, be equal to the amount of RAM. Also, it's recommended that the swap space is maximum twice the amount of RAM depending upon the amount of hard disk space available for the system because of diminishing returns.

For more modern systems (>1GB), your swap space should be at a minimum be equal to your physical memory (RAM) size "if you use hibernation", otherwise you need a minimum of round(sqrt(RAM)) and a maximum of twice the amount of RAM. The only downside to having more swap space than you will actually use, is the disk space you will be reserving for it.

The "diminishing returns" means that if you need more swap space than twice your RAM size, you'd better add more RAM as Hard Disk Drive (HDD) access is about 10³ slower then RAM access, so something that would take 1 second, suddenly takes more then 15 minutes! And still more than a minute on a fast Solid State Drive (SSD)

<b>Do specific application requirements exist?</b> 
Applications may have been written with a specific amount of swap space in mind. If this is the case, the system should be set up with the amount of swap that is recommended by the application vendor.
Do other requirements exist? Workstations and laptops may use hibernation functions that store the RAM contents in the swap area. In this case, the swap space would need to be equal to or greater than the amount of RAM installed in the system to enable hibernation.
Assigning swap as 'last effort' memory While the block devices hosting swap are generally many times slower than RAM, it is useful to have swap as another layer of memory that's available when needed. In the case of applications with high memory utilization, swap space can allow memory to be swapped out to disk to delay or prevent the termination of applications by the OOM killer.
Virtual guests: for virtual guests, the same considerations as for physical systems apply. Also for these, using a bit of swap can influence the behaviour of a process requesting more and more memory, so the process gets slowed down first (leaving time for a sysadmin to manually fix the situation) before eventually also the swap is exhausted and the OOM killer terminates processes. If the memory written by the processes is not exceeding the available swap, the system will just experience a temporary slowdown.


<b><h1>Create a swap file on aws ec2 instance</h1></b>



1.    Use the dd command to create a swap file on the root file system. In the command, bs is the block size and count is the number of blocks. The size of the swap file is the block size option multiplied by the count option in the dd command. Adjust these values to determine the desired swap file size.

The block size you specify should be less than the available memory on the instance or you receive a "memory exhausted" error.

In this example dd command, the swap file is 4 GB (128 MB x 32):

```$ sudo dd if=/dev/zero of=/swapfile bs=128M count=32```

2.    Update the read and write permissions for the swap file:

```$ sudo chmod 600 /swapfile```

3.    Set up a Linux swap area:

```$ sudo mkswap /swapfile```

4.    Make the swap file available for immediate use by adding the swap file to swap space:

```$ sudo swapon /swapfile```

5.    Verify that the procedure was successful:

```$ sudo swapon -s```

6.    Start the swap file at boot time by editing the /etc/fstab file.

Open the file in the editor:

```$ sudo vi /etc/fstab```

Add the following new line at the end of the file, save the file, and then exit:

```/swapfile swap swap defaults 0 0```

<h1>What is swappiness and how do I change it?</h1>
The swappiness parameter controls the tendency of the kernel to move processes out of physical memory and onto the swap disk. Because disks are much slower than RAM, this can lead to slower response times for system and applications if processes are too aggressively moved out of memory.

swappiness can have a value of between 0 and 100
swappiness=0 tells the kernel to avoid swapping processes out of physical memory for as long as possible
swappiness=100 tells the kernel to aggressively swap processes out of physical memory and move them to swap cache
The default setting in Ubuntu is swappiness=60. Reducing the default value of swappiness will probably improve overall performance for a typical Ubuntu desktop installation. A value of swappiness=10 is recommended. <b>Note</b> : Ubuntu server installations have different performance requirements to desktop systems, and the default value of 60 is likely more suitable.


To check the swappiness value

```cat /proc/sys/vm/swappiness```

To change the swappiness value A temporary change (lost on reboot) with a swappiness value of 10 can be made with

```sudo sysctl vm.swappiness=10```

To make a change permanent, edit the configuration file with your favorite editor:

```gksudo gedit /etc/sysctl.conf```

<h1>Disable and Remove a Swap File</h1>

Disable the swap file from the running system and the delete it:

```sudo swapoff /swapfile ```

```sudo rm /swapfile```

Remove the swap file details from fstab:


```gksudo gedit /etc/fstab```
