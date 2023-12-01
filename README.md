# Allo Protocol Indexer

The Allo Protocol Indexer is a tool that indexes blockchain events generated by [Allo contracts](https://github.com/Allo-Protocol/contracts) and serves the data over HTTP in JSON format. The data is organized in a specific structure that enables easy access to different parts of the protocol. The indexer is built using [Chainsauce](https://github.com/boudra/chainsauce) and is designed to work with any EVM-compatible chain.

The indexer data is used by [Grants Stack](https://github.com/gitcoinco/grants-stack) to show stats and allocate matching funds.

# Indexed Data

Access indexed data through this URL: https://grants-stack-indexer.gitcoin.co/data/

The indexed data follows this structure:

```
/{chainId}/rounds.json
/{chainId}/prices.json
/{chainId}/projects.json
/{chainId}/rounds/{roundId}/projects.json
/{chainId}/rounds/{roundId}/projects/{projectId}/votes.json
/{chainId}/rounds/{roundId}/projects/{projectId}/contributors.json
/{chainId}/rounds/{roundId}/applications.json
/{chainId}/rounds/{roundId}/applications/{applicationIndex}/votes.json
/{chainId}/rounds/{roundId}/votes.json
/{chainId}/rounds/{roundId}/contributors.json
```

Indexed chains are defined in [config.ts](src/config.ts).

## How to run?

### Production:

```bash
npm install
npm run build
npm run start
```

### Development:

**Make sure you set the environment variables before running, find a `.env` template in `.env.example`**

```bash
npm install
npm run dev
```

#### Indexing

Indexed JSON data is found in the `.var/storage/chainData` directory by default.

To only index data without tracking new events nor starting a server, provide the `--run-once` option:

```
npm run start -- --chains=mainnet --run-once
```

## Deployment

We deploy the Docker image to Fly, check [fly.production.toml](https://github.com/gitcoinco/allo-indexer/blob/main/fly.production.toml) and [Dockerfile](https://github.com/gitcoinco/allo-indexer/blob/main/Dockerfile) files for more details.

**Notes about the deployment**

- `main` continuously deploys to https://indexer-staging.fly.dev/
- `release` continuously deploys to https://indexer-grants-stack.gitcoin.co/data/
- On deploy the data directory is always cleared and everything is reindexed, we only persist the cache
- Find the data directory at `/mnt/indexer/data` and the cache directory at `/mnt/indexer/cache`
