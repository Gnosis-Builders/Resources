# Issues relating to Validator Node Setup

We will use this file to track all current issues regarding setting up a validator node for Gnosis Chain.

### Issue 1: Official Gnosis website not linking to the proper documentation to setup validator node.
In the [Official Gnosis Chain website for Validators](https://www.gnosis.io/validators), currently the link for `Start Validating Today` leads the user to a Page Not Found [link](https://docs.gnosischain.com/node/get-started). Users will have to navigate through the website again to locate the proper documenation for running up a node, which can be found pp[here](https://docs.gnosischain.com/node/).

### Issue 2: Mistake within the Official Gnosis Chain documentation site.
For users looking to setup their own dedicated node to start validating for Gnosis Chain, the instruction for `docker-compose.yml` in [here](https://docs.gnosischain.com/node/guide/validator/run/lighthouse) is incorrect, at line 20, where the data directory flag `--datadir` is set to `/data/validators`, and should be set to `/data` instead. The correct settings for the `validator` container in `docker-compose.yml` file should be as follows:
```
validator:
    container_name: validator
    image: sigp/lighthouse:latest-modern
    restart: always
    command: |
      lighthouse
      validator_client
      --network=gnosis
      --datadir=/data/validators
      --beacon-nodes=http://consensus:4000
      --graffiti=$GRAFFITI
      --debug-level=info
      --suggested-fee-recipient=$FEE_RECIPIENT
      --metrics
      --metrics-address=0.0.0.0
      --metrics-port=5064
    networks:
      - gnosis_net
    ports:
      - 5064:5064/tcp
    volumes:
      - /home/$USER/gnosis/consensus/validators:/data/validators
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    logging:
      driver: "local"
```

### Issue 3. Issues with setting up Web3Signer for DAppNode
Currently, the Web3signer Gnosis is not available in the DAppStore, and this causes issue with the staking setup process as users are not able to load they keys and start validating. Setting up validators from the Stakers UI page yields the same result, whereby the UI gets stucked in the `Setting new staker configuration` page. 