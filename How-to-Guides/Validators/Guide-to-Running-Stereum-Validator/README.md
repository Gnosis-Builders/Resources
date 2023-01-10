# Table of Contents
<details>
  <summary> <a href="#about-stereum">Stereum</a></summary>
</details>
<details>
  <summary> <a href="#hardware-requirements">Hardware Requirements</a></summary>
  <ol>
    <li>
      <a href="#minimum">Minimum</a>
    </li>
    <li>
      <a href="#recommended">Recommended</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#preparing-the-server">Preparing the Server</a></summary>
  <ol>
    <li>
      <a href="#home-server">Home Server</a>
    </li>
    <li>
      <a href="#cloud-server">Cloud Server</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#stereum-launcher">Stereum Launcher</a></summary>
  <ol>
    <li>
      <a href="#step-1-connecting-to-server">Step 1: Connecting to Server</a>
    </li>
    <li>
      <a href="#step-2-installation">Step 2: Installation</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#setup-validator-for-gnosis-chain">Setup Validator for Gnosis Chain</a></summary>
  <ol>
    <li>
      <a href="#step-1-generate-validator-keys">Step 1: Generate Validator Keys</a>
    </li>
    <li>
      <a href="#step-2-upload-your-validator-keys-to-stereum">Step 2: Upload your Validator Keys to Stereum</a>
    </li>
    <li>
      <a href="#step-3-deposit-gno-for-your-validator">Step 3: Deposit GNO for your validator</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#monitoring-your-validators">Monitoring your Validators</a></summary>
</details>

# About Stereum

Stereum provides a fast and easy way for solo stakers to set up their validator nodes within minutes. The easy-to-use, flexible and free Graphical User Interface (GUI) allows users to validate with just one click, eliminating complicated setups and allowing as many people as possible to participate at a network level, keeping the network decentralized and neutral.

To start validating Gnosis using Stereum, users will have to prepare the following:

1. A local machine (Windows, Mac, Linux) to run the Stereum Launcher
2. A server running Linux to host the node on (Cloud, Local Machine, VM)

# Hardware Requirements

## Minimum

- 4 Core CPU
- 1TB SSD Storage
- 8GB RAM Memory
- 10Mbit/s Wired Network

## Recommended

- 8 Core CPU
- 2TB SSD Storage
- 16GB RAM Memory
- 25Mbit/s Wired Network

# Preparing the Server

## Home Server

Coming soon!

## Cloud Server

### DigitalOcean

#### Step 1: Create Project on DigitalOcean

<img width="776" alt="Untitled" src="https://user-images.githubusercontent.com/51161692/211513915-4ad906fb-a00b-44bd-96da-5ebe54c180bb.png">

#### Step 2: Create & Setup Droplets

Select a virtual machine to run your stereum on. Following the recommended setup, we will run a droplet that is closest to our region (i.e. London) with **4 Core CPU**, **32GB RAM,** **900GB SSD Storage & Ubuntu**.

For authentication, we will choose Password option.

<img width="1434" alt="Screenshot_2023-01-10_at_3 00 57_PM" src="https://user-images.githubusercontent.com/51161692/211514326-1f453a95-a5c3-4f48-a85d-058c6b4a4d0d.png">

# Stereum Launcher

