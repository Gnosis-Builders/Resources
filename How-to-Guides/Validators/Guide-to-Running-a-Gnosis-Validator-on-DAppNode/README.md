# Guide to Running a Gnosis Validator on DAppNode

This tutorial will show how to run a Gnosis Validator on DAppNode. Because this tutorial uses a cloud server as an example, you may use your own hardware at home to implement it by following the same procedure or install DAppNode with ISO by following this [ISO installation guide](https://docs.dappnode.io/user/quick-start/core/installation/#iso-installation).

### 1. Create a Docker droplet on Cloud Service
Regarding the [Requirement for Running a Node from Gnosis Chain Official Document](https://docs.gnosischain.com/node/#requirements), we recommend your node have a CPU with at least 4 threads, at least 16 GB of RAM, and NVMe SSD (preferred) or SATA SSD. 

The recommended droplet used in this tutorial is 32 GB Memory - 600 GB Disk - Ubuntu 22.10 x64

### 2. Connect to your droplet
Copy the ipv4 address of your droplet. Open the terminal and type `ssh root@<YOUR_DROPLET_IPV4_ADDRESS>`

### 3. Install DAppNode
According to the [installation guide of the DAppNode](https://github.com/dappnode/DAppNode), because the droplet is running on Ubuntu, DAppNode will be installed with scripts by following these commands in [Install DAppNode with scripts](https://github.com/dappnode/DAppNode#install-dappnode-with-scripts) as follows

Get Prerequisites

```sudo wget -O - https://prerequisites.dappnode.io | sudo bash```

Script Installation

```sudo wget -O - https://installer.dappnode.io | sudo bash```

### 4. Initialize DAppNode aliases
Once DAppNode has been successfully started in your machine, run `dappnode_help` to see the full list of commands available. If running `dappnode_help` shows the command not found, you will need to add these aliases.
```
$ alias
alias dappnode_connect='docker exec -ti DAppNodeCore-vpn.dnp.dappnode.eth getAdminCredentials'
alias dappnode_get='docker exec -t DAppNodeCore-vpn.dnp.dappnode.eth vpncli get'
alias dappnode_help='echo -e "\n\tDAppNode commands available:\n\n\tdappnode_help\t\tprints out this message\n\n\tdappnode_wifi\t\tget wifi credentials (SSID and password)\n\n\tdappnode_openvpn\tget Open VPN credentials\n\n\tdappnode_wireguard\tget Wireguard VPN credentials (dappnode_wireguard --help for more info)\n\n\tdappnode_connect\tcheck connectivity methods available in DAppNode\n\n\tdappnode_status\t\tget status of dappnode containers\n\n\tdappnode_start\t\tstart dappnode containers\n\n\tdappnode_stop\t\tstop dappnode containers\n"'
alias dappnode_openvpn='docker exec -i DAppNodeCore-vpn.dnp.dappnode.eth getAdminCredentials'
alias dappnode_openvpn_get='docker exec -i DAppNodeCore-vpn.dnp.dappnode.eth vpncli get'
alias dappnode_start='docker-compose $DNCORE_YMLS up -d && docker start $(docker container ls -a -q -f name=DAppNode*)'
alias dappnode_status='docker-compose $DNCORE_YMLS ps'
alias dappnode_stop='docker-compose $DNCORE_YMLS stop && docker stop $(docker container ls -a -q -f name=DAppNode*)'
alias dappnode_wifi='cat /usr/src/dappnode/DNCORE/docker-compose-wifi.yml | grep "SSID\|WPA_PASSPHRASE"'
alias dappnode_wireguard='docker exec -i DAppNodeCore-api.wireguard.dnp.dappnode.eth getWireguardCredentials'
alias ls='ls --color=auto'
```

### 5. Install VPN - Wireguard VPN
There are various ways to access the DAppNode including local proxy, wifi hotspot, VPN Connections, and CLI. This tutorial shows how to run a Gnosis Validator on the Cloud Service using DAppNode, the VPN method is mostly used while implementing on the Cloud service. OpenVPN and Wireguard are both available. However, this tutorial will show the guide for Wireguard due to the more convenient setup with commands on the Ubuntu VPS and more stable. 

To install Wireguard in Ubuntu, you can follow these commands:
```
sudo apt install wireguard
```

In case your DAppNode doesn't show the information for connecting, you need to use the command ```dappnode_connect``` to show the details.

You will get the information below
```
Connect to DAppNode through Wireguard using the following credentials:
Preparing remote text Wireguard credentials; use CTRL + C to stop

[Interface]
Address = <YourInterfaceAddress>
PrivateKey = <YourInterfacePrivatekey>
ListenPort = 51820
DNS = <YourInterfaceDNS>

[Peer]
PublicKey = <YourPeerPublicKey>
Endpoint = <YourPeerEndpoint>
AllowedIPs = <YourPeerAllowedIPs>
```

### 6. Connect to your DAppNode through VPN
Following [the Wireguard installation guide](https://docs.dappnode.io/user-guide/ui/access/vpn/#linux) to install Wireguard on your computer, you need to create wg0 configure file ```sudo nano /etc/wireguard/wg0.conf``` and copy the configure information as above into your configuration file. 
Finally, you need to use this command to start Wireguard: ```sudo wg-quick up wg0```. The terminal will be shown as follows

```
[#] ip link add wg0 type wireguard
[#] wg setconf wg0 /dev/fd/63
[#] ip -4 address add 10.24.0.2 dev wg0
[#] ip link set mtu 1412 up dev wg0
[#] resolvconf -a tun.wg0 -m 0 -x
[#] ip -4 route add 10.20.0.0/24 dev wg0
[#] ip -4 route add 172.33.0.0/16 dev wg0
```

In addition, when you run the command ```sudo wg```, you will get this message as follows in the terminal
```
interface: wg0
  publickey: <public_key>
  privatekey: (hidden)
  listening port: 51820
```

Then you can visit the http://my.dappnode on your browser as follows

![image](https://user-images.githubusercontent.com/23649434/201589528-7b7edab0-f7f7-48fa-a656-f55b416cd505.png)

You can refer to this guide as [Initial Configurations for the DAppNode](https://docs.dappnode.io/first-steps#)

### 7. Setting up Gnosis Validator on DAppNode
From the Admin UI, please visit DAppStore and search Gnosis Beacon Chain Prysm.

![image](https://user-images.githubusercontent.com/23649434/201592661-9111180f-3ab3-49d8-a1ca-c67fa53e5cb2.png)

The image below shows the configured settings

![image](https://user-images.githubusercontent.com/23649434/201593138-663c57bc-5351-41f7-a786-817e4e1a8bcb.png)

You also need to install Nethermind Xdai(Gnosis chain) as maintaining your own execution client.

After installing successfully, the sync status is also available on the Dashboard. We recommend the use of Checkpoint sync to sync your node quickly, and avoid long-range attacks. You can use the checkpoint sync server at [https://checkpoint.gnosischain.com/](https://checkpoint.gnosischain.com/) provided by Gnosis or [https://checkpoint-sync-gnosis.dappnode.io/](https://checkpoint-sync-gnosis.dappnode.io/) provided by DAppNode.

### 8. Key Generator
There are two methods for key generators.

1. The first method is using the Command Line Tool. You can refer to this [official Command Line Tool guide by Gnosis Chain](https://docs.gnosischain.com/node/guide/validator/generate-keys/cli/) step-by-step.

2. The second method is using the Gnosis Wagyu KeyGen.

Install the Gnosis Wagyu Key Gen using this [link](https://github.com/alexpeterson91/Gnosis-Wagyu-Key-Gen/releases). Then, open the Gnosis Wagyu Key Gen and complete Create Secret Recovery Phrase, Configure Validator Keys as in the image below.

Following step by step
![image](https://user-images.githubusercontent.com/23649434/201819925-3e318c83-798f-4397-b860-71f857898804.png)

Then, you can create validator key files as in the image below.
![image](https://user-images.githubusercontent.com/23649434/201820812-5119f61f-c096-4b8d-b4d4-aec960ae7f6f.png)


### 9. Configure Keystores to Web3signer Gnosis package on DAppNode
Regarding the new version of DAppNode, you can install the Web3Signer using the following documentation [https://docs.dappnode.io/user/guides/validation-muticlient/](https://docs.dappnode.io/user/guides/validation-muticlient)

Navigate [http://ui.web3signer-gnosis.dappnode/](http://ui.web3signer-gnosis.dappnode/). Then, you need to import keystores from the file that you have created in the previous step.

Finally, you will have the validator public key as the image below and you can also check your validator public key through the website https://beacon.gnosischain.com/validator/<your_validator_publickey>

![image](https://user-images.githubusercontent.com/23649434/201821914-47f9279a-91c2-4dc1-9c86-49dbff4cba78.png)

### 10. Claim your validator
Once you have the validator public key, you need to claim these validator keys by depositing 1 GNO per validator. Navigate [https://deposit.gnosischain.com/](https://deposit.gnosischain.com/) and connect with your wallet. Then, you need to upload the deposit_data*.json that you generated in section 7.

![image](https://user-images.githubusercontent.com/23649434/201823454-dd479504-bcc6-4aa2-8ba3-35df0ad4834f.png)

Then, you click `Deposit`. Finally, you have successfully created your validator on Gnosis Chain. More detailed information can be referred at [https://docs.gnosischain.com/](https://docs.gnosischain.com/).


## How to contribute
The primary purpose of this repository is to continue evolving developments for running a Gnosis Validator. We want to make contributing to this project as easy and transparent as possible, and we are grateful to the community for contributing bug fixes and improvements. Read below to learn how you can participate in improving this tutorial.
