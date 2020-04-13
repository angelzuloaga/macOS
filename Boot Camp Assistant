# Boot Camp Assistant - Enabling creation of bootable USB disks for installing Windows

## Contents

- [Applicability](#applicability)
- [Introduction](#introduction)
- [Procedure](#procedure)
  - [1. Obtain permission to edit Boot Camp Assistant's Property List file.](#1-obtain-permission-to-edit-boot-camp-assistants-property-list-file)
    - [Option A: Duplicate the Boot Camp Assistant application package.](#option-a-duplicate-the-boot-camp-assistant-application-package)
    - [Option B: Disable System Integrity Protection (SIP).](#option-b-disable-system-integrity-protection-sip)
  - [2. Obtain your Mac's "Model Identifier" and "Boot ROM Version".](#2-obtain-your-macs-model-identifier-and-boot-rom-version)
  - [3. Edit Boot Camp Assistant's Property List file.](#3-edit-boot-camp-assistants-property-list-file)
    - [Enable earlier macOS versions](#enable-earlier-macos-versions)
      - [`LSMinimumSystemVersion`](#lsminimumsystemversion)
    - [Enable USB option](#enable-usb-option)
      - [`PreUSBBootSupportedModels`](#preusbbootsupportedmodels)
    - [Enable WIM support](#enable-wim-support)
      - [`PreESDRequiredModels`](#preesdrequiredmodels)
    - [Enable 32-bit support](#enable-32-bit-support)
      - [`32BitSupportedModels`](#32bitsupportedmodels)
    - [Enable 32-bit Windows 10, and Windows versions prior to Windows 10](#enable-32-bit-windows-10-and-windows-versions-prior-to-windows-10)
      - [`PreWindows10OnlyModels`](#prewindows10onlymodels)
    - [Enable Windows versions after Windows 7](#enable-windows-versions-after-windows-7)
      - [`Win7OnlyModels`](#win7onlymodels)
    - [Enable 64-bit Windows 10](#enable-64-bit-windows-10)
      - [`SupporedNonWin10Models` (*sic*)](#supporednonwin10models-sic)
    - [Enable BIOS/UEFI firmware modes](#enable-biosuefi-firmware-modes)
      - [`PreUEFIModels`](#preuefimodels)
      - [`UEFIOnlyModels`](#uefionlymodels)
    - [Enable internal installation](#enable-internal-installation)
      - [`ExternalInstallOnlyModels`](#externalinstallonlymodels)
    - [Enable boot ROM support](#enable-boot-rom-support)
      - [`DARequiredROMVersions`](#darequiredromversions)
  - [4. OPTIONAL: Restore original permissions (recommended).](#4-optional-restore-original-permissions-recommended)
  - [5. Code-sign the edited application package (if necessary).](#5-code-sign-the-edited-application-package-if-necessary)
  - [6. OPTIONAL: Re-enable System Integrity Protection (recommended).](#6-optional-re-enable-system-integrity-protection-recommended)
- [Conclusion](#conclusion)

## Applicability

This information applies specifically to the following system, but may also apply to other hardware/software versions:
- Hardware: [MacBook Pro (17-inch, Mid 2010)](https://support.apple.com/kb/SP581)
- Operating System: [macOS High Sierra](https://en.wikipedia.org/wiki/MacOS_High_Sierra) 10.13.6
- Application: Boot Camp Assistant, version 6.1.0 (6067.60.1)

## Introduction

In [macOS](https://en.wikipedia.org/wiki/MacOS), **Boot Camp Assistant** (*[Finder](https://en.wikipedia.org/wiki/Finder_(software))* → *Applications* → *Utilities* → *Boot Camp Assistant*) has the ability to create [bootable](https://en.wikipedia.org/wiki/Bootstrapping) [USB disks](https://en.wikipedia.org/wiki/USB_flash_drive) for installing [Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) in [Boot Camp](https://en.wikipedia.org/wiki/Boot_Camp_(software)) on a [Mac](https://en.wikipedia.org/wiki/Macintosh) computer.  However, some Mac computers don't support booting from external USB devices, or don't support certain versions of Windows in Boot Camp, etc.; in these cases, the option to create bootable USB disks for installing Windows has been suppressed within **Boot Camp Assistant**.

![Boot Camp Assistant](https://user-images.githubusercontent.com/30203863/52740824-8a66b780-2f99-11e9-833b-34e79faf9a72.png)

But you should still be able to use **Boot Camp Assistant** to create bootable USB disks for installing Windows on *other* computers, right?

To do so, you need to tell **Boot Camp Assistant** that your Mac supports the necessary options and Windows versions, by editing the `Info.plist` [property list](https://en.wikipedia.org/wiki/Property_list) file within its application package.

## Procedure

### 1. Obtain [permission](https://en.wikipedia.org/wiki/File_system_permissions) to edit Boot Camp Assistant's Property List file.

First, make sure it's OK with your mom.

Then, because **Boot Camp Assistant** is part of the operating system, in newer versions of macOS it is protected from editing, so you need to work around this limitation.

#### Option A: Duplicate the **Boot Camp Assistant** application package.

Copy the `/Applications/Utilities/Boot Camp Assistant.app` application package to another location, such as your `~/Desktop/`.  You will then be free to edit the `Info.plist` file within the copied application package.

#### Option B: Disable [System Integrity Protection](https://en.wikipedia.org/wiki/System_Integrity_Protection) (SIP).

Boot into "Recovery Mode" by restarting your Mac, then while it's rebooting, before the "gong" startup sound, press and hold <kbd>⌘ Command</kbd> + <kbd>R</kbd> until after the Apple logo and progress bar appear.

In Recovery Mode, open the **[Terminal](https://en.wikipedia.org/wiki/Terminal_(macOS))** application (*Utilities* menu → *Terminal*), then disable SIP by entering the following command:

```bash
csrutil disable
```

Then restart for the change to take effect.

Finally, if necessary, modify the permissions so that you can edit the `Info.plist` file.  In *Finder* → *Applications* → *Utilities* → *Terminal*, enter the following commands as necessary to add "write" permissions for "group" and "other" users:

```bash
sudo chmod go+w /path/to/your/Boot\ Camp\ Assistant.app/Contents
sudo chmod go+w /path/to/your/Boot\ Camp\ Assistant.app/Contents/Info.plist
```

> Note: Replace `/path/to/your/` with the actual [path](https://en.wikipedia.org/wiki/Path_(computing)) to your `Boot Camp Assistant.app` application package.  For example:
> - If you copied **Boot Camp Assistant** to your Desktop:
>
> ```bash
> sudo chmod go+w ~/Desktop/Boot\ Camp\ Assistant.app/Contents
> sudo chmod go+w ~/Desktop/Boot\ Camp\ Assistant.app/Contents/Info.plist
> ```
>
> - If you are modifying the original **Boot Camp Assistant** application package:
>
> ```bash
> sudo chmod go+w /Applications/Utilities/Boot\ Camp\ Assistant.app/Contents
> sudo chmod go+w /Applications/Utilities/Boot\ Camp\ Assistant.app/Contents/Info.plist
> ```

Or, right-click on the `Boot Camp Assistant.app` file → *Show Package Contents*, then right-click on the `Contents` folder → *Get Info*.

![Contents Info](https://user-images.githubusercontent.com/30203863/52748091-31078400-2fab-11e9-965f-0b315e02311f.png)

Click to unlock the icon in the bottom right corner (and enter your password if prompted), then set all privileges to "Read & Write".

Open the `Contents` folder, then repeat for the `Info.plist` file within.

### 2. Obtain your Mac's ["Model Identifier" and "Boot ROM Version"](https://support.apple.com/en-us/HT201518).

*Apple*  menu → *About This Mac* → *Overview* tab → *System Report...* button → *Hardware*:

![Hardware Overview](https://user-images.githubusercontent.com/30203863/52749598-47174380-2faf-11e9-93da-784062600196.png)

Make a note of your "Model Identifier" and "Boot ROM Version".

### 3. Edit Boot Camp Assistant's Property List file.

Use a suitable [code editor](https://en.wikipedia.org/wiki/Source_code_editor) to open the `/path/to/your/Boot Camp Assistant.app/Contents/Info.plist` file, and add/remove/change the Model Identifier or Boot ROM Version within the various dictionary keys as follows.  You might need to experiment with different combinations to find a configuration that works for you.  (Explanations are just guesses, as I wasn't able to find any documentation online.  Also refer to the included [Info.plist](./3c5b02c6d664b7fb157b44b1fc168799#file-info-plist) file for examples.)

---

#### Enable earlier macOS versions

This option enables this version of **Boot Camp Assistant** to run in earlier versions of macOS.

##### [`LSMinimumSystemVersion`](https://developer.apple.com/documentation/bundleresources/information_property_list/lsminimumsystemversion)

- **CHANGE** the version number to be *less than or equal to your operating system version*. 

```xml
	<key>LSMinimumSystemVersion</key>
	<string>10.0.0</string>
```

---

#### Enable USB option

This enables the option to "Create a Windows 7 (or Windows 8) version install disk" onto a USB drive.

##### `PreUSBBootSupportedModels`

- **REMOVE** the value for your **Model Identifier**.

```xml
	<key>PreUSBBootSupportedModels</key>
	<array/>
```

---

#### Enable WIM support

This should enable support for Windows installation package formats prior to ESD ([Electronic Software Distribution](https://en.wikipedia.org/wiki/Digital_distribution)), such as WIM ([Windows Imaging Format](https://en.wikipedia.org/wiki/Windows_Imaging_Format)).

- Note: This option is unconfirmed - for me, the WIM format appeared to be supported regardless of the settings here.

- Note: This also seems to enable the option to "Download the latest Windows support software from Apple".

##### `PreESDRequiredModels`

- **CHANGE** the value for your respective model to any valid Intel-based **Model Identifier (*omitting the comma and subsequent digits*)** that is *greater than or equal to your respective model's identifier*.

```xml
	<key>PreESDRequiredModels</key>
	<array>
		<string>MacBook10</string>
		<string>MacBookAir8</string>
		<string>MacBookPro15</string>
		<string>MacPro6</string>
		<string>Macmini8</string>
		<string>iMac18</string>
		<string>iMacPro1</string>
		<string>Xserve3</string>
		<string>Parallels14</string>
	</array>
```

---

#### Enable 32-bit support

This enables installation of 32-bit versions of Windows.

- Note: This option is unconfirmed - for me, the resulting installers appeared to support 32-bit Windows regardless of the settings here (notwithstanding the specific Windows versions enabled/disabled by the options below).

##### `32BitSupportedModels`

- **CHANGE** the value for your respective model to any valid Intel-based **Model Identifier** that is *greater than your respective model's identifier*.

```xml
	<key>32BitSupportedModels</key>
	<array>
		<string>MacBook10,1</string>
		<string>MacBookAir8,1</string>
		<string>MacBookPro15,2</string>
		<string>MacPro6,1</string>
		<string>Macmini8,1</string>
		<string>iMac18,3</string>
		<string>iMacPro1,1</string>
		<string>Xserve3,1</string>
		<string>Parallels14,1</string>
	</array>
```

---

#### Enable 32-bit Windows 10, and Windows versions prior to Windows 10

This enables installation of 32-bit Windows 10, and other Windows versions prior to [Windows 10](https://en.wikipedia.org/wiki/Windows_10).

- Related error message:

> **Need 64-bit Windows 10 or later ISO file.**
>
> Boot Camp only supports 64-bit Windows 10 or later installation on this platform.  Please use an ISO file for 64-bit Windows 10 or later installation.

##### `PreWindows10OnlyModels`

- **CHANGE** the value for your respective model to any valid Intel-based **Model Identifier** that is *greater than or equal to your respective model's identifier*.

```xml
	<key>PreWindows10OnlyModels</key>
	<array>
		<string>MacBook10,1</string>
		<string>MacBookAir8,1</string>
		<string>MacBookPro15,2</string>
		<string>MacPro6,1</string>
		<string>Macmini8,1</string>
		<string>iMac18,3</string>
		<string>iMacPro1,1</string>
		<string>Xserve3,1</string>
		<string>Parallels14,1</string>
	</array>
```

---

#### Enable Windows versions after Windows 7

This enables installation of Windows versions after [Windows 7](https://en.wikipedia.org/wiki/Windows_7).

- Related error message:

> **Windows 8 is not supported on this Mac**
>
> Boot Camp only supports installing Windows 7 on this Mac.  Please insert a USB drive or DVD which contains a full version of Windows 7.

##### `Win7OnlyModels`

- **REMOVE** the value for your **Model Identifier**.

```xml
	<key>Win7OnlyModels</key>
	<array/>
```

---

#### Enable 64-bit Windows 10

This enables installation of 64-bit Windows 10.

- Related error message:

> **Windows 10 is not supported on this Mac.**
>
> Boot Camp only supports 64-bit Windows 8 or Windows 7 installation on this platform.  Please use an ISO file for 64-bit Windows 8 or Windows 7 installation.

##### `SupporedNonWin10Models` (*sic*)

- **REMOVE** the value for your **Model Identifier**.  (Notice that the word "Suppored" is missing the letter "t".)

```xml
	<key>SupporedNonWin10Models</key>
	<array/>
```

---

#### Enable BIOS/UEFI firmware modes

This option enables booting to BIOS ([Basic Input/Output System](https://en.wikipedia.org/wiki/BIOS)) and/or UEFI ([Unified Extensible Firmware Interface](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface)) firmware modes.

- Note: This option is unconfirmed - for me, the resulting installers appeared to support both BIOS and UEFI regardless of the settings here.

##### `PreUEFIModels`

- **CHANGE** the value for your respective model to any valid Intel-based **Model Identifier (*omitting the comma and subsequent digits*)** that is *greater than or equal to your respective model's identifier*.

```xml
	<key>PreUEFIModels</key>
	<array>
		<string>MacBook10</string>
		<string>MacBookAir8</string>
		<string>MacBookPro15</string>
		<string>MacPro6</string>
		<string>Macmini8</string>
		<string>iMac18</string>
		<string>iMacPro1</string>
		<string>Xserve3</string>
		<string>Parallels14</string>
	</array>
```

##### `UEFIOnlyModels`

- **REMOVE** the value for your **Model Identifier**.

```xml
	<key>UEFIOnlyModels</key>
	<array/>
```

---

#### Enable internal installation

This option enables installation to internal hard drives.  Probably irrelevant if you are trying to create an external USB installation disk.

- Note: This option is unconfirmed - I have never attempted to install Windows onto an internal drive in a Mac.

##### `ExternalInstallOnlyModels`

- **REMOVE** the value for your **Model Identifier**

```xml
	<key>ExternalInstallOnlyModels</key>
	<array/>
```

---

#### Enable boot ROM support

This option enables support for your boot ROM.

- Note: This option is unconfirmed - it had no effect for me, but others have suggested that it is required.

##### `DARequiredROMVersions`

- **ADD** this key if it is missing, and **ADD** your **Boot ROM Version** as a new `<string>` value within the key's `<array>` value.

```xml
  <key>DARequiredROMVersions</key>
  <array>
    <string>96.0.0.0.0</string>
  </array>
```

---

### 4. OPTIONAL: Restore original permissions (recommended).

For security reasons, it is recommended to restore the original permissions within the **Boot Camp Assistant** application package.  In *Finder* → *Applications* → *Utilities* → *Terminal*, enter the following commands as necessary to remove "write" permissions for "group" and "other" users:

```bash
sudo chmod go-w /path/to/your/Boot\ Camp\ Assistant.app/Contents/Info.plist
sudo chmod go-w /path/to/your/Boot\ Camp\ Assistant.app/Contents
```

Or, right-click on the `Boot Camp Assistant.app` file → *Show Package Contents*, then within the `Contents` folder, right-click on the `Info.plist` file → *Get Info*.  Then click to unlock the icon in the bottom right corner (and enter your password if prompted), then set the privileges for everything other than "system" to "Read only".  Repeat for the `Contents` folder itself.

### 5. Code-sign the edited application package (if necessary).

Some versions of macOS will require the **Boot Camp Assistant** application package to be [code-signed](https://en.wikipedia.org/wiki/Code_signing) in order to run it.  In *Finder* → *Applications* → *Utilities* → *Terminal*, enter one of the following commands to sign it *ad-hoc* (without an identity):

```bash
sudo codesign --force --sign - /path/to/your/Boot\ Camp\ Assistant.app
```

- If that doesn't work, then for later versions of macOS, nested content must be signed as well, by specifying the `--deep` option:

```bash
sudo codesign --force --sign - /path/to/your/Boot\ Camp\ Assistant.app --deep
```

> NOTE: If [Xcode](https://en.wikipedia.org/wiki/Xcode) Command Line Tools are not already installed, the above commands may prompt to do so; afterward, the above commands may need to be run again.

### 6. OPTIONAL: Re-enable System Integrity Protection (recommended).

For security reasons, it is recommended to re-enable System Integrity Protection (SIP).  Boot into "Recovery Mode" by restarting your Mac, then while it's rebooting, before the "gong" startup sound, press and hold <kbd>⌘ Command</kbd> + <kbd>R</kbd> until after the Apple logo and progress bar appear.  In Recovery Mode, open the **Terminal** application (*Utilities* menu → *Terminal*), then enable SIP by entering the following command:

```bash
csrutil enable
```

Then restart for the change to take effect.

## Conclusion

If everything worked, in **Boot Camp Assistant** you should now have the option to "Create a Windows 7 or Windows 8 version install disk" (which should also work for Windows 10 and later versions, depending on the chosen configuration for your `Info.plist` file).

![Boot Camp Assistant_- Create Install Disk](https://user-images.githubusercontent.com/30203863/52756987-d11fd600-2fc8-11e9-88f2-518dc1f64b9d.png)
