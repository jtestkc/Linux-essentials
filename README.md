# Linux-Swap


(all versions)
Amount of installed RAM	Recommended swap space	Recommended swap space if allowing for hibernation
2GB or less	Twice the installed RAM	3 times the amount of RAM
> 2GB - 8GB	The same amount of RAM	2 times the amount of RAM
> 8GB - 64GB	At least 4GB	1.5 times the amount of RAM
> 64GB or more	At least 4GB	Hibernation not recommended
Note: A swap space of at least 100GB is recommended for systems with over 140 logical processors or over 3TB of RAM.

The following items also influence the decision on how much swap space should be allocated:

Do specific application requirements exist? Applications may have been written with a specific amount of swap space in mind. If this is the case, the system should be set up with the amount of swap that is recommended by the application vendor.
Do other requirements exist? Workstations and laptops may use hibernation functions that store the RAM contents in the swap area. In this case, the swap space would need to be equal to or greater than the amount of RAM installed in the system to enable hibernation.
Assigning swap as 'last effort' memory While the block devices hosting swap are generally many times slower than RAM, it is useful to have swap as another layer of memory that's available when needed. In the case of applications with high memory utilization, swap space can allow memory to be swapped out to disk to delay or prevent the termination of applications by the OOM killer.
Virtual guests: for virtual guests, the same considerations as for physical systems apply. Also for these, using a bit of swap can influence the behaviour of a process requesting more and more memory, so the process gets slowed down first (leaving time for a sysadmin to manually fix the situation) before eventually also the swap is exhausted and the OOM killer terminates processes. If the memory written by the processes is not exceeding the available swap, the system will just experience a temporary slowdown.


<b>Create a swap file</b>
1.    Use the dd command to create a swap file on the root file system. In the command, bs is the block size and count is the number of blocks. The size of the swap file is the block size option multiplied by the count option in the dd command. Adjust these values to determine the desired swap file size.

The block size you specify should be less than the available memory on the instance or you receive a "memory exhausted" error.

In this example dd command, the swap file is 4 GB (128 MB x 32):

$ sudo dd if=/dev/zero of=/swapfile bs=128M count=32
2.    Update the read and write permissions for the swap file:

$ sudo chmod 600 /swapfile
3.    Set up a Linux swap area:

$ sudo mkswap /swapfile
4.    Make the swap file available for immediate use by adding the swap file to swap space:

$ sudo swapon /swapfile
5.    Verify that the procedure was successful:

$ sudo swapon -s
6.    Start the swap file at boot time by editing the /etc/fstab file.

Open the file in the editor:

$ sudo vi /etc/fstab
Add the following new line at the end of the file, save the file, and then exit:

/swapfile swap swap defaults 0 0
