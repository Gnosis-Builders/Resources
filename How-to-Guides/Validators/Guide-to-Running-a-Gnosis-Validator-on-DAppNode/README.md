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
Finally, you need to use this command to start Wireguard: ```sudo wg-quick up wg0```. You will receive output like the following

```
Output:
[#] ip link add wg0 type wireguard
[#] wg setconf wg0 /dev/fd/63
[#] ip -4 address add <IPv4 addresses> dev wg0
[#] ip link set mtu 1412 up dev wg0
[#] resolvconf -a tun.wg0 -m 0 -x
[#] ip -4 route add <IPv4 addresses> dev wg0
[#] ip -4 route add <IPv4 addresses> dev wg0
```

The status of the tunnel on the peer can check via the command ```sudo wg0```

```
Output:
interface: wg0
  public key: <public_key>
  private key: (hidden)
  listening port: <listening_port>

peer: <peer>
  endpoint: <endpoint>
  allowed ips: <allowed_ips>
  latest handshake: 1 second ago
  transfer: 6.28 KiB received, 3.94 KiB sent
```

Then you can visit the http://my.dappnode on your browser as follows

![image](https://user-images.githubusercontent.com/23649434/201589528-7b7edab0-f7f7-48fa-a656-f55b416cd505.png)

You can refer to this guide as [Initial Configurations for the DAppNode](https://docs.dappnode.io/first-steps#)

### 7. Setting up Gnosis Beacon Chain (GBC) Validator on DAppNode

We will now configure and set up DAppNode on your cloud server so that we can start validating for Gnosis Chain. You can follow the steps below to set up your validator(s).
Learn how to setup DAppNode via this online documentation [https://docs.gnosischain.com/node/tools/dappnode](https://docs.gnosischain.com/node/tools/dappnode)

## How to contribute
The primary purpose of this repository is to continue evolving developments for running a Gnosis Validator. We want to make contributing to this project as easy and transparent as possible, and we are grateful to the community for contributing bug fixes and improvements. Read below to learn how you can participate in improving this tutorial.
