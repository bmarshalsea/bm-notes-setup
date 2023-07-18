# WSL Setup

## Windows Pre-Requisites

### Chocolatey Installable

Install [Chocolatey] (<https://chocolatey.org/install>).
This enables Windows users to be able manage software installed on Windows in via scripting, ensuring it is setup in the same way for each person.
To install this, use the following command in PowerShell:

```PowerShell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

## WSL

Be aware of this MS Setup Guide to WSL for development: [Windows Subsystem for Linux Documentation](https://learn.microsoft.com/en-us/windows/wsl/)
It is also important to be aware of WSLg: [GitHub - microsoft/wslg: Enabling the Windows Subsystem for Linux to include support for Wayland and X server related scenarios](https://github.com/microsoft/wslg)

Note: The following commands can often fix issues encountered while installing: 

```sh
sudo apt-get purge <packageName>* # Note: The * at the end of the first line is required. 
sudo apt-get autoremove
sudo apt-get autoclean
sudo apt-get dist-upgrade
sudo apt-get update
```

### WSL2 Pre-requisites

Open System Information and confirm that you are either:
> Windows 11 Pro
> Windows 10 Pro (minimum of Build 19044)

**Note:** *Home cannot support WSL - will need to upgrade the Windows install to Pro.*

Ensure Windows is up to date:
> Run all Windows Updates and Reboot

#### WSL2 and Ubuntu 22.04lts:

PowerShell can now install everything in one command. Open Terminal with a Powershell in Admin Mode.

```PowerShell
wsl --install
wsl --set-default-version 2
wsl -l -v # Confirm Ubuntu-22.04 is installed
# wsl --install -d Ubuntu-22.04 - Only required if Ubuntu-22.04 has not been installed. 
wsl --set-default Ubuntu-22.04
wsl # Start the linux terminal
```

It will ask you for a username and password: REMEMBER THESE OR YOU WILL HAVE TO DELETE AND START OVER.

On exit and closure of Terminal it should show up in the Terminal List.
If not you’ll have to add it through the Add new profile butotn.

##### Manual install

>Search for "Features" and select "Turn Windows features on or off"
>Check/Ensure the following are fully selected:
>[x] Containers
>[x] Hyper-V
>[x] Virtual Machine Platform
>[x] Windows Hypervisor Platform
>[x] Windows Subsystem for Linux
>
>Reboot

#### Amazon 2

```PowerShell
Invoke-WebRequest -Uri https://github.com/yosukes-dev/AmazonWSL/releases/download/2.0.20200722.0-update.2/Amazon2.zip -OutFile $env:TMP\Amazon2.zip
Expand-Archive -Path $env:TMP\Amazon2.zip -DestinationPath C:\WSL\Amazon2
cd C:\WSL\Amazon2\Amazon2.exe
wsl --set-version Amazon2 2 # Ensure Amazon2 is in wsl2
wsl -d Amazon2 # Run the Amazon2 distro
```

## CONFIGURATION

> Remember your drives are at /mnt/{driveletter}

### First Time Configuration

Open distro, it will ask for a username and password for the user.

*Note: Make the Username anything you want. Make sure the password is something you remember, as it's used whenever you sudo/root something - and if you don't you'll have to go through all of this again.*

Update/Upgrade everything to the latest: 

```sh
sudo apt update -y && sudo apt upgrade -y
```

Setup the drive permissions:

```sh
sudo nano /etc/wsl.conf
```

Add the following to the file:

>[automount]
>options = "metadata"
>Exit and Save (Ctrl+X then yes)

Restart Ubuntu (Via PowerShell)

```PowerShell
wsl --shutdown
```

## Where’s my Windows Files? (Accessing Windows files from Linux)

In Ubuntu **cd /mnt** here you will find the various drives mapped.

## Where’s my WSL Dev Files? (Accessing files within Windows)

In Windows 11 and newer Windows 10 distro's in Explorer there will be a Linux folder (at root, Penguin folder).

In Explorer **\\wsl$** which is part of the network section.

***Note:** This will be slow and is not the correct answer most of the time, but may bridge a gap.*

### Visual Studio

Open Visual Studio
Click Open Folder…
Follow Common WSL Folder Path below
When it loads it will prompt about “WSL Windows” > click yes
This then reopen and bottom left will show: ![Image](vs-wsl.png)

### Common WSL Folder Path

In the address bar type: \\wsl$
Click on the folder for the Distro you are using
Goto: *home/&lt;your wsl username&gt;/_dev*
Goto the folder of the Repo you wish to manage
Click Select Folder

This will initially take some time to map the files across

## Why’s it taking so long to compile?

Cross OS (Linux > Windows or Windows > Linux) is not optimal and takes considerable time, going through a fair few Virtual Network Loops. You need to git clone the repo into the WSL folder structure.

### Open Terminal/Linux

```sh
cd ~ # Change to home directory
mkdir _dev #Make a new dev ( _dev ) directory:
cd _dev
```

Once in this folder, use *git clone* to download the repositories required.

***Note:** On the first request it will ask about RSA Fingerprinting - say **yes***
