# Satisfactory Server Notes

Notes on setting up a dedicated satisfactory server hosted in a data center. The server is deployed and managed with LinuxGSM and hosted on OVHCloud public cloud.

## System Requirements
|  |  |
| - | - |
| Processor | Recent (comparable to the i5-3570 [Intel] or Ryzen 5 3600 [AMD] or better) x86/64 (AMD/Intel) processor. No 32 bit or ARM support. The server favours higher single-core performance over multiple cores. |
| Memory | 12GB minimum, 16GB RAM is recommended for larger saves or to host more than 4 players. |
| Storage | 25GB for the game server itself |
| Operaating System | Any currently supported version of Windows or major Linux distribution. Out-of-support OSs such as Windows 7 are explicitly not supported. |
| Internet Connection | Broadband internet connection. Hosting from home will require the ability to configure port forwarding. |

## Setting up server on OVHCloud
[Guide from OVHCloud documentation](https://help.ovhcloud.com/csm/en-gb-public-cloud-compute-getting-started?id=kb_article_view&sysparm_article=KB0051017)
1. Create an account on OVHCloud with a new project
1. Add SSH public key to the project
1. From the top bar navigate to Public Cloud and create a new Compute Instance
1. Select appropriate model for server (Depends on region availability and pricing)
1. Select server region (Only one in North America)
1. Select Ubuntu newest LTS for OS image and add your ssh key from earlier to it.
1. Select Flexible instance under instance configuration. Flex option locks the storage at 50Gb, but will allow you to downgrade your instance instead of having to remake it entirely. A satisfactory server will easily fit in the instance.
1. Under Configure your network select public mode
1. Select hourly billing period.
1. Create the instance.
1. Go to the instance dashboard and copy the login information under the Networks column. Paste it into terminal and enter the passphrase for your private key
1. Change the password of the user account

    `sudo passwd ubuntu`

## Setting up OpenStack API on server instance
OpenStack tools are used to manage OVHCloud instances from the system console. WIll be used later to automate management of the instance using scripts.
[Documentation from OVHCloud](https://help.ovhcloud.com/csm/en-public-cloud-compute-prepare-openstack-api-environment?id=kb_article_view&sysparm_article=KB0050988)
[OpenStack API documentation](https://docs.openstack.org/python-openstackclient/latest/)
1. SSH into instance and run `apt update`
1. Use commands below to install OpenStack client
```
apt install python3-pip python3-venv
python3 -m venv env
source env/bin/activate
pip3 install --upgrade pip
pip3 install python-openstackclient
```
1. Openstack is now ready to use. You can exit env by using command `deactivate`

## Deploying Satisfactory game server using LinuxGSM
[LinuxGSN page for Satisfactory] (https://linuxgsm.com/servers/sfserver/)
1. Install all required dependencies

    For Ubuntu versions greater than 20.10

    `sudo dpkg --add-architecture i386; sudo apt update; sudo apt install curl wget file tar bzip2 gzip unzip bsdmainutils python3 util-linux ca-certificates binutils bc jq tmux netcat lib32gcc-s1 lib32stdc++6 libsdl2-2.0-0:i386 steamcmd
`
1. Install additional module GameDig. GameDig is a tool to monitor game servers

    [GameDig installation instructions from LinuxGSM documentation] (https://docs.linuxgsm.com/requirements/gamedig?_ga=2.241630212.747431594.1703013593-999399711.1702833779&_gl=1*llpn32*_ga*OTk5Mzk5NzExLjE3MDI4MzM3Nzk.*_ga_QTLEDYPYK9*MTcwMzA2MDgxNC42LjAuMTcwMzA2MDgxNC4wLjAuMA..)

    Install the latest version of nvm

    `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash`

    Verify it has been installed

    `command -v nvm`

    It should output `nvm`. If not, close the current terminal, open a new one and try again.

    Install the latest version of node using nvm

    `nvm install node`

    Once node is installed, use npm to install GameDig

    `npm install gamedig -g`

1. Install Satisfactory dedicated server with the following steps

    Create a user and login

    `adduser sfuser`

    Set a strong password for security best practices

    `su - sfserver`

    Download linuxgsm.sh

    `wget -O linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh sfserver`

    Run the installer and follow on screen instuctions

    `./sfserver install`