# Backend Deployment

Deploy a vLLM instance behind [Poncho](https://github.com/pnyxai/pokt-ml-sidecar) using Docker Compose. This setup includes:

- **vLLM Server**: Hosts the language model
- **Poncho Instance**: Serves as the entrypoint for Pocket Network requests

## Prerequisites

> **⚠️ Important**: Ensure the `relay-and-miner` service is running before deploying this backend, as it depends on it.

## Configuration

Create a `.env` file in this directory following the `.env.sample` template. At minimum, configure:

```env
# Your HuggingFace Token
HF_TOKEN=hf_zzzzzzz

# Host machine path for model storage
MODELS_PATH=/your/host/huggingface_hub/
```

## Deployment

Start the services with:

```sh
docker compose up
```

## Testing

Test the deployment by sending a relay through the Pocket Network:

```sh
pocketd relayminer relay \
    --app=pokt1wvn4a8kj4mfnq0cjakadskxwr2zkev35psjxh9 \
    --supplier=pokt19a3t4yunp0dlpfjrp7qwnzwlrzd5fzs2gjaaaj \
    --node=https://sauron-rpc.beta.infra.pocket.network \
    --grpc-addr=sauron-grpc.beta.infra.pocket.network:443 \
    --grpc-insecure=false \
    --payload='{"prompt":"Here is a very long and colorful story of a relay that kept timing out while passing through several nodes:\n\n","max_tokens":250,"model":"pocket_network","stream":false}' \
    --supplier-public-endpoint-override=http://localhost:8080/v1/completions \
    --chain-id pocket-lego-testnet \
    | jq
```

### Configuration Notes

- **`--app`**: Replace with your own app address
- **`--supplier`**: Replace with the supplier address from your `relay-and-miner` deployment
- **`--supplier-public-endpoint-override`**: Overrides the staked URL for testing (see [supplier stake config](../stake/supplier_stake_config.yaml))

For production relays through Pocket Network, omit the `--supplier-public-endpoint-override` parameter and ensure you have a valid staked URL.