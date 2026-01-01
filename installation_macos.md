# Manual Setup - macOS

To start off, run the following command in terminal:

$ softwareupdate --list-full-installers; echo; echo "Please enter version number you wish to download:"; read REPLY; [ -n "$REPLY" ] && softwareupdate --fetch-full-installer --full-installer-version "$REPLY"

It will give you options of macOS versions to download, type the macOS version number you would like to download (eg. 10.15 for macOS catlina) note that the recommend version to download is macOS catilina, mojavie, big sur can have issues and you may bump into problems so I would go with macOS Catilina.

Anyways once it's finished downloading, you will be greeted with the install macOS [Version].app in your applications folder, once it's done you can proceed to "Setting up the usb and installer"

Setting up the usb and installer:

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

Mounting and setting up Opencore ESP:

First we will need to mount our EFI partiton, even tho we formatted the USB drive, the ESP isn't mounted yet, this is where our friend comes in, MountEFI

To start, ofc you will need to download the repo of MountEFI, you can download it from github at https://github.com/corpnewt/MountEFI by clicking the green "Code" button and then clicking download zip

You must also get python 3 in order for this tool to work.

Or if you want to use git clone, the command is "git clone https://github.com/corpnewt/MountEFI.git"

Once you extract the downloaded zip by opening it, open the "MountEFI.command" file inside the extracted repo.

Select your Opencore ESP number and click enter:

<img width="1394" height="954" alt="image" src="https://github.com/user-attachments/assets/9431bfb8-8ad4-4c76-9582-74c0b75004e1" />

You will see that once we open the EFI partiton, its empty, This is where the fun begins🥳🤓
