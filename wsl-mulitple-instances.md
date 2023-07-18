# Backup/Restore, and Multiple Instances of the Same Distros

Follow the backup and restore process described here: [Backup/Restore](https://sungkim11.medium.com/why-you-should-use-multiple-instances-of-same-linux-distro-on-wsl-windows-10-f6f140f8ed88)

## Terminal Setup

Terminal will not update correctly - and will need the Json file to be edited.

In the example below a Ubuntu-22.04.1.v2 distro is opened using the wsl -d command, change this distro name and the other relevant values to whichever distro you wish to load.

```json
{
    "colorScheme": "Tango Dark",
    "commandline": "wsl -d Ubuntu-22.04.1.v2",
    "cursorShape": "filledBox",
    "font": 
    {
        "face": "Cascadia Mono"
    },
    "guid": "{9a1e97cc-cd2c-5f9f-9f89-7487a8a1177a}",
    "hidden": false,
    "icon": "https://assets.ubuntu.com/v1/49a1a858-favicon-32x32.png",
    "name": "Ubuntu-22.04.1 LTS v2",
    "tabTitle": "Ubuntu 22.04.1 LTS v2"
},
```

To confirm that this is working correctly:

```PowerShell
wsl --shutdown
wsl -l -v # All should be shown as STATE Stopped.
# Now open the distro via terminal
wsl -l -v # The distro you want to run should be shown as STATE Running.
```

## User Setup

***Note:** As the wsl - d command has been used, the root user will be used instead of the user that was setup. This is an oddity of WSL.*

Open the distro.

```sh
sudo nano /etc/wsl.conf
```

Add the following to the file:

>[user]
>default=<the same user you have in the distro backed up>
>Exit and Save (Ctrl+X then yes)

```Powershell
wsl --shutdown
```

Reopen the distro