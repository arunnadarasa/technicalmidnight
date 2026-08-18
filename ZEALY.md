# Zealy: Submit a Technical PR (Protocol / SDK / Examples)

Official PR: https://github.com/midnightntwrk/example-nft-contracts/pull/23

This repository is the working copy of that contribution, pushed here as quest evidence.

## Paste-ready explanation

NFT-ZK in `midnightntwrk/example-nft-contracts` was simulator-only. I added a real Undeployed path: Docker Compose (current midnight-local-dev image tags), midnight-js deploy and `mintAdmin` using the constructor admin keypair (`assert(contractAdmin == deriveAdminPublicKey(getAdminSecret()))` — a non-empty hash is not auth), and Fly.io templates that keep the node 6PN-internal while exposing the IPv4-only proof-server over public HTTPS (Fly 6PN is IPv6-only). CI stays docker-free; 50 unit tests pass including a wrong-admin-key simulator case.

## What changed

1. **Local Undeployed deploy + mint** (`examples/undeployed/`)
   - Genesis wallet seed `…0002`
   - Wait for indexer, proof server, and block height >= 2 (avoids TTL error 171)
   - `mintAdmin` uses the same admin secret the constructor hashed onto the ledger
2. **Fly.io hosted Undeployed** (`examples/undeployed/fly/`, `scripts/fly-bootstrap.sh`)
   - Node: 6PN-internal only
   - Indexer: public HTTPS + WSS
   - Proof: public HTTPS, 2 GB RAM, never auto-stop
3. **Auth lesson from StreetRail** ([zealymidnight#1](https://github.com/arunnadarasa/zealymidnight/issues/1)): simulator test that `mintAdmin` fails when the admin secret does not match the constructor key.

## Reproduce locally

```bash
yarn install
yarn compact
yarn undeployed:up
yarn undeployed:deploy
yarn undeployed:mint
```

Simulator-only: `yarn test` (no Docker).

## Related work

- Fly.io Undeployed stack: https://github.com/arunnadarasa/flymidnight
- NFT contracts on local Undeployed: https://github.com/arunnadarasa/zealymidnight
