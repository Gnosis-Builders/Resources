# Builders Cheat Sheet

## Smart Contract Deployment

### (1) Using Truffle 

#### -> Default Compile :
        truffle compile --network chiado
        truffle compile --network gnosis

#### -> Compile with options :
        truffle compile [--list <filter>] [--all] [--network chiado] [--quiet]
	    truffle compile [--list <filter>] [--all] [--network gnosis] [--quiet]

        [--list <filter>] : Compile contract based on a pattern(optional)
        [--all] : Compiles all contracts in the project
        [--quite] : Reduces info displayed during compilation

#### -> Deploy Contract

        truffle migrate --network chiado
        truffle migrate --network gnosis

### (2) Using Hardhat

#### -> Compile Contract :
        npx hardhat compile

#### -> Deploy Contract :
        Gnosis Mainnet : npx hardhat run scripts/deploy.ts --network gnosis
			             npx hardhat run scripts/deploy.js --network gnosis	
		Chiado Testnet : npx hardhat run scripts/deploy.ts --network chiado
			             npx hardhat run scripts/deploy.js --network chiado

#### -> Verify Contract :

        Gnosis Mainnet : npx hardhat verify --network gnosis <deployed contract address>
                               
        Chiado Testnet : npx hardhat verify --network chiado <deployed contract address>

### (3) Using Foundry

#### -> Compile Contract : 
        forge build

#### -> Deploy Contract : 


#### -> Gnosis Mainnet : 

        forge create --rpc-url https://rpc.gnosischain.com --private-key <your_private_key> src/<YourContract>.sol:<YourContract>

#### -> Chiado Testnet : 

        forge create --rpc-url https://rpc.chiadochain.net --private-key <your_private_key> src/<YourContract>.sol:<YourContract>

#### ->Deploy with arguments

#### -> Gnosis Mainnet : 

        forge create --rpc-url https://rpc.gnosischain.com \
            --constructor-args <argument-1> <argument-2...>\
            --private-key <your_private_key> src/<YourToken>.sol:<YourToken> \

#### -> Chiado Testnet : 

        forge create --rpc-url https://rpc.chiadochain.net \
            --constructor-args <argument-1> <argument-2...>\
            --private-key <your_private_key> src/<YourToken>.sol:<YourToken> \


#### -> Verify Your contract: 

#### -> Gnosis Mainnet : 

        forge create --rpc-url https://rpc.gnosischain.com \
            --private-key <your_private_key> src/<YourToken>.sol:<YourToken> \
            --etherscan-api-key <your_etherscan_api_key> \
            --verify

#### -> Chiado Testnet :

        forge create --rpc-url https://rpc.chiadochain.net \
            --private-key <your_private_key> src/<YourToken>.sol:<YourToken> \
            --etherscan-api-key <your_etherscan_api_key> \
            --verify


## Interacting with Gnosis


### (1)  Using web3.js : 

#### -> Add web3 to project :  
        yarn add web3
        npm install web3 
        Link the dist/web3.min.js
        const web3 = new Web3(Web3.givenProvider); [//create a web3 instance and set a provider]

#### -> Interacting with the contract : 

        var contract = new web3.eth.Contract(jsonInterface[, address][, options])


#### -> Setting Gnosis as custom chain :

#### -> Gnosis Mainnet : 

        web3.eth.defaultCommon = {customChain: {name: 'Gnosis', chainId: 100, networkId: 100}}; 

#### -> ChiadoTestnet : 

        web3.eth.defaultCommon = {customChain: {name: 'Chiado Testnet', chainId: 10200, networkId: 10200}};


### (2) Using Ethers.js 

#### -> Adding Ethers.js to project :
        npm install --save ethers
		yarn add ethers

#### -> To import ethers.js using node.js to application

        const { ethers } = require("ethers");
        import { ethers } from "ethers";

#### -> Connecting gnosis with metamask : 

        const provider = new ethers.providers.Web3Provider(window.ethereum)
        await provider.send("eth_requestAccounts", []);
        const signer = provider.getSigner()


#### -> Connecting Gnosis with RPC :

        const provider = new ethers.providers.JsonRpcProvider();
        const signer = provider.getSigner()
 

#### -> Interacting with contract 

        const Contract = new ethers.Contract(Address, Abi, provider);

