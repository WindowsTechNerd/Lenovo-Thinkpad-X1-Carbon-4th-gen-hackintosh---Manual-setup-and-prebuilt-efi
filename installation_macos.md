# Manual Setup - macOS

Sections:
- [Manual Setup - macOS](#manual-setup---macOS)

To start off, run the following command in terminal:

$ softwareupdate --list-full-installers; echo; echo "Please enter version number you wish to download:"; read REPLY; [ -n "$REPLY" ] && softwareupdate --fetch-full-installer --full-installer-version "$REPLY"

It will give you options of macOS versions to download, type the macOS version number you would like to download (eg. 10.15 for macOS catlina) note that the recommend version to download is macOS catilina, mojavie, big sur can have issues and you may bump into problems so I would go with macOS Catilina.

Anyways once it's finished downloading, you will be greeted with the install macOS [Version].app in your applications folder, once it's done you can proceed to "Setting up the usb and installer"

# Setting up the USB and installer:

Now we'll be formatting the USB to prep it for both the macOS installer and the Opencore bootloader.

We will want to format the USB with "Mac OS Extended (Journled)" and the GUID Partion map (Or GPT for short) and click erase

<img width="2074" height="1364" alt="image" src="https://github.com/user-attachments/assets/f9df2e22-c449-462f-9806-5b579047288a" />

This should format the USB and make two partions:

1. The EFI Partiton (Or the ESP Partiton)
2. The MyVolume partiton

If you have these, you are set to go.

Then you can run the following command to setup the MyVolume partiton:

sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

Make sure to replace "/Install\ macOS\ Big\ Sur.app" with your other macOS version (Unless your version is macOS 11 Big Sur)

(sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume for macOS catilina and sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume for Mojavie) 

Note for users on Apple Silicon arm64 chips installing macOS versions older then big sur:

If the createinstallmedia fails with zsh: killed or Killed: 9 then it's most likely an issue with the installer's code signature. To fix this, you can run the following command:

cd /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/
codesign -s - -f --deep /Applications/Install\ macOS\ Big\ Sur.app

You will also need the command line tools for Xcode installed before running the command above:

xcode-select --install

This will also take some time so you may want to take a snack or sit back and relax.
Once you are done, please proceed to Mounting and setting up Opencore ESP

# Mounting and setting up Opencore ESP (Mounting):

First we will need to mount our EFI partiton, even tho we formatted the USB drive, the ESP isn't mounted yet, this is where our friend comes in, MountEFI

To start, ofc you will need to download the repo of MountEFI, you can download it from github at https://github.com/corpnewt/MountEFI by clicking the green "Code" button and then clicking download zip

<img width="488" height="485" alt="Screen Shot 2026-01-01 at 6 54 11 pm" src="https://github.com/user-attachments/assets/b619fbb2-850e-46f4-bdf0-27f1f650f085" />


You must also get python 3 in order for this tool to work.

Or if you want to use git clone, the command is "git clone https://github.com/corpnewt/MountEFI.git"

Once you extract the downloaded zip by opening it, open the "MountEFI.command" file inside the extracted repo.

Select your Opencore ESP number and click enter:

<img width="1394" height="954" alt="image" src="https://github.com/user-attachments/assets/9431bfb8-8ad4-4c76-9582-74c0b75004e1" />

You will see that once we open the EFI partiton, its empty, This is where the fun begins🥳🤓

<img width="1764" height="1096" alt="image" src="https://github.com/user-attachments/assets/d5846fc4-f759-4fa0-af4c-3c4ab78f65e5" />

# Mounting and setting up Opencore ESP (Setting up):

First, we need the OpenCorePKG, you can download it from the github releases page at https://github.com/acidanthera/OpenCorePkg/releases/

I would recommend using the DEBUG release for as there are more debugging options that you may need incase something doesn't work, becuase I am not solving a billon problems at once, it's not easy typing this day and night, If you are reading this, thanks for your support:)

Anyways, once your zip has downloaded of the OpenCorePKG, in the root of the folder you extract by opening it's zip, you should find two folders in the root of it:

1. IA32 (32-bit/x86)
2. X64 (64-bit/x64)

<img width="2064" height="1208" alt="image" src="https://github.com/user-attachments/assets/1637f329-b634-4c66-b70e-e40e2f62f5f9" />

We are going with the "x64" folder becuase the Lenovo Thinkpad X1 Carbon 4th gen has a Intel CORE i7 vPro skylake gen cpu is 64-bit/x64.

Inside the x64 folder, there should be an folder called "EFI"

Place that folder inside the root of the ESP on your usb that we mounted eirlyier, this should be the layout of your efi folder: 

<img width="2064" height="1994" alt="image" src="https://github.com/user-attachments/assets/94f40f70-3c0e-4f8b-b2c4-760f6639abb8" />

