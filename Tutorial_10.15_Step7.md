# Step 7: Fixes / Enhancements / Alternative Solutions / Bugs

## HDMI/VGA Video-Out Fix for MBP13,3
Open /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist  
Find the Board-ID which used in your config.plist, default in this tutorial is "Mac-B809C3757DA9BB8D". Differs when using different smbios.  
Replace the attribute Config2 with none  
`sudo kextcache -system-prelinked-kernel && sudo kextcache -system-caches`  
reboot 

## Error: couldn't allocate runtime area / unable to start installer / unable to boot at all
Since OSXAptio is a lil bit picky with memory maps, you have to swap to OSXAptioV2 and choose a different slide= command (see question above) to a suitable number. First delete the OsxAptioFixDrv-64.efi from your CLOVER/drivers64UEFI and replace it with OsxAptioFix2Drv-64.efi from the folder aptiov2 in drivers64uefi. Then change the slide param in the config.plist. See [this Tutorial](/Additional/slide_calc.md) for more informations. It is still possible you cant get it to boot because no memory section is big enough. There are multiple different options available, but each of them has to be considered experimental or buggy. [More details](/10.15/CLOVER/drivers64UEFI/other/readme.md)
  
## Error: same as above, but additionally mentiones "unable to load kernel cache"
this normally only occurs on installation with firmware 1.2.25. If you have this message as well as the couldn't allocate runtime area, then there is a high posibility you can boot the installation with `OsxAptioFix2Drv-free2000.efi` instead of `OsxAptioFix2Drv-64.efi`. You can find the free2000 version in ./10.15/CLOVER/drivers64UEFI/Other. Replace them and install normally. Free2000 is not very stable and sometimes crash on start, so switch back to the normal version after installation. Sometimes you still need the slide parameter from above, sometimes you dont.

## clover doesnt boot OR Model Name Error 
if you get "Model Name: Apple device" in "About this mac" or your mac cant boot without the USB stick - then you're not loading the cloverx64.efi from your EFI. Simply update your EFI configuration by adding it to the boot order by hand. See [Additional/Setup-Bootmanager.jpg](Additional/Setup-Bootmanager.jpg) how to configure to boot from it 

## On installation: OSInstall.MPKG missing or corrupt
a bug, which occurs if there are more than one possible targets. Delete `EmuVariableUEFI-64.efi` from the boot stick to continue the installation.  

## Audio Fixes
### Audio Fix by using ported hdaverb
this is the newest fix by KNNSpeed - engineered for the Dell 9560, but works on the 9550, too. This has a dependency on Lilu.kext, which is rarely known to generate kernel panics on boot - especially on upgrades. Keep that in mind.  
See [this Tutorial](/10.15/Post-Install/Additional\ Steps/Audio/ComboJack/README.md)  
folder: ./10.15/Post-Install/Additional\ Steps/Audio/ComboJack

### Audio Fix by using patched AppleHDA - DEPRECATED
alternative to VoodooHDA and with better compatibility, but less stability. Requires replacing the AppleHDA Kext from Apple
See [this Tutorial](/10.15/Post-Install/Additional\ Steps/Audio/AppleHDA_sysCL/readme.md)  
folder: ./10.15/Post-Install/Additional\ Steps/Audio/AppleHDA_sysCL

## Display Backlight Control not working
the supplied AppleBacklightInjector contains an id for the display. It is possible that this id is different on your machine (especially if you use the non touch display). In this case just follow [this tutorial](Additional/PatchAppleBacklight_v2/readme.md)

## Display Flickers randomly
See tutorial in [Additional/EDID-Injector](Additional/Tools/EDID-Injector) for possible fix.  

## Display ICC Calibration
ICC profile for 4k screen calibrated with Spyder4Pro colorimeter and DisplayCAL is available in Additional/Profiles.   
Every panel is a lil bit different, so don't expect too much precision, but this profile works great for sRGB and AdobeRGB.

## SSDT / DSDT Modifications
You don't have to decompile the DSDT/SSDT files by yourself. The source dsl files are available in ./10.15/Advanced/DSDT-HotPatches/Patches. Use these for modifications.

## NVRAM Emulation / Saving Sound and Brightness settings after reboot
the native nvram installed in the Dell is not usable right now because of the Aptiofix. Clover can emulate this storage. Just install clover normally, but select "Advanced" when asked for the location of the installation. Now select "Install all RC Scripts on the target partition". You can find the installation files for clover in ./Additional/Clover_v2.4k_r4003.pkg - but i suggest downloading the newest from [Sourceforge](https://sourceforge.net/projects/cloverefiboot/)

## Sleep results in reboot
This is only in case sleep worked in the past. If you have sleep issues from the beginning and you strictly followed this tutorial (check at least twice!), you need additional assistance (easiest way is asking in a forum).  
  
Sometimes (especially on a dual boot environment after booting in the other OS) a normal sleep results in a full (and dirty) reboot. For me the old behaviour can be obtained by issuing this command: `sudo pmset -a hibernatemode 0 && sudo reboot`, albeit already being in hibernatemode 0. The reboot is mandatory, otherwise it doesn't work. Some people reported this fixes their problems, while other still had sleep issues. Just give it a shot.

## Additional Resources / Request help
It's much to read, but this thread include many solutions to the less common problems. Please read every post before asking a question:  
http://www.insanelymac.com/forum/topic/319764-guide-dell-xps-15-9550-sierra-10122-quick-installation/  
also please check if your question is already answered here: https://github.com/wmchris/DellXPS15-9550-OSX/issues?q=is%3Aissue+is%3Aclosed
