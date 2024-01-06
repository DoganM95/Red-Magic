This is a root/flash/unbrick guide for Nubia devices i have laying around and comes with absolutely no warranty. 
The steps are partially tested, partially recreated from what i remember worked for me, use at your own risk.

# Prepare

## Download files (if not done already)
- Download the factory firmware from https://help.redmagic.gg/hc/en-us/articles/15828163597593-REDMAGIC-8-Pro  
- Download fastboot enhance from https://github.com/libxzr/FastbootEnhance
- Download the latest adb/fastboot executables from https://developer.android.com/tools/releases/platform-tools
- Download PathAdder (makes terminal commands easier) from https://github.com/DoganM95/AddToPath-Context-Entry-Windows
- Download the magisk app from https://github.com/topjohnwu/Magisk

## Prepare host pc

### Getting fastboot device ready

- Boot phone into fastboot by holding `power + vol down` keys until a screen with text appears
- Go to device manager in windows, you will see an unknown *Android Device*
- Right click it -> update driver -> browse my pc -> let me pick -> Android -> Android Bootloader Interface

### Extracting the partitions

- Extract payload.bin from the factory zip
- Open FastbootEnhance.exe
- Payload Dumper -> Browse -> payload.bin
- Choose 
  - boot
  - init_boot
  - system
  - vendor
- Click extract image (may take a while, wait for the pop-up)

### Extract fastboot tools
- Pick a location where the folder can stay permanently
- Extract the platform-tools.zip there
- "Install" AddToPath.reg by double clicking it
- Right click the extracted adb folder and `Add To Path`
- Now you can just run adb and fastboot from anywhere 

### Create magisk patched boot
- Install Magisk.apk on your or any other phone
- Put the init.boot.img on said phone
- Click patch manually and choose that init_boot.img

## Root with working fingerprint etc.

If you already rooted your device just to realize the fingerprint scanner & lockscreen doesn't work anymore or  
if you want to root a stock device, follow this guide on XDA:  
https://forum.xda-developers.com/t/root-redmagic-8s-pro.4617049/  

## Get Phone status

- Boot phone into fastboot by holding `power + vol down` keys until a screen with text appears
- Connect phone to PC and open a terminal
- `fastboot getvar all` to get the current device state, including information like
  - ```
    slot-unbootable:a/b:yes/no
    slot-successful:a/b:yes/no
    current-slot:a/b
    ``` 

## Unbricking the device

- Boot phone into fastboot by holding `power + vol down` keys until a screen with text appears

### Switching slots (Quick and dirty way)

- `fastboot getvar all`
- Check if the slot you are not on is 
  - `slot-unbootable:no` and
  - `slot-successful:yes`
- `fastboot --set-active=<slot_you_are_not_on>` and replace `<slot_you_are_not_on>` with `a` or `b`
- Phone should boot successfully into the switched slot, but now the other slot will be broken and it is recommended to fix it

### Flashing an OTA (Sideload)
- From Fastboot menu, choose to boot into recovery
- A white screen will appear with a green button, don't click anything
- `adb devices` should show a sideload device
- `adb sideload .\NX729J-update.zip` will upload and flash the OTA
- 

<!-- ### Flashing partitions (not quick but clean way)

- Run FastbootEnhance.exe
- Double click the recognized fastboot device
- Click *Reboot to fastbootd*
- The phone will show a white screen for a whilem then yellow and blue text (=fastbootd)
- Click Flash Payload.bin
- A warning will appear, click yes and wait for the flashing to finish
- Slot is now repaired and bootable -->

## Rooting (flash a patched magisk boot image)

- Install and open Magisk app on your or any working android phonw
- Upload the init_boot.img to said phone
- Choose patch image and choose the init_noot.img
- A patched image will be located in /Download, transfer that to the host pc
- `fastboot flash init_boot ./magisk_patched_something.img` will flash it to the currently active slot
- `fastboot reboot`
