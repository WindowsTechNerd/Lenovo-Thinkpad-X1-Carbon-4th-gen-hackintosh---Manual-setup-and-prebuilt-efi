# Manual Setup - macOS

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

It's now time for our firmware drivers, for most computers (including ours) we only need the two EFI drivers
