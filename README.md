# Pocket Network AI Supplier Example

![Join the network...](./assets/banner.png)

A complete example for deploying a service supplier on the Pocket Network. This repository demonstrates how to set up a text generation service (LLM workloads) and stake it on the network to earn POKT.

## Overview

This project provides end-to-end infrastructure for:

1. **Staking a Service** – Register a service on Pocket Network (example with `text-generation` in testnet)
2. **Running a Supplier** – Deploy a relay and miner to process incoming requests
3. **Hosting a Backend** – Deploy a vLLM instance behind Poncho sidecar for inference
4. **Testing** – Verify the complete setup with test relays

## Project Structure

### [`stake/`](./stake)
Pocket Network on-chain configuration and staking setup.

- Register a service on the network
- Stake a supplier entity with a public endpoint
- Stake an application entity to test the service
- Includes example configuration files and transaction references

### [`relay-and-miner/`](./relay-and-miner)
The core relay and miner infrastructure for processing network requests.

- **Relayer**: Listens to Pocket Network requests and routes them to backends
- **Miner**: Manages payment validation and routing decisions
- **Redis**: Enables inter-process communication
- Includes Docker Compose setup and comprehensive configuration guides

### [`backend/`](./backend)
vLLM deployment with Poncho sidecar for text generation inference.

- Containerized vLLM server for language model hosting
- Poncho sidecar as the entrypoint for Pocket Network requests
- Configured to work seamlessly with the relay-and-miner infrastructure
- Includes environment setup and testing documentation

## Quick Start

1. **Set up staking** – Follow [`stake/README.md`](./stake) to register and stake your service
2. **Run relay-and-miner** – Deploy with `cd relay-and-miner && docker compose up`
3. **Deploy backend** – Follow [`backend/README.md`](./backend) and run `docker compose up`
4. **Test the service** – Send a relay through the Pocket Network

For detailed instructions, see the README in each directory.

## License

See [LICENSE](./LICENSE) file for details.
