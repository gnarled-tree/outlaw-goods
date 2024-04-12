# Solana Coin
## Creating an spl token using the umi library
### Requirements
- Typescript
- UMI – A Solana Framework for JavaScript clients.
- ts-node
- (Rust & Cargo package manager)[https://www.rust-lang.org/]
- Solana env
### Create Solana Env
Assuming Rust is installed
```
rustc --version
sh -c  "$(curl -sSfL https://release.solana.com/stable/install)"
solana --version
```
### Generate a keypair (Public key & Private Key)
Use devnet, mainnet-beta or testnet
```
solana-keygen --version
solana-keygen new
solana config get
solana config set --url devnet
```
### Install (Anchor Virtual Machine - Sealevel runtimew network)[https://www.anchor-lang.com/]
Use a (Solana playground)[https://beta.solpg.io/]
```
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm --version
avm install latest
avm use latest
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
