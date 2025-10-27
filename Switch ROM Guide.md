I will use:
- **Phone**: OnePlus 8 Pro (instantnoodlep)
- **ROMs to switch**: From `Evolution X 11.3` to `LineageOS 23.0 nightly` (both Android 16)

## 📋 Preparation

### What to download:

1. **New ROM** 
   - LineageOS: [https://download.lineageos.org/devices/instantnoodlep](https://download.lineageos.org/devices/instantnoodlep/builds)
   - Evolution X: [https://evolution-x.org/device/instantnoodlep](https://evolution-x.org/devices/instantnoodlep)
   - Other ROMs: check their official sites

2. **GApps** (optional)
   - MindTheGapps: [https://wiki.lineageos.org/gapps](https://github.com/MindTheGapps/16.0.0-arm64/releases/tag/MindTheGapps-16.0.0-arm64-20250812_214353)
   - Choose version for ROM's Android version.

3. **Magisk** (optional, if root needed)
   - GitHub: [https://github.com/topjohnwu/Magisk/releases](https://github.com/topjohnwu/Magisk/releases)
   - Download APK → rename to `.zip`


> [!WARNING]
> Highly recommend Magisk v29.0. I used to flash Magisk v30.3 for Evolution X 11.2 (Android 16) and got corrupted system and **needed to wipe system completely**


### Backup:
```bash
# If already root - use Neo Backup
Neo Backup → Select All → Backup

# Copy to PC directory 
adb pull /sdcard/NeoBackup/ C:\Backup\

# Don't forget about contacts, SMS, photo etc.
```

## 🚀 Installation process

### Connect phone to PC

Check ADB connection

```cmd
adb devices
```

You should see:

```cmd
List of devices attached
<serial number> 	device
```

## Step 1: Reboot in Recovery

```cmd
adb reboot recovery
```

## Step 2: Wipe data

To flash new ROM we need to erase previous ROM. It can be done in recovery (mine is Evolution X):

`Factory reset` → `Format data/factory reset` → `Format data`

<img src="images/Recovery%20-%20Format%20data%20completely.jpg" alt="Recovery - Format data completely.jpg" width="30%"/>

## Step 3: Apply from ADB

In recovery go:

`Apply update` → `Apply from ADB` → recovery is waiting for sideloading `.zip` from PC  

<div align="center">
  <img src="images/Recovery%20-%20Apply%20Update.jpg" alt="Recovery - Apply Update.jpg" width="30%"/>
  <img src="images/Recovery%20-%20Apply%20from%20ADB.jpg" alt="Recovery - Apply from ADB.jpg" width="30%"/>
  <img src="images/Recovery%20-%20Waiting%20for%20adb%20sideload.jpg" alt="Recovery - Waiting for adb sideload.jpg" width="30%"/>
</div>

## Step 4: Sideload LineageOS

Flash new ROM:
```cmd
adb sideload lineage-23.0-20251024-nightly-instantnoodlep-signed.zip
```

> [!IMPORTANT]
> Sequence matters! ROM always needs to be flashed first

<img src="images/Sideload%20-%20Installing%20ROM.jpg" alt="Sideload - Installing ROM.jpg" width="30%"/>

If you plan install GApps and Magisk, choose `Yes` here:

<img src="images/Recovery%20-%20Ask%20for%20Additional%20packages.jpg" alt="Recovery - Ask for Additional packages.jpg" width="30%"/>

This will reboot you to new LineageOS recovery.

> [!IMPORTANT]
> **Why is it not necessary to manually flash boot, dtbo, recovery?**
> 
> ROM ZIP file (for example, `lineage-23.0.zip`) has `payload.bin` file inside with all working images:
> - `boot.img` (kernel + ramdisk)
> - `dtbo.img` (device tree)
> - `recovery.img` (recovery for new ROM)
> - `vbmeta.img` (verified boot metadata)
> - `system.img`, `vendor.img`, `product.img` other
> 
> When executing `adb sideload lineage-22.zip`, recovery automatically:
> 1. Unpacks `payload.bin`
> 2. Flashes **all** images into the correct sections
> 3. Configures the system
> 
> **Therefore, although these images are ROM specific (except `vbmeta.img`), there is no need to manually flash them via fastboot!** Everything happens automatically when sideloading the ZIP file.


### Step 5: Sideload GApps (optional)

In recovery:

`Apply update` → `Apply from ADB` → recovery is waiting for sideloading `.zip` from PC

```bash
adb sideload MindTheGapps-16.0.0-arm64-20250812_214353.zip
```

> [!IMPORTANT]
> While sideload, you may get the following notification:  
> 
> <img src="images/Signature%20verification%20error.jpg" alt="Signature verification error.jpg" width="30%"/>
> 
> Press `Yes` here


Wait 2-3 minutes → `Installation complete!`  

<img src="images/LineageOS%20recovery%20-%20Install%20GApps.jpg" alt="LineageOS recovery - Install GApps.jpg" width="30%"/>



### Step 6: Sideload Magisk (optional)

As in previous steps:
`Apply update` → `Apply from ADB` → recovery is waiting for sideloading `.zip` from PC

```bash
adb sideload Magisk-v29.0.zip
```

> [!NOTE]
> If you got `Signature verification failed` again, press `Yes` again

Wait < 1 minute → `Magisk v29.0` installed!

<img src="images/LineageOS%20recovery%20-%20Install%20Magisk.jpg" alt="LineageOS recovery - Install Magisk.jpg" width="30%"/>


> [!NOTE]
> Magisk may ask for [additional setting](link-to-magisk-guide.md) after first launch.

### Step 7: Reboot to system

In recovery press `Reboot system now`

Got a new ROM!

<img src="images/LineageOS%20Start%20screen.jpg" alt="LineageOS%20Start%20screen.jpg" width="30%"/>





