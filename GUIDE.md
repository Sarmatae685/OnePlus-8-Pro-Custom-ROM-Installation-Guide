# How to install Custom ROM on OnePlus 8 Pro 

> [!NOTE]
> Potentially will work on any other OnePlus, I just have 1+8 Pro

---

- **Phone**: OnePlus 8 Pro (instantnoodlep)  
- **ROM**: Evolution X 11.2 Android 16  

### Contents

- [Unlock Developer Options](#unlock-developer-options)
- [Install drivers](#install-drivers)
- [Unlock the bootloader](#unlock-the-bootloader)
- [Flashing images and installing ROM](#flashing-images-and-installing-rom)


### What is needed

- OnePlus phone
- Windows/Linux PC
- USB cable (preferred original, but suits any cable)
- Battery charge 60%+
- Curiosity

## My stock ROM  

<img src="images/Stock%20ROM_1.jpg" alt="Stock ROM_1.jpg" width="30%"/>

---

## Unlock Developer Options

1. Tap 7-10 times on "Build number" to unlock "Developer options"
2. Go "Additional Settings" → scroll down → "Developer options"
3. In dev options find and set "OEM unlocking" and "USB Debugging"

<div align="center">
  <img src="images/Unlock%20Developer%20Options_2.jpg" alt="Unlock Developer Options_2.jpg" width="30%"/>
  <img src="images/Finding%20Dev%20Options_3.jpg" alt="Finding Dev Options_3.jpg" width="30%"/>
  <img src="images/Allow%20OEM%20and%20USB_4.png" alt="Allow OEM and USB_4.png" width="30%"/>
</div>

---

## Install drivers

Now we need to download `adb` and `fastboot` drivers.


### For Windows
- **adb**: Go to [SDK Platform Tools Downloads](https://developer.android.com/tools/releases/platform-tools) and choose *SDK Platform-Tools for Windows*
  Download it to any directory.
- **Fastboot**: I had some problems with this one, so this [video](https://www.youtube.com/watch?v=snyKAmQfXWw) helped me. Download fastboot drivers from [Google Drive](https://drive.google.com/file/d/1_cIlDdkGtz63kc2VM6oy3vSMszIMtL5g/view), attached in given video desc

### For Linux
```bash
# Update package list
sudo apt update

# Install ADB and Fastboot
sudo apt install android-tools-adb android-tools-fastboot

# Check
adb version
fastboot --version
```
It automatically adds adb to path, so you can call adb commands from any directory.


## Unlock the bootloader

> [!CAUTION]
> **Everything on your phone will erased!**  
> Make sure you backed up important data (numbers, chats, media, files, wifi settings)

If on Windows, go to `platform-tools` directory, where adb drivers are stored and call cmd   
<img src="images/Call%20cmd%20from%20adb%20drivers%20dir.png" alt="Call cmd from adb drivers dir.png" width="60%"/>

Then cmd commands are following:
### 1. Check adb connection

```cmd
adb devices
```

You should see:
```cmd
List of devices attached
<serial number>  device
```

### 2. Reboot to fastboot mode (yes, it is called bootloader)
```cmd
adb reboot bootloader
```

### 3. Check fastboot connection
```cmd
fastboot devices
```

You should see:
```cmd
<serial number>   fastboot
```

### 4. Unlock bootloader
```cmd
fastboot oem unlock
```

or for newer devices
```cmd
fastboot flashing unlock
```

Than you will see the screen on phone that asks you whether unlock or not. Navigate via button `Volume Down` and press `Power` button to choose option 
"`UNLOCK THE BOOTLOADER`". Remember, *everything on phone will be erased!*

Then walk through the OS start screen on phone.

> [!NOTE]
> You can verify that bootloader is unlocked by going in *Delevoper options* once again and see, that button "OEM unlocking" is not available now.  </br></br>
> <img src="images/Check%20bootloader%20unlocked.jpg" alt="Check bootloader unlocked.jpg" width="30%"/>

## Flashing images and installing ROM

At this step we need to boot crucial files:
- `recovery.img`
- `vbmeta.img`
- `dtbo.img`
- `boot.img`
- `EvolutionX-16.0.zip`

> [!WARNING]
> Make sure you download this file **from ROMs official website**, because:  
> `vbmeta.img` - ✅ universal  
> `dtbo.img` - ❌ ROM specific  
> `recovery.img` - ❌ ROM specific  
> `boot.img` - ❌ ROM specific  
> 
> That means, if you downloaded for example `dtbo.img` or `recovery.img` from [LineageOS website](https://download.lineageos.org/devices/instantnoodlep/builds), but will flash `EvolutionX-16.0.zip` ROM, you most likely will get **broken phone** (brick or bugs in system, if it boots)


1. First go to [Evolution X site](https://evolution-x.org/devices/instantnoodlep) 
2. Press `How to install` button 
3. Press all buttons named images/archive names 
4. Put the files in `platform-tools` directory for your convenience (to avoid specifying path to `.img`/`.zip` in cmd)  

<img src="images/Images%20to%20flash.png" alt="Images to flash.png" width="80%"/>  

Make sure you are in `platform-tools` directory in cmd, then switch phone to `fastboot mode` and re-check connection:

```cmd
adb reboot bootloader
fastboot devices
```

### 1. Flashing images (SEQUENCE MATTERS!)
```cmd
fastboot flash vbmeta vbmeta.img 
fastboot flash dtbo dtbo.img
fastboot flash recovery recovery.img
```

### 2. Load in Evolution X recovery
```cmd
fastboot reboot recovery
```

### 3. While in recovery, navigate:
Factory Reset → Format Data/Factory Reset → confirm to format the device.

<div align="center">
  <img src="images/Recovery%20-%20Factory%20reset.jpg" alt="Recovery - Factory reset.jpg" width="65%"/>
  <img src="images/Recovery%20-%20Format%20Data%20and%20Factory%20Reset.jpg" alt="Recovery - Format Data and Factory Reset.jpg" width="65%"/>
</div>

### 4. After data wiped out, choose:
Apply update → Apply from ADB → back to cmd and **flash the ROM itself**:

> [!NOTE]
> Specify `.zip` extension as well

```adb
adb sideload EvolutionX-16.0-20251006-instantnoodlep-11.2.1-Official.zip
```

Then chill for a couple minutes!


> [!TIP]  
> You can face a notification in cmd:  
> ```cmd  
> D:\platform-tools> adb sideload EvolutionX-16.0-20251006-instantnoodlep-11.2.1-Official.zip  
> serving: 'EvolutionX-16.0-20251006-instantnoodlep-11.2.1-Official.zip'  (~47%)    adb: failed to read command: No error  
> ```
> 
> It's OKAY. ADB connection was lost when recovery automatically rebooted. The firmware was fully installed by this point.

After installation is complete, you will be asked in recovery, whether you'd like to install additional packages (they mean root and other). Choose "No".  

Then in recovery press "Reboot system now"  

<div align="center">
  <img src="images/Additional%20packages%20asking.jpg" alt="Additional packages asking.jpg" width="65%"/>
  <img src="images/Install%20completed%20Reboot%20to%20system.jpg" alt="Install completed Reboot to system.jpg" width="65%"/>
</div>  

And enjoy your new ROM!  

<img src="images/Evolution%20X%20Start%20Screen.png" alt="Evolution X Start Screen.png" width="30%"/>      

> [!WARNING]  
> Don't lock the bootloader while using Evolution X, it can **permanently brick your device** and recover it would be hard because the bootloader is locked.  
> It's advised to **keep the bootloader unlocked** to avoid complications.  
