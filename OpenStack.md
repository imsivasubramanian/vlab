\## Installation of OpenStack and Creation of Virtual Machines



\## Aim

To install OpenStack in a Linux system and create a virtual machine.



\## 1. Update Ubuntu Packages



sudo apt update



\## 2. Install Packages Required for DevStack



sudo apt install git python3-pip -y

sudo apt install build-essential -y



\## 3. Create the Stack User



sudo useradd -s /bin/bash -d /opt/stack -m stack

sudo chmod +x /opt/stack

echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

sudo chmod 0440 /etc/sudoers.d/stack



\## 4. Switch to the Stack User



sudo su - stack



\## 5. Download DevStack



git clone https://opendev.org/openstack/devstack

cd devstack



\## 6. Configure OpenStack



nano local.conf



Add the following configuration, replacing the password with a strong password for your lab:



\[\[local|localrc]]

ADMIN\_PASSWORD=YourStrongPassword

DATABASE\_PASSWORD=$ADMIN\_PASSWORD

RABBIT\_PASSWORD=$ADMIN\_PASSWORD

SERVICE\_PASSWORD=$ADMIN\_PASSWORD



Save and close the file.



\## 7. Install OpenStack



./stack.sh



Wait for the installation to finish. This may take a considerable amount of time.



\## 8. Access the OpenStack Dashboard



Open a browser and visit the IP address configured for your DevStack installation. Log in using the configured administrator credentials.



http://<YOUR\_UBUNTU\_IP>/dashboard



Find your Ubuntu IP using:



hostname -I



\## 9. Load OpenStack Credentials



From the DevStack directory:



source openrc admin admin



\## 10. List Available Images and Flavors



openstack image list

openstack flavor list



\## 11. Create a Key Pair



openstack keypair create mykey > mykey.pem

chmod 600 mykey.pem



\## 12. Check Available Networks



openstack network list



Note the network ID you intend to use.



\## 13. Create a Virtual Machine



Replace the image, flavor, and network placeholders with valid values from your environment.



openstack server create \\

&#x20; --image <image\_name\_or\_id> \\

&#x20; --flavor <flavor\_name\_or\_id> \\

&#x20; --nic net-id=<network\_id> \\

&#x20; --key-name mykey \\

&#x20; my-first-vm



\## 14. Verify Virtual Machine Status



openstack server list



\## 15. Connect to the Virtual Machine



For an accessible VM with SSH enabled, use the appropriate username and reachable IP address:



ssh -i mykey.pem <username>@<vm\_ip\_address>



\## Result

OpenStack was installed on the Linux system, and a virtual machine was created and its status verified.

