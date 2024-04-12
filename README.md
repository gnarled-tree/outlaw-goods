# Solana Coin
## Creating an spl token using the umi library
### Requirements
- Typescript
- UMI – A Solana Framework for JavaScript clients.
- ts-node
- [Rust & Cargo package manager](https://www.rust-lang.org/)
- Solana env
### Create Solana Env
Assuming Rust is installed on a recently up to date version of Debian Linux
Run these commands in a terminal:
```
rustc --version
sh -c  "$(curl -sSfL https://release.solana.com/stable/install)"
solana --version
```
### Generate a keypair (Public key & Private Key)
Use devnet, mainnet-beta or testnet
Run these commands in a terminal:
```
solana-keygen --version
solana-keygen new
solana config get
solana config set --url devnet
```
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
