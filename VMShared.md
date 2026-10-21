VMShared VM to VM



\## 4. Access Shared Folder \& Fix Permissions



Check mounted devices in `/media`. The shared folder appears as `sf\_VMShared`:



\### Terminal (VM1 \& VM2)



ls /media

ls /media/sf\_VMShared



\### Permission Denied Fix



If regular users are denied access to `/media/sf\_VMShared`, add your current user to the `vboxsf` system group and reboot:



\### Terminal — Add User to vboxsf Group



sudo usermod -aG vboxsf $USER

sudo reboot



After rebooting, re-verify access:



\### Terminal — Re-check Access



ls /media/sf\_VMShared



\### Fix Permissions with usermod (PDF Page 11)



Adding user to `vboxsf` group.



\### Access Granted (PDF Page 11)



Viewing `22IT043.txt` inside `sf\_VMShared`.





\## 5. Transfer Files Between VMs



In \*\*VM1\*\*, copy a file into the shared folder:



\### VM1 Terminal



cp VM1.txt /media/sf\_VMShared/



Switch to \*\*VM2\*\*, and verify that the file is immediately available:



\### VM2 Terminal



ls /media/sf\_VMShared

cat /media/sf\_VMShared/VM1.txt



Any change made in the shared folder from one VM is immediately accessible to the other VM.







\# Complete SSH Shell Method Implementation



Aim: To transfer files from one virtual machine to another using SSH (Secure Shell) and SCP (Secure Copy Protocol).



\## Step 1: Configure the VirtualBox network



1\. Shut down both VMs.

2\. Open VirtualBox and select VM1.

3\. Go to Settings → Network.

4\. Enable Adapter 1 and select Host-only Adapter.

5\. Choose the same host-only network for VM2.

6\. Click OK and start both VMs.



Host-only networking allows the VMs to communicate with each other. If you also need internet access for package installation, use a second NAT adapter or temporarily use a network configuration that provides internet access.



\## Step 2: Find the IP addresses



Open a terminal in VM1 and run:



ip -4 addr



Repeat in VM2:



ip -4 addr



Find the IPv4 addresses belonging to the shared VM network. For example:



\- VM1 IP: `192.168.56.103`

\- VM2 IP: `192.168.56.104`



These are examples only. Your actual IP addresses may differ.



\## Step 3: Install and start SSH on both VMs



Run the following commands in VM1:



sudo apt update

sudo apt install openssh-server openssh-client -y

sudo systemctl enable --now ssh

sudo systemctl status ssh



Confirm the status shows `active (running)`. Press `q` to exit.



Now run the same commands in VM2:



sudo apt update

sudo apt install openssh-server openssh-client -y

sudo systemctl enable --now ssh

sudo systemctl status ssh



\## Step 4: Allow SSH through the firewall



On VM1, run:



sudo ufw allow 22/tcp

sudo ufw status



If UFW is inactive, you generally do not need to enable it for this experiment. If another firewall is in use, allow TCP port 22 there.



\## Step 5: Test the connection from VM2 to VM1



On VM1, find your username:



whoami



Suppose it displays `tce`. On VM2, connect using VM1's username and IP address:



ssh tce@192.168.56.103



Replace `tce` with the username from VM1 and the IP with VM1's actual address.



The first connection may ask:



Are you sure you want to continue connecting (yes/no/\[fingerprint])?



Type:



yes



Then enter VM1's password. The password will not appear as you type; this is normal.



If successful, you will be logged into VM1 through VM2's terminal. To return to VM2, type:



exit



\## Step 6: Create a test file on VM2



Make sure you are back in VM2's terminal. Run:



cd \~

echo "Hello from VM2 - SSH file transfer test" > file.txt

cat file.txt



You should see:



Hello from VM2 - SSH file transfer test



\## Step 7: Copy a file from VM2 to VM1



Run this command in VM2:



scp \~/file.txt tce@192.168.56.103:/home/tce/



Replace `tce` and `192.168.56.103` with VM1's actual username and IP address.



Enter VM1's password if prompted.



Now, on VM1, verify the file:



ls -l /home/tce/file.txt

cat /home/tce/file.txt



You should see the file and its contents.



\## Step 8: Copy a file from VM1 to VM2



On VM1, create another test file:



cd \~

echo "Hello from VM1 - SSH file transfer test" > serverfile.txt

cat serverfile.txt



Now return to VM2 and run:



scp tce@192.168.56.103:/home/tce/serverfile.txt \~/



Enter VM1's password if requested.



Verify the received file on VM2:



ls -l \~/serverfile.txt

cat \~/serverfile.txt



Expected output:



Hello from VM1 - SSH file transfer test



\## Step 9: Final verification



Your results should look like this:



| Test                           | Expected result                  |

| ------------------------------ | -------------------------------- |

| SSH connection from VM2 to VM1 | Successful login                 |

| VM2 → VM1 using SCP            | `file.txt` received on VM1       |

| VM1 → VM2 using SCP            | `serverfile.txt` received on VM2 |

| Read transferred files         | Correct text displayed           |



\## Troubleshooting



| Error                        | Solution                                                           |

| ---------------------------- | ------------------------------------------------------------------ |

| `Connection timed out`       | Check both VMs use the same host-only network and can communicate. |

| `Connection refused`         | On VM1, run `sudo systemctl enable --now ssh`.                     |

| `Permission denied`          | Check VM1's username, password and destination permissions.        |

| `Could not resolve hostname` | Check the command syntax and IP address.                           |

| `No route to host`           | Check the network adapter settings and IP addresses.               |



\## Result



The SSH Shell Method was implemented successfully by connecting VM2 to VM1 using SSH and transferring files in both directions using SCP.

