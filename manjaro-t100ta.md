# This is a fork of [GeoffreyCoulaud's guide](https://gist.github.com/GeoffreyCoulaud/074d2c4495578c0602f6180bc1a2e6eb)
## Changes
* Since the 32 bit Manjaro is no longer available, I have attached a zip of the folder /boot/grub/i386-efi he mentioned in the guide.

# How to setup Manjaro on the Asus t100ta
Based on [this tutorial by *jbMacAZ* on forums.manjaro.org](https://forum.manjaro.org/t/manjaro-usb-with-ia32-efi-support/77180/6)  
Made on 26/05/2020 for the Asus t100ta and Manjaro 20.0.1  
These instructions **should** work for any 32bit EFI / 64bit CPU system, but I only tested it on the Asus t100ta, use at your own risk.  
I tested the community editions for *Budgie*, *Gnome* and *LXQt*, but this tutorial should be good for any 64bit Manjaro edition.   

## My setup
I'm using vanilla Ubuntu 20.04, you will need gparted and [unetbootin](https://unetbootin.github.io/) for this tutorial.  
You will also need a 4GB or more USB stick which data **will be wiped**.  
If like me your t100ta dock is broken, you can charge the computer normally until full, then use the charging micro-usb port to have an OTG cable + USB hub to connect the USB stick, a keyboard and a mouse.

## Steps
1. Download a 64bit manjaro iso of your choice from [the official website](https://manjaro.org/download/)
2. Download a 32bit manjaro iso from [the 32bit manjaro project site](https://osdn.net/projects/manjaro32/storage/)
3. Download `bootia32.efi` (or build it) from [*jfwells* "linux-asus-t100ta" github](https://github.com/jfwells/linux-asus-t100ta/tree/master/boot)
4. Format the USB stick to have one **fat32** partition of 100% size (you can use disks or gparted to do this)
5. Label the USB key `MANJARO` (you can use gparted for this)
6. Burn the 64bit iso to the stick with Unetbootin (this is important because you want to be able to write on the stick)
7. Copy `bootia32.efi` to `/efi/boot` on the stick
8. Copy the folder `/boot/grub/i386-efi` from the 32bit iso to `/boot/grub` on the stick
9. Edit on the stick `/boot/grub/kernels.cfg` replace `misolabel=anything_that_is_here` to `misolabel=MANJARO`
10. Edit on the stick `/boot/grub/grub.cfg`, find the `efi_detect` function, replace  
`for efi in (*,gpt*)/efi/*/*.efi (*,gpt*)/efi/*/*/*.efi (*,gpt*)/*.efi (*,gpt*)/*/*.efi ; do`  
with  
`for efi in (*,gpt*)/efi/*/*.efi (*,gpt*)/efi/*/*/*.efi (*,gpt*)/*.efi (*,gpt*)/*/*.efi (*,msdos*)/*/*/*.efi ; do`
11. On the t100ta, reboot and mash `F2` to go to BIOS/UEFI. **Disable secure boot** on the "boot" section
12. Plug in the USB stick, Save and exit, then mash `escape` to choose a boot option, then choose the USB stick
13. On the menu that appears, choose "install manjaro", then install regularly and reboot to your fresh insallation !
## Fixes
### The screen shuts off regularly and the system hibernates (around every 15 seconds)
1. **For Gnome desktop** ([based on *5bentz* "linux-asus-t100ta" github](https://github.com/5bentz/linux-asus-t100/issues/1))
	1. Install `gnome-tweak-tool`
	2. Open `gnome-tweaks`
	3. Go to the power section
	4. "suspend on lid closed off" => "off" 
	
2. **For Budgie desktop** ([based on a ubuntu budgie power management thread](https://discourse.ubuntubudgie.org/t/power-management/1012/6))
	1. Open `/etc/systemd/logind.conf` for edition as admin
	2. Remove the starting `#` on the lines `HandleLidSwitchXXXXX`
	3. On these lines, change `=suspend` to `=ignore` 
	
	*Note : You may want to let some of these options to suspend, for example the "docked" one* 

3. **For other desktops** 
	* I tested community edition *LXQt+openbox* edition which worked fine and didn't have this problem.  
	However, if you get this problem i think that the *For Budgie Desktop* section above may be the universal solution to this problem. I'm no expert though, so you may need some tinkering .
