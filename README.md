# MacBookPro 7,1 Running Linux Mint 20.2 Uma January 2022 Guide: 

I decided to revive my old Macbook Pro Late 2010 / 7,1. and Install UT99 for some retro gaming goodness. Below are the steps I used to get it all working. 

**To get the NVIDA 340 Graphics driver to work there are several issues:** 

Problem 1: By default Linux Mint uses the nuoveau graphics drivers so games like UT99 don't work well.  
Problem 2: Grub Graphics Mode doesn't switch reliably to Nvidia Drivers because of Apple's weird hybrid UEFI. Linux Mint Video after boot is garbled.  
Problem 3: Screen brightness controls don't work.  


I followed the guide the link below to install the Nvidia Drivers. However, I wasn't able to get the screen brightness controls working with the link at the end of the artice. DO NOT REBOOT until grub has been modified as outlined in Fix 2.  

Fix 1: https://askubuntu.com/questions/264247/proprietary-nvidia-drivers-with-efi-on-mac-to-prevent-overheating/613573#613573  

Fix 2: grub needs to be changed to the following:  

sudo nano /etc/default/grub  

GRUB_DEFAULT=0  
GRUB_TIMEOUT_STYLE=  
GRUB_TIMEOUT=3  
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`  
GRUB_CMDLINE_LINUX="nvidia-drm.modeset=1"  
GRUB_CMDLINE_LINUX=""  
GRUB_TERMINAL=console  
    
sudo updategrub

Heads Up: If you get garbled video on boot: type in your password then hit enter. Hit CTRL+ALT+DEL 2x and it shoud show the NVIDIA Logo and then display properly. You will need to login again.


Fix 3: I used System Settings Keyboard to create shortcuts to control the screen brightness controls:


sudo apt install xbacklight

Increase screen brightness:  
xbacklight -inc 02 -time 0  

Decrease screen brightness:  
xbacklight -dec 02 -time 0  


**Download UT99 and Patch 469b** 

Download UT99 from Archive.org:  
https://ia902800.us.archive.org/15/items/UnrealTournamentGOTYUSA/Unreal%20Tournament%20%28USA%29%20%28Disc%201%29%20%28Game%20of%20the%20Year%20Edition%29%20%28Rerelease%29.zip  

Download UT 469b Patch  
https://github.com/OldUnreal/UnrealTournamentPatches/releases  

**Install wine, winetricks, and iat:**

sudo apt install wine32  
sudo apt install winetricks  
winetricks orm=backbuffer glsl=disable
sudo apt install iat    

**Use iat to turn the .bin to a .iso:** 

cd Downloads/

extract the .zip file so that the .bin and .cue files are present in the Downloads/ folder.

iat 'Unreal Tournament (USA) (Disc 1) (Game of the Year Edition) (Rerelease).bin' > UT.iso

**Mount and Install UT99 and Patch 469b**

sudo mount -o loop UT.iso /mnt  
cd /mnt  
wine Setup.exe   
cd ~  
cd Downloads/  
wine OldUnreal-UTPatch469b-Windows.exe  
