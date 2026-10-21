Creation of VHD on Windows and Restoring on a Different Host



\## Aim

To create a Virtual Hard Disk (VHD) on Windows and restore it on another host.



\## Part A: Create a VHD on Windows



1\. Press `Win + R`, type `diskmgmt.msc`, and press Enter.

2\. In Disk Management, select \*\*Action → Create VHD\*\*.

3\. Browse and choose a location to save the VHD file.

4\. Specify the virtual hard disk size.

5\. Select the VHD or VHDX format.

6\. Choose \*\*Fixed size\*\* or \*\*Dynamically expanding\*\*.

7\. Click OK to create the virtual disk.

8\. Right-click the new disk and select \*\*Initialize Disk\*\*.

9\. Right-click the unallocated space and select \*\*New Simple Volume\*\*.

10\. Assign a drive letter and format the volume using NTFS.



\## Part B: Restore the VHD on Another Host



1\. Copy the `.vhd` or `.vhdx` file to the other computer.

2\. Open Disk Management using `diskmgmt.msc`.

3\. Select \*\*Action → Attach VHD\*\*.

4\. Browse to the VHD file and click OK.

5\. If necessary, assign a drive letter to the partition.

6\. Access the files on the attached virtual disk.



\## Part C: Storage Virtualization Using LVM



\### Step 1: Create a Linux VM



1\. Open VirtualBox and create or select an Ubuntu VM.

2\. Allocate RAM and configure the VM.

3\. Add an additional 7 GB dynamically allocated virtual disk through \*\*Settings → Storage\*\*.



\### Step 2: Check Existing Disks and LVM



Run these commands inside Ubuntu:



sudo fdisk -l

sudo pvdisplay

sudo vgdisplay

sudo lvdisplay



\### Step 3: Create a Physical Volume



First, identify the new disk using `lsblk`. If the new disk is `/dev/sdb`, run:



sudo pvcreate /dev/sdb



\### Step 4: Extend the Volume Group



Replace `ubuntu-vg` with the actual volume group name displayed by `vgdisplay`.



sudo vgextend ubuntu-vg /dev/sdb



\### Step 5: Extend the Logical Volume



Check the logical volume path using `lvdisplay`. To add 3 GB to the root logical volume:



sudo lvextend -L +3G /dev/ubuntu-vg/root



\### Step 6: Resize the Filesystem



For an ext4 filesystem:



sudo resize2fs /dev/ubuntu-vg/root



Alternatively, resize the logical volume and filesystem together:



sudo lvextend -r -L +3G /dev/ubuntu-vg/root



Use only one of the two resizing methods.



\### Step 7: Verify the Available Space



df -h



\## Result

A VHD was created on Windows and attached on another host. Storage virtualization was demonstrated by extending an LVM volume in Ubuntu.



\## Precautions



\- Verify the correct disk device before running `pvcreate`.

\- Back up important data before modifying disks or logical volumes.

\- Ensure the destination host supports the VHD/VHDX format.

\- Confirm that the volume group has enough free space before extending a logical volume.+

