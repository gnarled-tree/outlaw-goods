# Solana Coin
## Creating an spl token using the umi library
### Requirement
- Typescript
- UMI – A Solana Framework for JavaScript clients.
- ts-node
- Solana env
### Creating an empty directory, initializing and setting up an empty TypeScript project
```
mkdir ts-token-solana  && cd ts-token-solana && npm init -y && tsc --init && mkdir src && touch src/main.ts
```
### Install packages
```
npm install \
  @metaplex-foundation/umi \
  @metaplex-foundation/umi-bundle-defaults \
  @metaplex-foundation/mpl-token-metadata \
  @solana/web3.js \ 
  @types/node \
```
### 
