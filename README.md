# HelloWorld + Counter Smart Contract
This guide walks you through deploying a simple smart contract to Eclipse Testnet.

## Prerequisites
You'll have to do a few things before you can deploy your smart contract to Eclipse Testnet.

1. Install [metamask](https://metamask.io/download/)
2. Add Eclipse [Testnet/Devnet wallet](https://docs.eclipse.xyz/developers/wallet/testnet-and-devnet-wallets) to your browser 

## Install Dependencies
Install Rust, and its package manager Cargo.


```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

You can check if the installation was successful by running the following commands:

```
rustc --version
cargo --version
```

ii. We'll also have to install Node.js and npm using, which will be used later in this guide.

Visit the [official Node.js download page](https://arc.net/l/quote/lhmguvbu) and download the installation binary for your system. Versions recommended are 14.0 and above. If you have trouble with this, you might consider using Homebrew or some other package manager.

Note that npm is bundled with the Node.js installation, so you don't have to install it separately.

iii. Now let's install the Solana CLI. This allows you to interact with Solana clusters (Eclipse Testnet in this case).

```
~ sudo sh -c "$(curl -sSfL https://release.anza.xyz/v1.18.22/install)"
```

iv. Instead of setting your Solana CLI to a local cluster, set it to Eclipse Testnet with the following command:

```
solana config set --url https://testnet.dev2.eclipsenetwork.xyz
```

v. If this is your first time using the Solana CLI, you will need to generate a new keypair:

```
solana-keygen new
```

This will generate a new key pair and save it to your local machine. You will need this key pair to sign transactions to deploy your smart contract to Eclipse Testnet.

### Getting Sepolia ETH Tokens
You will need an Ethereum wallet such as Metamask to claim and bridge Sepolia ETH. As a user, you can use Sepolia ETH to explore the Eclipse testnet and dApps deployed on the network. Sepolia ETH is not meant to be traded, and is only used to test applications. Sepolia ETH can be claimed from a number of faucets: Alchemy, QuickNode, and Infura. 

Here are instructions on how to claim Sepolia ETH on [Chainlink Sepolia ETH Faucet](https://faucets.chain.link/sepolia). 

1. Tick 'Ethereum Sepolia' and click 'Continue'

2. Enter your wallet in the provided box and click "Get tokens".

### Deposit Sepolia ETH to Eclipse Testnet
Once you've acquired Sepolia ETH, this script allows you to deposit Sepolia ETH to the Eclipse test network.

#### Wallet Setup

##### Getting Your Ethereum Private Key
Once you have [Sepolia ETH](https://faucets.chain.link/sepolia), copy your Ethereum wallet address and your private private key for later use by navigating to "Account details" and clicking "Show private key".

##### Getting Your Eclipse Public Address
Copy the previously generated "Solana" address using the Solana CLI: ```solana-keygen new --no-outfile``` or ```solana-keygen new --outfile my-wallet.json```.

The public key in the output should resemble something like this following: ```6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8```

### Create a Deposit
Obtain and set up the deposit.js script: https://github.com/Eclipse-Laboratories-Inc/testnet-deposit

You'll need to install the dependencies with ```npm```.


Finally, execute the script:

1. ```cd eclipse-deposit```
2. ```npm install```
3. Change name of ``"example-private-key.txt"`` to ``"private-key.txt"`` and paste your Metamask wallet's private key in the file.
4. Run the CLI tool with the necessary options:
    ```bash
    node bin/cli.js -k <path_to_private_key> -d <solana_destination_address> -a <amount_in_ether> --mainnet|--sepolia 
    ```

   For example:

   **Sepolia Testnet Deposit:**
    ```bash
    node bin/cli.js -k private-key.txt -d 6g8wB6cJbodeYaEb5aD9QYqhdxiS8igfcHpz36oHY7p8 -a 0.004 --sepolia
    ```

   - The `-k, --key-file` option specifies the path to the Ethereum private key file.
   - The `-d, --destination` option specifies the Solana destination address on the rollup (base58 encoded).
   - The `-a, --amount` option specifies the amount of Ether to deposit.
   - Use `--mainnet` or `--sepolia` to select the network. The tool will use different contract addresses depending on the network.
   - The `-r, --rpc-url` option is optional and allows overriding the default JSON RPC URL.

A successful command example:

```Output
Transaction hash: 0x15041e2f67e821f8a76671aa282f3b7994d81b2c0b3434d67880ef88c9884186
```
You can check the transaction hash on [Sepolia testnet](https://sepolia.etherscan.io/).
You can verify your testnet account balance using the [Eclipse Explorer](https://explorer.dev.eclipsenetwork.xyz/?cluster=testnet).

## Deploying the Smart Contract​
Now that we've set up our environment, we can deploy our smart contract to Eclipse Testnet. Let's make sure that everything is installed properly by running a local Solana cluster.

```Copy
solana-test-validator
```
 
> [i] In case the validator fails to start, restart your computer and run the following command:
>```
>sudo $(command -v solana-sys-tuner) --user $(whoami) > sys-tuner.log 2>&1 &
>```
>

We don't need that local Solana cluster but we're using it to check that everything is installed properly. Next, we'll clone the Solana Hello World repository and install the dependencies.

```Copy
git clone https://github.com/solana-labs/example-helloworld
cd example-helloworld
npm install
```
Next, 
1. install "Even Better TOML" extension
2. Locate and open the 'Cargo.toml' file at ```src/program-rust/Cargo.toml```
3. Copy the toml config from the ```'example-Cargo.toml'``` file from the root of this repository and paste it in ```src/program-rust/Cargo.toml```

We build the smart contract:

```Copy
npm run build:program-rust
```

Finally, we can deploy the smart contract to Eclipse Testnet:

```Copy
solana program deploy dist/program/helloworld.so
```

Expected output
```
Program Id: Butt9GJQQPXX6ih65e1Z11R4QyQ8YfAEHit7VmkrLs8v
```
>[IMPORTANT]
> It might prompt with
>```
>Error: Account 9BEnr8WzpJ9kaEJQyG65iE6357hcXG8EpDEGgbLi2AZ1 has insufficient funds for spend (0.00331058 SOL) + fee (0.0000026 SOL)
>```
>
>Nothing to worry about, just ask Anmol to send you some Eclipse testnet funds or do it yourself (hint: the command is somewhere up there :eyes: )!

We can run the JavaScript client and confirm whether the smart contract was deployed successfully:

```Copy
npm run start
```

The output should be something like this:

```
Let's say hello to a Solana account...
Connection to cluster established: http://127.0.0.1:8899 { 'feature-set': 2045430982, 'solana-core': '1.7.8' }
Using account AiT1QgeYaK86Lf9kudqKthQPCWwpG8vFA1bAAioBoF4X containing 0.00141872 SOL to pay for fees
Using program Dro9uk45fxMcKWGb1eWALujbTssh6DW8mb4x8x3Eq5h6
Creating account 8MBmHtJvxpKdYhdw6yPpedp6X6y2U9dCpdYaZJdmwV3A to say hello to
Saying hello to 8MBmHtJvxpKdYhdw6yPpedp6X6y2U9dCpdYaZJdmwV3A
8MBmHtJvxpKdYhdw6yPpedp6X6y2U9dCpdYaZJdmwV3A has been greeted 1 times
Success
```

>Not seeing the expected output?
> - Make sure you've run all the commands in the previous steps.
> - Inspect the program logs by running ```solana logs``` to see why the program failed.
>An example of what you might find is given below.
>```Copy
>Signature: 4pya5iyvNfAZj9sVWHzByrxdKB84uA5sCxLceBwr9UyuETX2QwnKg56MgBKWSM4breVRzHmpb1EZQXFPPmJnEtsJ
>Status: Error processing Instruction 0: Program failed to complete
>Log Messages:
>  Program G5bbS1ipWzqQhekkiCLn6u7Y1jJdnGK85ceSYLx2kKbA invoke [1]
>  Program log: Hello World Rust program entrypoint
>  Program G5bbS1ipWzqQhekkiCLn6u7Y1jJdnGK85ceSYLx2kKbA consumed 200000 of 200000 compute units
>Program failed to complete: exceeded maximum number of instructions allowed (200000) at instruction #334
>  Program G5bbS1ipWzqQhekkiCLn6u7Y1jJdnGK85ceSYLx2kKbA failed: Program failed to complete
>  ```

### For queries
Reachout to Anmol on [Telegram](https://t.me/anmolaroraeth) or on the Eclipse Discord