Now go into /OC/Drivers in our EFI folder and delete all the files expect these ones (if needed)

<img width="626" height="384" alt="Screen Shot 2026-01-01 at 9 23 32 pm" src="https://github.com/user-attachments/assets/c7ac1466-1835-4f18-9a9f-dfff3624fd58" />

And now keep the following from /OC/Toolsif if you want (highly recommend):

<img width="520" height="89" alt="Screen Shot 2026-01-01 at 9 28 00 pm" src="https://github.com/user-attachments/assets/49551d13-d610-4891-8327-493a10dd2872" />

Now here's how much cleaner our EFI should look (This is the 100th line lol):

<img width="2102" height="1438" alt="image" src="https://github.com/user-attachments/assets/65b8f8be-c7c6-49e3-9493-80cb39f00ce3" />

Now we can place our firmware drivers for this big boy (I call it the big mac)

See Gathering Files for our kexts.

# Gathering Files

It's now time for our firmware drivers, for most computers (including ours) we only need the two EFI drivers:

1. HfsPlus.efi (Get at https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi)
2. OpenRuntime.efi (Get at https://github.com/acidanthera/OpenCorePkg/releases)

Both these firmware drivers are required.

Please follow the steps below for our big mac's kexts.

# Kexts (Required)

For all systems and computers (including ours ofc) we need the two kexts below in order for any system from the opencore boot picker to be booted, these kexts, along with other ones, have to go in the /OC/Kexts folder in your EFI folder.

1. Lilu.kext (Get at https://github.com/acidanthera/Lilu/releases, a kext to patch processes from other kexts)
2. VirtualSMC (Get at https://github.com/acidanthera/VirtualSMC/releases, Emulates the SMC chip found on real macs, without this kext, only macOS isn't bootable)

We will also need the following kexts below that are required for our computer to work with VirtualSMC:

1. SMCProcessor.kext (Used for monitoring Intel CPU Temp)
2. SMCSuperIO.kext (Used for monitoring Intel CPU fan speeds)
3. SMCLightSensor.kext (Used for macOS to function with the ambient light sensor on the big mac, All thinkpads have this.)
4. SMCBatteryManager.kext (Used for measuring battery readouts on laptops like ours.)

You should find these kexts in the VirtualSMC

We will also need the following required kexts for graphics:

1. WhateverGreen (Get at https://github.com/acidanthera/WhateverGreen/releases, required for graphics, macOS won't boot without this.)

# Kexts (Audio)

We will need the following kexts for Audio:

1. AppleALC (Get at https://github.com/acidanthera/AppleALC/releases)

# Kexts (Ethernet)

Our system uses an Intel NIC chipset and it is not based off 1211 so we will need the followng kext:

1. IntelMasusi (Get at https://github.com/acidanthera/IntelMausi/releases)

# Kexts (USB)

Unlike the other kext's, this setup requires some more manual stuff for as we need to make a UTBMap.kext for our big mac

Start by download the following stuff below

1. USBToolBox (Tool, Get at https://github.com/USBToolBox/tool, this tool is for making the UTBMap.kext file for our system, this kext we are going to make will not work on other machines.)
2. USBToolBox (Kext, Get at https://github.com/USBToolBox/kext/releases)

Make sure to download these files on the big mac running Windows for as we need to make the UTBMap.kext file for it

Now open the file you downloaded from github.com/USBToolBox/tool, it should be "Windows.exe" open it (You need python on the big mac for this)

You should see this

<img width="333" height="151" alt="image" src="https://github.com/user-attachments/assets/959122bd-98c3-493c-a4ab-cd752f0b49e5" />

Type D and click enter for it to find your ports, make sure to fill in all your usb ports before typing D and click enter

After waiting five seconds, type B and click enter
Then type S and click enter
And then type K and click enter, then your UTBMap.kext file should be ready
Please the UTBMap.kext file in the kexts folder on your usb in /OC/Kexts and also place the USBToolBox.kext file from the USBToolBox (Kext) zip that you downloaded in there

# Kexts (Wi-Fi)

It's now time for the thing that every computer has to have these days, and probely where you are reading this guide from, that's right, the wi-fi!

For our intel network card, we can only pick from two kexts:

1. AirportItlwm.kext (This kext grants us native intel network card support with the native macOS wifi menu)
2. Itlwm.kext (This kext is a more stable version of AirportItlwm.kext that supports macOS 11 and before)

We will be going to use Itlwm, it should be in the AirportItlwm.kext releases page at https://github.com/OpenIntelWireless/itlwm/releases, please look at the video below if you don't see itlwm.kext on the releases page:

https://github.com/user-attachments/assets/fa64f581-040b-434c-81e3-5e7549c131ee

Anyways, ofc we have to place the kext in /OC/Kexts

# Kexts (Bluetooth)

I will write this an other time...
