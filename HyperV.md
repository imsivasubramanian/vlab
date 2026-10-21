VM Installation Using PowerShell - ( Hyper-V )



\## Aim

To create and manage a virtual machine using Hyper-V PowerShell commands.



\## 1. Set VM Details



$VMName = "SivaVM"

$VMPath = "C:\\HyperV\\SivaVM"

$ISOPath = "C:\\ISO\\ubuntu.iso"

$SwitchName = "Default Switch"



\## 2. Display Available Virtual Switches



Get-VMSwitch



\## 3. Create VM Folder



New-Item -ItemType Directory -Path $VMPath -Force



\## 4. Create Virtual Machine and Virtual Hard Disk



New-VM -Name $VMName -Path $VMPath -NewVHDPath "$VMPath\\SivaVM.vhdx" -NewVHDSizeBytes 30GB -MemoryStartupBytes 2GB -Generation 2



\## 5. Connect Virtual Switch



Connect-VMNetworkAdapter -VMName $VMName -SwitchName $SwitchName



\## 6. Attach Ubuntu ISO



Add-VMDvdDrive -VMName $VMName -Path $ISOPath



\## 7. Set DVD as First Boot Device



Set-VMFirmware -VMName $VMName -FirstBootDevice (Get-VMDvdDrive -VMName $VMName)



\## 8. Configure Secure Boot for Ubuntu



Set-VMFirmware -VMName $VMName -EnableSecureBoot On -SecureBootTemplate MicrosoftUEFICertificateAuthority



\## 9. Start the Virtual Machine



Start-VM -Name $VMName

vmconnect.exe localhost SivaVM



\## 10. Check VM Status



Get-VM -Name $VMName



\## 11. Stop the Virtual Machine



Stop-VM -Name $VMName



\## 12. Start the VM Again



Start-VM -Name $VMName





\## Result

The virtual machine named SivaVM is created and managed using Hyper-V PowerShell commands.

