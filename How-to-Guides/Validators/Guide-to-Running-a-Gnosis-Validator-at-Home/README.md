# Guide to Running a Gnosis Validator at Home

This tutorial will show how to run a Gnosis Validator at Home. Because this tutorial is implemented on Cloud server as an example, you can implement it by using the same method with your hardware at home.

This tutorial are available in [English](https://github.com/hoduchieu01/Guide-to-Running-a-Gnosis-Validator-on-Cloud/blob/main/README.md), [Vietnamese](https://github.com/hoduchieu01/Guide-to-Running-a-Gnosis-Validator-on-Cloud/blob/main/locales/VN/README.md).

### 1. Create a Docker droplet on Cloud Service
Based on the [Beacon Chain Node Requirement](https://docs.gnosischain.com/node/consensus-layer-validator#beacon-chain-node-requirements), you need a machine equipped with an SSD because HDD is too slow. The recommended droplet that used in this tutorial is 32 GB Memory / 600 GB Disk - Ubuntu 22.10 x64

### 2. Connect to your droplet
Copy ipv4 address of your droplet. Open the terminal and type `ssh root@<YOUR_DROPLET_IPV4_ADDRESS>`

### 3. Install DAppNode
According the [installation guide of the DappNode](https://github.com/dappnode/DAppNode), because droplet is running on the Ubuntu, DappNode will be installed with scripts by following these commands in [Install DAppNode with scripts](https://github.com/dappnode/DAppNode#install-dappnode-with-scripts) as below

Get Prerequisites

```sudo wget -O - https://prerequisites.dappnode.io | sudo bash```

Script Installation

```sudo wget -O - https://installer.dappnode.io | sudo bash```

### 4. Install VPN - Wireguard VPN
There are various ways to access the DAppNode including local proxy, wifi hotspot, VPN Connections, and CLI. This tutorial shows how to run a Gnosis Validator on the Cloud Service using DAppNode, the VPN method is mostly used while implementing on the Cloud service. OpenVPN and Wireguard are both available. However, this tutorial will show the guide for Wireguard due to the more convenient setup with commands on the Ubuntu VPS and more stable. 

To install Wireguard in Ubuntu, you can follow by these commands:
```
sudo apt install openresolv
sudo apt install wireguard
```

In case of your DAppNode doesn't show the information for connect, you need to use the command ```dappnode_connect``` to shows the details.

You will get the information as below
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

### 5. Connect to your DAppNode through VPN
Following [Wireguard installation guide](https://docs.dappnode.io/user-guide/ui/access/vpn/#linux) to install Wireguard in your computer, you need to create wg0 configure file ```sudo nano /etc/wireguard/wg0.conf``` and copy the configure information as above into your configure file. 
Finally, you need to use this command to start Wireguard: ```sudo wg-quick up wg0```. The terminal will be shown as below

![image](https://user-images.githubusercontent.com/23649434/201591812-97c4bcb7-5760-485f-a7a3-62d5e8418d46.png)


Then you can visit the http://my.dappnode on your browser as below

![image](https://user-images.githubusercontent.com/23649434/201589528-7b7edab0-f7f7-48fa-a656-f55b416cd505.png)

You can refer to this guide as [Initial Configurations for the DAppNode](https://docs.dappnode.io/first-steps#)

### 6. Setting up Gnosis Validator on DAppNode
From the Admin UI, please visit DAppStore and search Gnosis Beacon Chain Prysm.

![image](https://user-images.githubusercontent.com/23649434/201592661-9111180f-3ab3-49d8-a1ca-c67fa53e5cb2.png)

The image below shows the configure settings

![image](https://user-images.githubusercontent.com/23649434/201593138-663c57bc-5351-41f7-a786-817e4e1a8bcb.png)

You also need to install Nethermind Xdai(Gnosis chain) as maintaining your own execution client.

After installing successfully, the chains and block sync status are also available on the Dashboard.

### 7. Key Generator
There are two methods for key generators.

1. The first method is using Docker to generate keystores and spin up clients. Based on the official guide of [Step 1) Generate Validator Account(s) and Deposit Data](https://docs.gnosischain.com/node/consensus-layer-validator#step-1-generate-validator-accounts-and-deposit-data). Please follow these commands step by step for generate your Keystores

Download the Key Generator

```
cd
sudo docker pull ghcr.io/gnosischain/validator-data-generator:latest
```

Create Folder for Your Key Storage
```
mkdir home/<your_username>/vkeys
```

Run the Generator to Create Keystores by changing `/home/<your_username>/`, NUM, WITHDRAWAL_ADDRESS
```
docker run -it --rm -v /home/<your_username>/validator_keys:/app/validator_keys \
  ghcr.io/gnosischain/validator-data-generator:latest new-mnemonic \
  --num_validators=NUM --mnemonic_language=english \
  --folder=/app/validator_keys --eth1_withdrawal_address=WITHDRAWAL_ADDRESS
```

Then, you will get the deposit_data*.json and keystores in the /home/<your_username>/validator_keys

2. The second method is using the Gnosis Wagyu Key Gen.

Install the Gnosis Wagyu Key Gen using this [link](https://github.com/alexpeterson91/Gnosis-Wagyu-Key-Gen/releases). Then, open the Gnosis Wagyu Key Gen and complete Create Secret Recovery Phrase, Configure Validator Keys as the image below.

Following step by step
![image](https://user-images.githubusercontent.com/23649434/201819925-3e318c83-798f-4397-b860-71f857898804.png)

Then, you can create validator key files as the image below.
![image](https://user-images.githubusercontent.com/23649434/201820812-5119f61f-c096-4b8d-b4d4-aec960ae7f6f.png)


### 8. Configure Keystores to Web3signer Gnosis package on DAppNode
Navigate [http://ui.web3signer-gnosis.dappnode/](http://ui.web3signer-gnosis.dappnode/). Then, you need to import keystores from the file that you have created in the previous step.
Finally, you will have the validator public key as the image below and you can also check your validator public key through the website https://beacon.gnosischain.com/validator/<your_validator_publickey>

![image](https://user-images.githubusercontent.com/23649434/201821914-47f9279a-91c2-4dc1-9c86-49dbff4cba78.png)

### 9. Claim your validator
Once you have the validator public key, you need to claim these validator keys by deposit 1GNO per validator. Navigate [https://deposit.gnosischain.com/](https://deposit.gnosischain.com/) and connect with your wallet. Then, you need to upload the deposit_data*.json that you generated in the section 7.

![image](https://user-images.githubusercontent.com/23649434/201823454-dd479504-bcc6-4aa2-8ba3-35df0ad4834f.png)

Then, you click `Deposit`. Finally, you have successfully created your validator on Gnosis Chain. More detailed informations can be referred through [https://docs.gnosischain.com/](https://docs.gnosischain.com/).


## How to contribute
The primary purpose of this repository is to continue evolving develops for running a Gnosis Validator . We want to make contributing to this project as easy and transparent as possible, and we grateful to the community for contributing bug fixes and improvements. Read below to learn how you can participate in improving this tutorial.

### Good First Issues
We have a [list of issues](https://github.com/hoduchieu01/Guide-to-Running-a-Gnosis-Validator-on-Cloud/issues) that contain bugs which have a relatively limited scope. This is a great place to get started, gain experience, and get familiar with our contribution process.


## License
Guide to Running a Gnosis Validator at Home is [MIT licensed](./LICENSE).
