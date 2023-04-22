# Issues relating to Validator Node Setup

This file will be used to track all current issues regarding setting up a validator node for Gnosis Chain.

### Issue 1: Mistake within the Official Gnosis Chain documentation site.
For users looking to setup their own dedicated node to start validating for Gnosis Chain, the instruction for `docker-compose.yml` in [here](https://docs.gnosischain.com/node/guide/validator/run/lighthouse) is incorrect. At line 20, where the data directory flag `--datadir` is set to `/data/validators`, it should be set to `/data` instead. The correct settings for the `validator` container in `docker-compose.yml` file should be as follows:
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

### Issue 2: DAppNode commands not working after installation
For users running DAppNode on cloud services (Digital Ocean, Google Cloud, etc), once DAppNode has been successfully started, DAppNode commands such as `dappnode_help` or `dappnode_connect` shows `command not found` in the command line. This requires users to setup the DAppNode aliases in the later step:
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

This step was not reflected in the official documents, hence users will not be able to use commands to access their DAppNode if their aliases were not set up.
