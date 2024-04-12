# [Calyptus](https://calyptus.co) - Solana Coin
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
### Install [Anchor Virtual Machine](https://book.anchor-lang.com)
```
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm --version
avm install latest
avm use latest
```
### Creating an SPL token using the spl-token cli
Run these commands in a terminal:
```
cargo install spl-token-cli
spl-token --version
solana airdrop 10
```
### Create token
Run these commands in a terminal (Replace the account details with your own):
```
~$ spl-token create-token --decimals 56
Creating token 2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n under program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf2Ss625VQ5DA

Address:  2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n
Decimals:  56

Signature: 5JGxACrLz4sMcbpFdkerUQ1VEExfPgBaQUZB2Vw5PRMBiCcFJXLnjVXcZ21PHqfbtNGn2yD2nWW2FUEvpZv44bXo
```
### Create account
```
~$ spl-token create-account 2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n

Creating account 6zSczM5wpcSpWZSnJxSS2qxUhsR5mstXviSyQCPkWmMX

Signature: 2EtovToTq2WhZynMaWKHQY1o6LAR1LNncm6iiG6Z4voeaWJvniXW2NbhJoocePuZy12avLuN62VS2FydtmDFZYHj
```
### Mint Token
```
$ spl-token mint 2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n 2207520
Minting 2207520 tokens
  Token: 2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n
  Recipient: 6zSczM5wpcSpWZSnJxSS2qxUhsR5mstXviSyQCPkWmMX

Signature: 5SPD2dWNfYxQGdP5Ptb2BZr7pK2yXA4kjPQzpoEtLgvXULfmNKHr2LaWhNWFdAWyBJcAqydV4vBQqWBv6n7gihh7
```
### Check supply and account details 
```
$ spl-token supply 2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n

$ spl-token accounts

Token                                         Balance                               
------------------------------------------------------------------------------------
2KA5S4awX4UkD6gQ2nWz6PMJmcbG5Rysnh5yVVbWse1n  2207520
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
### System Wallet keypair extracted in src/helpers.ts
```
import { readFileSync } from 'fs';
import { homedir } from 'os';
import { Keypair } from '@solana/web3.js';

const USER_KEYPAIR_PATH = homedir() + "/.config/solana/id.json";
export const userKeypair = Keypair.fromSecretKey(
    Buffer.from(JSON.parse(readFileSync(USER_KEYPAIR_PATH, "utf-8")))
);
```
### Import src/helpers.ts into src/main.ts 
```
import { createUmi } from "@metaplex-foundation/umi-bundle-defaults";
import { userKeypair } from "./helpers";

const umi = createUmi('https://api.devnet.solana.com');
```
### Register keypair for use in transactions
```
import { mplTokenMetadata } from "@metaplex-foundation/mpl-token-metadata";
import { keypairIdentity } from "@metaplex-foundation/umi";
import { createUmi } from "@metaplex-foundation/umi-bundle-defaults";
import { userKeypair } from "./helpers";

const umi = createUmi('https://api.devnet.solana.com');

const keypair = umi.eddsa.createKeypairFromSecretKey(userKeypair.secretKey);

umi.use(keypairIdentity(keypair))
    .use(mplTokenMetadata())
```
### Wrap umi eddsa interface with keypair for web3.js
```
import { mplTokenMetadata } from "@metaplex-foundation/mpl-token-metadata";
import { keypairIdentity } from "@metaplex-foundation/umi";
import { createUmi } from "@metaplex-foundation/umi-bundle-defaults";
import { userKeypair } from "./helpers";

const umi = createUmi('https://api.devnet.solana.com');

const keypair = umi.eddsa.createKeypairFromSecretKey(userKeypair.secretKey);

umi.use(keypairIdentity(keypair))
    .use(mplTokenMetadata())
```
## Minting a fungible token
Three steps
### Create account metadata
```
const metadata = {
    name: "Solana Gold",
    symbol: "GOLDSOL",
    uri: "https://raw.githubusercontent.com/solana-developers/program-examples/new-examples/tokens/tokens/.assets/spl-token.json",
};

const mint = generateSigner(umi);
async function createMetadataDetails() {
    await createV1(umi, {
        mint,
        authority: umi.identity,
        name: metadata.name,
        symbol: metadata.symbol,
        uri: metadata.uri,
        sellerFeeBasisPoints: percentAmount(0),
        decimals: 9,
        tokenStandard: TokenStandard.Fungible,
    }).sendAndConfirm(umi)
}
```
### Mint coins and create mint account if it does not already exist
```
async function mintToken() {
    await mintV1(umi, {
        mint: mint.publicKey,
        authority: umi.identity,
        amount: 10_000,
        tokenOwner: umi.identity.publicKey,
        tokenStandard: TokenStandard.Fungible,
    }).sendAndConfirm(umi)
}
```
### Alternative one step process combining two steps to create a fungible token
```
async function mintToken() {
    await mintV1(umi, {
        mint: mint.publicKey,
        authority: umi.identity,
        amount: 10_000,
        tokenOwner: umi.identity.publicKey,
        tokenStandard: TokenStandard.Fungible,
    }).sendAndConfirm(umi)
}
```
### Tests ommitted 
(...)
### Execute script
```
ts-node main.ts
```
