This README documents the Docker Compose setup for the Pocket Network supplier relay and miner services.

## Architecture

Based on [` pocket-relay-miner`](https://github.com/pokt-network/pocket-relay-miner/tree/main), this containerized environment provides:

- **Relayer**: Listens to Pocket Network requests and processes communications
- **Miner**: Oversees payment requests and routing decisions
- **Redis**: Handles inter-process communication

## Quick Start

```sh
docker compose up
```

This initializes the relay and miner services and begins listening for Pocket Network connections.

## Configuration

### Miner Configuration

The file `config/miner-config.yaml` contains many configuration, but most importantly it allows you to configure network targets. By default it targets testnet, but you might want to use mainnet:

```yaml
pocket_node:
  query_node_rpc_url: "https://sauron-rpc.infra.pocket.network"
  query_node_grpc_url: "sauron-grpc.infra.pocket.network:443"
  grpc_insecure: false
```

Also, here you define applications that pay your supplier via `known_applications` (this is optional but helps by doing a warm-start of their sessions):

```yaml
known_applications:
  - "pokt1wvn4a8kj4mfnq0cjakadskxwr2zkev35psjxh9"
```

### Relayer Configuration

The `config/relayer-config.yaml` manages service routing and protocol handling.

```yaml
services:
  text-generation:
    validation_mode: eager
    timeout_profile: streaming
    max_body_size_bytes: 10485760
    default_backend: rest
    backends:
      rest:
        url: "http://poncho-sidecar:8000"
```

Configure backend endpoints for your services (In this example the `http://poncho-sidecar:8000` reflects your local vLLM instance). Multiple services can be added when multiple suppliers are staked.

See [` pocket-relay-miner`](https://github.com/pokt-network/pocket-relay-miner/tree/main) documentation for complete configuration reference.

### Supplier Keys

The `config/supplier-keys.yaml` file contains private keys. Add additional supplier keys as needed.

- enforces adherence authentication
- associates relayer responses with stake holder
- requires strict security handling
