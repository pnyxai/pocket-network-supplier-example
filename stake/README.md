This guide demonstrates creating a service in the Pocket Network, staking a supplier to provide that service, and launching an application to consume POKT by requesting data.

Prerequisites:
- [`pocketd`](https://dev.poktroll.com/explore/account_management/pocketd_cli) CLI client
- This guide uses testnet. Production usage requires removing `--chain-id` and updating endpoints (available in [` pocket-network-resources`](https://github.com/pokt-network/pocket-network-resources))

## Initialize Three Accounts

```sh
pocketd keys add my-service-owner
# pokt1r9k8aga3s4ruzjw8du0fwgxpxc8hyynrds2jkc

pocketd keys add my-application
# pokt1wvn4a8kj4mfnq0cjakadskxwr2zkev35psjxh9

pocketd keys add my-supplier
# pokt19a3t4yunp0dlpfjrp7qwnzwlrzd5fzs2gjaaaj
```

Fund accounts via [faucet](https://faucet.beta.testnet.pokt.network/pokt/).

## Register Service

Example: `text-generation` service (defined with 300000 CUs per relay). Usage in mainnet requires removing `--chain-id`.

```sh
pocketd tx service add-service \
    "text-generation" \
    "machine learning task of generating text given another text - Large language models of medium size are included here" \
    300000 \
    --fees 1upokt \
    --from my-service-owner \
    --node https://sauron-rpc.beta.infra.pocket.network \
    --chain-id pocket-lego-testnet
```

Transaction hash: [`E2DA95E96899915AAB5FBB0DB7511AE14B75A3644106348821E7E8AD5046A304`](https://beta.poktscan.com/tx/E2DA95E96899915AAB5FBB0DB7511AE14B75A3644106348821E7E8AD5046A304)

## Stake Supplier Entity

This command uses `supplier_stake_config.yaml` to define the target service, stake amount, and revenue share configuration. 
This file also defines the `publicly_exposed_url` of your service, make sure this points to a url that you own.

```sh
pocketd tx supplier stake-supplier \
  --config=supplier_stake_config.yaml \
  --from=my-supplier \
  --gas=auto \
  --gas-prices=1upokt \
  --gas-adjustment=1.5 \
  --node https://sauron-rpc.beta.infra.pocket.network \
  --chain-id pocket-lego-testnet
```

Transaction hash: [`BF5C145FD8F84D97CC61EFABEE6126890C640A61C97A3547F44F1B324E7BAC78`](https://beta.poktscan.com/tx/BF5C145FD8F84D97CC61EFABEE6126890C640A61C97A3547F44F1B324E7BAC78)

## Stake Application Entity

Configuration file defines spending limits and target service. Simpler process than supplier stake.

```sh
pocketd tx application stake-application \
  --config=application_stake_config.yaml \
  --from=my-application \
  --gas=auto \
  --gas-prices=1upokt \
  --gas-adjustment=1.5 \
  --node https://sauron-rpc.beta.infra.pocket.network \
  --chain-id pocket-lego-testnet
```

Transaction hash: [`A44E893CFFBCF72866F72F6BB8B4EA596B20450D91D5A769E18076A8A41CAC18`](https://beta.poktscan.com/tx/A44E893CFFBCF72866F72F6BB8B4EA596B20450D91D5A769E18076A8A41CAC18)

Wait up to 30 minutes for service rotation to complete.
