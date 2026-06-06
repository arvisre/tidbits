I had BleachBit 4.6.0 installed on my Fedora Workstation 43. However, on the BleachBit website the release for Fedora 43 was 6.0.0.  
So I downloaded the update and installed it with "Software Install".  
Later, from the terminal I ran the command sudo dnf check-update and sudo dnf distro-sync.  
I usually update software using the above mentioned commands. As I ran those commands, I noted that dnf replaced version 6.0.0 with 4.6.0.  

## Learning:
distro-sync will upgrade / degrade software versions that are found in the repository.
Hence, in this case I must have used sudo dnf check-update and sudo dnf upgrade.

### From the Internet:
"sudo dnf upgrade updates installed packages to the newest version available in your enabled repositories, but it generally does not downgrade packages."  
"sudo dnf distro-sync tries to make your installed packages match the repository versions exactly, so it can upgrade, downgrade, or keep packages at the current version if needed"  
