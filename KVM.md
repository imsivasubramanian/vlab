\# Installation of KVM and Creation of Virtual Instances



\## Aim

To install KVM virtualization tools in Ubuntu and create virtual machine instances using QEMU and Virtual Machine Manager.



\## Step 1: Update Package Lists



sudo apt update



\## Step 2: Install KVM and Required Packages



sudo apt install -y qemu-kvm qemu-system-x86 libvirt-daemon-system libvirt-clients bridge-utils virt-manager qemu-utils



\## Step 3: Start the Libvirt Service



sudo systemctl enable --now libvirtd



Check the service status:



sudo systemctl status libvirtd



\## Step 4: Check Hardware Virtualization Support



egrep -c '(vmx|svm)' /proc/cpuinfo



Check the loaded KVM modules:



lsmod | grep kvm



\## Step 5: Verify Virtual Machine Management



virsh list --all



If necessary, run:



sudo virsh list --all



\## Step 6: Open Virtual Machine Manager



virt-manager



\## Step 7: Create a Virtual Machine



1\. Open Virtual Machine Manager.

2\. Select File → New Virtual Machine.

3\. Choose the installation ISO image.

4\. Allocate RAM and CPU cores.

5\. Create a virtual disk.

6\. Click Finish to create the virtual machine.





Download : wget https://download.cirros-cloud.net/0.5.1/cirros-0.5.1-x86\_64-disk.img





\## Result



KVM virtualization tools can be installed and configured in Ubuntu, provided that the environment supports hardware virtualization. Virtual machine creation can then be performed using Virtual Machine Manager.



