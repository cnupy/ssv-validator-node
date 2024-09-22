# SSV Node

This repo contains the docker-compose files needed to run an SSV distributed validator node.

It contains:

- Ethereum Execution client (Nethermind)
- Ethereum Consensus client (Lighthouse)
- SSV Distributed Validator client
- MEV-Boost client
- SSV DKG service
- Grafana monitoring service
- Prometheus metrics collection service

# Getting started

A guide on how to get started using this repo to run Lido CSM can be found [here](https://github.com/cnupy/lido-csm-ssv-dvt-guide).

## Hardware & system requirements

 - CPU: Quad-core
 - RAM: 16GB 
 - Storage: 512GB NVME SSD (For mainnet at least 2TB)

 ## Starting the node

 To start the node first clone this repo:

```
git clone https://github.com/cnupy/ssv-validator-node.git
```

Make sure your user has the `docker` role. If not you can use this command to add it:

```
sudo usermod -a -G docker $USER
```

Create the `.env` configuration file:

```
cd ssv-validator-node
cp .env.sample .env
```

To generate the operator key:

```
mkdir secrets
tr -dc 'A-Za-z0-9' </dev/urandom | head -c 16 >> secrets/password
touch secrets/encrypted_private_key.json
docker compose run --rm ssv-node \
/go/bin/ssvnode \
generate-operator-keys \
-p password
```

To view the operator public key:

```
cat secrets/encrypted_private_key.json | jq .pubKey
```

Before starting the node open the `.env` file and set the `NETWORK` and `SSV_OPERATOR_ID` variables.

Start the node using this command:

```
docker compose up -d
```