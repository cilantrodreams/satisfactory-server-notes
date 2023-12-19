# Satisfactory Server Notes

Notes on setting up a dedicated satisfactory server hosted in a data center. The server is deployed and managed with LinuxGSM and hosted on OVHCloud public cloud.

## System Requirements
| Syntaxs | Description |
| - | - |
| Processor | Recent (comparable to the i5-3570 [Intel] or Ryzen 5 3600 [AMD] or better) x86/64 (AMD/Intel) processor. No 32 bit or ARM support. The server favours higher single-core performance over multiple cores. |
| Memory | 12GB minimum, 16GB RAM is recommended for larger saves or to host more than 4 players. |
| Storage | 25GB for the game server itself |
| Operaating System | Any currently supported version of Windows or major Linux distribution. Out-of-support OSs such as Windows 7 are explicitly not supported. |
| Internet Connection | Broadband internet connection. Hosting from home will require the ability to configure port forwarding. |

## Setting up server
[Guide from OVHCloud documentation](https://help.ovhcloud.com/csm/en-gb-public-cloud-compute-getting-started?id=kb_article_view&sysparm_article=KB0051017)
1. Create an account on OVHCloud with a new project
1. Add SSH public key to the project
1. From the top bar navigate to Public Cloud and create a new Compute Instance
1. Select appropriate model for server (Depends on region availability and pricing)
1. Select server region (Only one in North America)
1. Select Ubuntu newest LTS for OS image and add your ssh key from earlier to it.
1. Select Flexible instance under instance configuration. Flex option locks the storage at 50Gb, but will allow you to downgrade your instance instead of having to remake it entirely. A satisfactory server will easily fit in the instance.
1. Under Configure your network select public mode
1. Select hourly or monthly billing period.
1. Create the instance.
1. Go to the instance dashboard and copy the login information under the Networks column. Paste it into terminal and enter the passphrase for your private key
1. Change the password of the user account

    `sudo passwd ubuntu`