Once the server has been set up and running, we will now download the Stereum Launcher from [stereum.net](http://stereum.net) to set up our server. Download stereum according to the OS that your machine is running (Mac, Windows & Linux).

## Step 1: Connecting to Server

Enter your server details as given by your selected cloud providers. In this case, since we are not using ssh, we will toggle USE SSH KEY to off. 

<img width="1052" alt="Untitled 1" src="https://user-images.githubusercontent.com/51161692/211514465-5b995348-a9f7-4f22-8335-2b9e884a7fc9.png">

## Step 2: Installation

After connecting to your node, Stereum provides you with multiple options with regard to the installation of your Gnosis node:

- 1-Click Installation
    - Pre-configured node installation
- Custom Installation
    - Choose your own clients and services
- Import Configuration
    - For migrating node setup, previous configuration files can be imported to save time

In this guide, we will be choosing “1-Click Installation” as it provides the most straightforward option to validate for Gnosis.

<img width="1058" alt="Untitled 2" src="https://user-images.githubusercontent.com/51161692/211514531-d877c1b1-8aba-445b-82bc-f276bd3bdaa5.png">

Once the option has been selected, select Gnosis under the menu dropdown and select Staking. Finally, select Install.

<img width="1053" alt="Untitled 3" src="https://user-images.githubusercontent.com/51161692/211514610-8d5b8180-ebd8-4a4e-843e-44cf1dd35862.png">

Stereum will install the necessary services and clients automatically. Click Next.

<img width="1055" alt="Untitled 4" src="https://user-images.githubusercontent.com/51161692/211514666-6e01f5eb-a26c-4843-bf03-02f8d136a226.png">

Click Next after verifying the services and clients that Stereum are installing.

<img width="1051" alt="Untitled 5" src="https://user-images.githubusercontent.com/51161692/211514724-6f5a4b07-4d3c-4385-8eca-06fa3170b080.png">

Click Install to begin the installation process.

<img width="1053" alt="Untitled 6" src="https://user-images.githubusercontent.com/51161692/211514760-a4a6a1e2-5669-4410-9791-f7c1cc7a2393.png">

The installation will start automatically. Wait for the installation to be done before applying further changes.

<img width="1054" alt="Untitled 7" src="https://user-images.githubusercontent.com/51161692/211514861-4dff24f6-c357-457d-a7cf-122147c48833.png">

# Setup Validator for Gnosis Chain

Now that you have set up and configured your stereum on your dedicated machine, it’s time to set up your validator so that you can start validating for Gnosis.

## Step 1: Generate Validator Keys

To begin validating for Gnosis, you will need to generate a validator private key to sign on-chain operations. Follow this updated guide [here](https://docs.gnosischain.com/node/guide/validator/generate-keys/wagyu) to see how can you generate validator keys using Wagyu Key Gen.

## Step 2: Upload your Validator Keys to Stereum

We will now upload the keystone files generated in the previous step into your stereum node. On the top left-hand menu, select STAKING. You can either click or drag your keystore.json file into the window to load your keystore files. 

![Untitled 8](https://user-images.githubusercontent.com/51161692/211514963-99055c7d-71d3-4956-b031-caa5b87c906c.png)

Once you have loaded your keystore.json file, click ENTER PASSWORD & IMPORT to import your keystores.

![Untitled 9](https://user-images.githubusercontent.com/51161692/211515032-2e51112d-37c4-41d6-ad96-21e0e39a2704.png)

Once done, your keys will start to load.

![Untitled 10](https://user-images.githubusercontent.com/51161692/211515065-0f518e48-7a9a-4cf7-b96e-33e5999ae20a.png)

Congratulations! Your keystores has been loaded on your stereum node.

![Untitled 11](https://user-images.githubusercontent.com/51161692/211515114-7ed5318e-349a-4be3-a839-14f72908fb66.png)

## Step 3: Deposit GNO for your validator

> :warning: Note: Ensure that your Stereum node is fully synced before depositing your GNO. Else, your funded validators will be offline and you will be penalised for missing attestations. The sync process will take roughly 1-4 days.

To check on the sync status of your node, on the top left menu select CONTROL, and you will be able to see the sync status for both your execution and consensus clients. Ensure that both are fully sync before continuing.  

![Untitled 12](https://user-images.githubusercontent.com/51161692/211515197-032f6ec6-1cfd-4daa-88ea-0a2d2b51fa77.png)

Once validator keys have been generated, you will need to make a deposit of **1 GNO for each validator**. Follow this updated guide [here](https://docs.gnosischain.com/node/guide/validator/deposit) to see how you can make deposits for your validator.

# Monitoring your Validators

If you have selected 1-Click Installation at the start, the Grafana service would have been installed alongside the Stereum node. To access Grafana dashboards to monitor your validators, select the Grafana logo located at the right-hand side of the menu bar. Once the pop-up appears, open the Grafana Dashboard in your browser.

![Untitled 13](https://user-images.githubusercontent.com/51161692/211515244-0ad72939-cd97-458a-989a-85ee79b60b7c.png)

Select Dashboards on the menu located on the left, and under general, you will be presented with a list of pre-configured dashboards for your node.

![Untitled 14](https://user-images.githubusercontent.com/51161692/211515285-4d3735c0-d39e-4a88-946b-90fb3e82575b.png)

![Untitled 15](https://user-images.githubusercontent.com/51161692/211515322-b47d374f-b713-4473-b044-3d68144e997f.png)
