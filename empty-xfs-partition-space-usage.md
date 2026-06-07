# Why a 652GB empty XFS partition was consuming 13GB of space? </br>  
After setting my Fedora Workstation 43 host computer, I noticed that the XFS partition created for the purpose of storing Virtual Machines was consuming space for no reason. </br>  
The 652GB partition had no data in it but using the command **df -hT** I noticed that 13GB of space was consumed already. </br>  
as@asfed:\~$ df -hT  
Filesystem     Type      Size  Used Avail Use% Mounted on  
/dev/nvme0n1p3 ext4       92G  7.1G   80G   9% /  
devtmpfs       devtmpfs   32G     0   32G   0% /dev  
tmpfs          tmpfs      32G   96K   32G   1% /dev/shm  
efivarfs       efivarfs  384K  113K  267K  30% /sys/firmware/efi/efivars  
tmpfs          tmpfs      13G  2.0M   13G   1% /run  
tmpfs          tmpfs     1.0M     0  1.0M   0% /run/credentials/systemd-journald.service  
tmpfs          tmpfs     1.0M     0  1.0M   0% /run/credentials/systemd-cryptsetup@luks\x2d39ddb133\x2d0244\x2d4aa3\x2d96cf\x2da418697f8b7f.service  
tmpfs          tmpfs      32G  480K   32G   1% /tmp  
/dev/nvme0n1p1 ext4      1.8G  425M  1.3G  25% /boot  
/dev/nvme0n1p2 vfat      953M   20M  933M   3% /boot/efi  
/dev/nvme0n1p5 xfs        44G  890M   43G   2% /isos  
/dev/nvme0n1p6 xfs       652G   13G  640G   2% /vms  
/dev/dm-0      ext4      137G  345M  130G   1% /home  
tmpfs          tmpfs     1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service  
tmpfs          tmpfs     6.3G  3.8M  6.3G   1% /run/user/1000 </br>  

Further research using Claude AI tool, I found that XFS pre-allocates internal structures while the filesystem is created. This overhead for a 652GB partition can be nearly 13GB. </br>  
The reason I chose to use XFS specifically for this partition was because XFS is fast when handling large files, but I did not know there would such a cost to it. </br>  
However since this 13GB of space consumption is a one-time cost and it would NOT grow beyond this, I chose to keep the filesystem as such. </br>  
From the image below, we can note that, as of today the filesystem usage is at 29GB while the disk usage is 17GB, resulting in the 12-13GB overhead. </br>  
<img width="1235" height="595" alt="Screenshot From 2026-06-07 22-31-41" src="https://github.com/user-attachments/assets/4d850523-1c55-4a21-be6e-e0676351f1fb" />
