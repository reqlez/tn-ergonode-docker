# Easy Testnet Ergo Node Setup

Prerequisites: make sure git, nano?, docker, and docker compose plugin are installed.

Clone this repo:

```console
git clone https://github.com/reqlez/tn-ergonode-docker.git && cd tn-ergonode-docker
```

Create Docker network / volume:

```console
docker network create tn-ergo-node
```

```console
docker volume create tn_ergo_node
```

Build + start node container temporarily to generate API Key Hash:

```console
docker compose build --no-cache
docker compose up -d
```

Generate an 'apiKeyHash' for node, ex:

```console
curl -X POST "http://localhost:9052/utils/hash/blake2b" -H "Content-Type: application/json" -d "\"YOUR_API_KEY\""
```

Stop node container:

```console
docker compose down
```

Uncomment / set settings like apiKeyHash:

- `nano config/ergo.conf`

Start node container and initialize wallet:

```console
docker compose up -d
```

```console
curl -X POST "http://localhost:9052/wallet/init" -H "api_key: YOUR_API_KEY" -H "Content-Type: application/json" -d "{\"pass\":\"YOUR_WALLET_PASS\"}"
```

Get wallet address so you can use it for future reference:

```console
curl -X GET "http://localhost:9052/wallet/addresses" -H "api_key: YOUR_API_KEY"
```

Visit https://tn-faucet.ergohost.io and get some test ERG for your wallet address.

Wait for node to sync, you can monitor progress under: http://ip.of.your.node:9052/panel

You can also check / use node API under: http://ip.of.your.node:9052/swagger

For troubleshooting, check logs via:

```console
docker compose logs -f
```

