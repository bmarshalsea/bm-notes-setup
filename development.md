# Development Setup

The following setups a basic C#, NodeJS/TypeScript development environment.

## Development Applications

Use Chocolatey to install the following recommended applications up the environment:

```PowerShell
choco install microsoft-windows-terminal -y
choco install git # git bash, always useful to have
choco install vscode -y
choco install adb
choco install gitkraken # Recommended GUI, best available
refreshEnv
```

## Terminal

Update the settings of Terminal
Within the Startup tab:
> Default Profile: Windows PowerShell
> When Terminal starts: Open windows from a previous session

Within the Profile/PowerShell tab:
> Run this profile as Administrator: On

## VS Code Configuration

It is recommended using the default Key Setup.
This allows for others to interact with your Code to help you, either through direct or remote and removes any “*Why isn't this key combo working?*”.

The following are a list of known Visual Code extensions that are useful for NodeJS and others.

```PowerShell
code --install-extension shd101wyy.markdown-preview-enhanced # Markdown Preview Enhanced: https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced
code --install-extension leizongmin.node-module-intellisense # Modules Intellisense: https://marketplace.visualstudio.com/items?itemName=leizongmin.node-module-intellisense
code --install-extension waderyan.nodejs-extension-pack # Node.js Extension Pack: https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack
code --install-extension afractal.node-essentials # Node.js Essential Pack: https://marketplace.visualstudio.com/items?itemName=afractal.node-essentials
code --install-extension esbenp.prettier-vscode # Prettier - https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode
code --install-extension rvest.vs-code-prettier-eslint # https://marketplace.visualstudio.com/items?itemName=rvest.vs-code-prettier-eslint
code --install-extension ms-vscode-remote.vscode-remote-extensionpack # https://marketplace.
```

## Visual Studio

It is recommended this is downloaded directly and installed to ensure the correct values are chosen. If in doubt hit default.

[Visual Studio Community 22 Visual Studio 2022 Community Edition – Download Latest Free Version](https://visualstudio.microsoft.com/vs/community/)

Other often required applications:

```sh
sudo apt install python2 # Python2
sudo apt install python-is-python3 # Python diverter
sudo apt-get install build-essential # Build essentials (Make is required)
```

## Setup Git

(Note: The following assumes you only have a single email that you use, if you have multiple then you will need to adjust as your needs.)

***Note:** Make sure to update the scripts with your Name and Email Address*

```sh
cd ~
sudo apt-get install git
git config --list --global # shows what you currently have in your git config
git config --global user.name "<Your Name>" # Set your name
git config --global user.email "<Your github registered email>" # Set your email
git config --global init.defaultBranch main # Set default branch name
```

### Setup a SSH key

***Note:** The original source is here: [Generating a new SSH key and adding it to the ssh-agent - GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)*

Create a new SSH key:

```sh
ssh-keygen -t ed25519 -C "<your github registered email address>"
# Use the default location (Press enter)
# Enter a passphrase you can remember! (Recommend the same as your Ubuntu password)
cd ~/.ssh # cd to the SSH directory
eval "$(ssh-agent -s)" #Start the SSH Agent Service
ssh-add ~/.ssh/id_ed25519 #Add your new SSH key
```

#### Register the SSH key with GitHub

```sh
nano id_ed25519.pub # Display the key - Unless you changed the path
```

Copy the contents
Log into [GitHub](https://github.com/)
Click on your profile pic and select settings
Click on SSH and GPG Keys
Click New SSH Key
Call it "*WSL-&lt;distroname&gt;-&lt;computername&gt;*" so it can be easily identified.
Paste in the contents

#### Register the SSH Key with BitBucket

Log into [BitBucket](https://bitbucket.org/account/settings/) Personal Settings
Click on SSH
Click Add SSH Key
Call it "*WSL-&lt;distroname&gt;-&lt;computername&gt;*" so it can be easily identified.
Paste in the contents

### Install Node Dev Environment

```sh
cd ~
sudo apt-get install curl # Install curl (should be installed by default on newer distro's but just in case)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash # Install npm
```

**IGNORE THE ERROR - MOVE ALONG, IT WASN'T THERE AND THIS TYPE OF BEHAVIOUR IS FULLY ACCEPTABLE IN THE WORLD OF NODEJS DEV.**

> Restart WSL or cut/paste/run the Export command text that is listed on screen

### Install Node versions

```sh
nvm install lts/hydrogen # Current LTS (18)
nvm alias default lts/hydrogen
npm install -g yarn # Install Yarn
npm install -g serverless # Install serverless framework
```

### Install MySQL2

***Note:** If you have MySQL installed and the service running in Windows then this will fail. You will need to disable it via PowerShell.*

```sh
sudo apt install mysql-server # Install mysql-server
net stop MySQL
Set-Service -StartupType Disabled MySQL
sudo service mysql start # Start the service
```

Update the root password:

```sh
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'toor';
exit
```

Run the mysql setup, and follow the settings below:

```sh
sudo mysql_secure_installation
```

>Validate Password Component: no
>Change password for root: no
>Remove anonymous Users: yes
>Disallow Root Login Remotely: no
*Note: Disallow Root Login Remotely is turned off required for legacy security approach implemented by poor developers.*
>Remove test database: yes
>Reload privilege tables: yes

Check it’s working:

```sh
sudo mysql -u root -p 
# It will prompt for the password (Even if you put it in the cli)
show databases;
sudo nano /etc/wsl.conf # Setup mysql to start on launch
```

Add the following to the file:

>[boot]
>command = "service mysql start"
>Exit and Save (Ctrl+X then yes)

### Setup AWS (Missing)

***Note:** (This needs to install and setup the variables for AWS)*

### Install GitKraken

```sh
wget https://release.gitkraken.com/linux/gitkraken-amd64.deb
sudo apt install ./gitkraken-amd64.deb
gitkraken # Run GitKraken
```

>Set up your profile
>Go into Preferences: File/Preferences
>Open Profiles
>Ensure the email here is your github email address
>Open Integrations
>Select GitHub
>Click Connect to GitHub button
>Log in to your Github account on the opened browser
>Manually Copy the OAuth Token into GitKrakens OAuth Token.
>Click Generate SSH key and add to GitHub
>Select Bitbucket and copy into the token space the OAuth Token from the opened window.
>Log into your Bitbucket account on the opened browser
>Manually Copy the OAuth Token into GitKrakens OAuth Token.
>Click Generate SSH key and Add to Bitbucket
>Click Copy public key to clipboard, this will also open on the correct Bitbucket Page
>Add the Key to Bitbucket: <https://bitbucket.org/account/settings/ssh-keys/>
>Recommended Name: *Personal Key (GitKraken-WSL-Ubuntu-v2-&lts;computeridentifier&gt;)*

***Note:** GitKraken will prompt to update regularly. This can be completed by downloading the .deb package when asked and running (and replacing the **** with the correct version)*

```sh
sudo apt install ./gitkraken-v*****.deb
```

### Docker

[Docker for WSL](https://docs.docker.com/desktop/wsl/)

### Terraform

Condensed from: [](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

```sh
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint
```

Confirm gpg outputs:

```text
/usr/share/keyrings/hashicorp-archive-keyring.gpg
-------------------------------------------------

pub   rsa4096 XXXX-XX-XX [SC]
AAAA AAAA AAAA AAAA
uid           [ unknown] HashiCorp Security (HashiCorp Package signing) <security+packaging@hashicorp.com>
sub   rsa4096 XXXX-XX-XX [E]
```

```sh
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt-get install terraform
terraform -help
touch ~/.bashrc
terraform -install-autocomplete
```