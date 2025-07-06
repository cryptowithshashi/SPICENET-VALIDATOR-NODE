# SPICNET VALIDATOR NODE GUIDE

Spicenet is a next-generation blockchain network focused on scalability, security, and decentralization. Validators play a vital role in supporting the infrastructure that keeps Spicenet fast, reliable, and censorship-resistant.

### 🌟 About Spicenet
Spicenet is an innovative blockchain project dedicated to building a secure and efficient decentralized network.

- Funding Raised: $3.4 million seed round

- Key Backers: Hack VC, Magnus Capital, and other strategic partners

### By becoming a validator, you contribute directly to the success and growth of the Spicenet ecosystem.

**🔍 What is a Spicenet Validator?**

- Validators are essential participants in the network responsible for:

**📥 Receiving and Verifying Transactions**
- Validators accept batches of transactions, check their correctness, and maintain consensus.

**🌐 Maintaining Network State**
- They ensure the blockchain ledger remains consistent and up to date.

**🕵️‍♂️ Detecting Fraud**
- Validators monitor network activity to identify malicious behavior.

**🛡️ Producing Fraud Proofs**
- If invalid transactions are detected, validators generate proofs to prevent damage to the network.

Your participation strengthens the security and resilience of Spicenet.


## 🛠️ Validator Requirements

### ⚖️ Validator Types Comparison

| Validator Type | Capabilities | Hierarchy Level | Rewards |
| :--- | :--- | :--- | :--- |
| **Non-voting** |  Cannot vote or challenge state, Cannot generate fraud proofs, Behaves like a full node | Below voting validators | Lower rewards due to limited participation |
| **Voting** | Can vote and participate in consensus, Can challenge state and generate fraud proofs | Above non-voting validators | Higher rewards for increased participation |

---

### 🖥️ Hardware Requirements – Non-voting Validators

| Parameter | Minimum Requirement | Recommended Requirement |
| :--- | :--- | :--- |
| RAM | 16GB | 32GB |
| CPU | 8 cores | 16 cores |
| Storage | 500GB | 2TB |
| Bandwidth | 500 Mbps sustained | 1 Gbps sustained |
| GPU | Not required | Not required |
| Stake | TBD (Subsidised by Spicenet Foundation) | TBD (Subsidised by Spicenet Foundation) |

> **Note:** No GPU needed for non-voting validators as Spicenet does not yet optimize GPU workloads.

---

### 🖥️ Hardware Requirements – Voting Validators

| Parameter | Minimum Requirement | Recommended Requirement |
| :--- | :--- | :--- |
| RAM | 32GB | 64GB |
| CPU | 16 cores | 32 cores (or dual 24-core CPUs) |
| Storage | 1TB | 2TB |
| Bandwidth | 500 Mbps sustained | 1 Gbps sustained |
| GPU | Optional – Nvidia 4070 minimum recommended for prover | Optional – Nvidia 4080+ for optimal zkVM proving performance |
| Stake | TBD (Subsidised by Spicenet Foundation) | TBD (Subsidised by Spicenet Foundation) |


# 1 Setting Up Your Environment

## Operating System Setup


- If you haven't already, install Ubuntu 20.04 LTS on your machine.

- Update your system:

```BASH
sudo apt update && sudo apt upgrade -y
```

## Installing Dependencies

- Install necessary packages:

```bash
sudo apt install -y build-essential git curl
sudo apt install -y pkg-config libssl-dev clang postgresql
```

- Install Rust:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

- Add Rust to your path:

```bash
source $HOME/.cargo/env
```

## Setting Up Firewall

- If you're using UFW (Uncomplicated Firewall), allow SSH connections:

```bash
sudo ufw allow ssh
```

We'll open specific ports for Spicenet later in the configuration process.

# 1.2  Installing Spicenet

## Using Binaries

Use our binaries to run the Spicenet validator node, download the latest version at

```bash
https://github.com/pepper-research/testnet-canary/releases
```

# 2.1 Binaries Configuration

Once you have downloaded the required binaries from releases of the repo, we will be configuring it to work in a specific file system structure

- App
   - spicenet-ubuntu-latest
   - configs
     - celestia


`configs` can be found in the testnet repo at the root.

Set up connection to celestia in the file `configs/celestia/rollup.toml` in the fields `celestia_rpc_address` and `celestia_rpc_auth_token`

 Make sure your celestia bridge node runs on the mocha testnet. Refer to celestia's guide on running and maintaining a celestial bridge node - https://docs.celestia.org/how-to-guides/bridge-node

 ### Run the binary with


```bash
chmod +x spicenet-ubuntu-latest
./spicenet-ubuntu-latest
```

# 2.2 Docker Configuration

We have a pre-built docker image hosted on dockerhub. Pull the image with

```bash
docker pull spicenet/node:v0.1.0
```
The container needs volume on the host system to work uninterrupted, so the file structure should look like this 

- App

  - configs

      - celestia


`celestia_rollup_config.toml` and `test-data` can be found in the validator repo [here](https://github.com/pepper-research/testnet-canary)
 at the root.


### Run the docker container with

```bash
docker run -it -v $(pwd):/app spicenet/node:v0.1.0 --da-layer celestia --rollup-config-path ./celestia_rollup_config.toml --genesis-config-dir ./test-data/genesis/celestia
```

# 2.3 Security Best Practices

## Key Management

- Generate a new key pair for your validator:

```bash
./target/debug/sov-cli keys generate
```

- Securely store your private key. Never share it or store it in plain text on your server.

- Use hardware security modules (HSMs) for additional security if available.

## Node Security

- Keep your operating system and all software up to date.
- Use a firewall to restrict incoming connections:

```bash
sudo ufw allow 12346/tcp
sudo ufw enable
```

- Disable root SSH access and use key-based authentication.

## Network Security

- Use a VPN or private network for communication between your nodes if running multiple.

- Implement DDoS protection if your node is publicly accessible.

- Regularly monitor your node for unusual activity or performance issues.


# 3.1 Starting Your Node

To start your Spicenet validator node:

- Navigate to your Spicenet directory:

```bash
cd /path/to/spicenet
```

- Ensure that a Celestia node is running, as Spicenet uses Celestia for data availability. If you haven't started a Celestia node, you can do so with:

```bash
make start
```

This command starts a Celestia node as a container and creates the necessary configurations for your Spicenet node to connect to it.

- Start the Spicenet node:

```bash
cargo run
```
When your node starts running, you'll see output similar to the following:

```bash
2023-10-02T15:30:10.123Z INFO  sov_rollup::runner] Starting Spicenet node...
2023-10-02T15:30:10.234Z INFO  sov_rollup::runner] Connecting to DA layer...
2023-10-02T15:30:10.345Z INFO  sov_rollup::runner] Successfully connected to DA layer
2023-10-02T15:30:10.456Z INFO  sov_rollup::runner] Initializing state...
2023-10-02T15:30:10.567Z INFO  sov_rollup::runner] State initialized. Current state root: 0x1a2b3c...
2023-10-02T15:30:10.678Z INFO  sov_rollup::runner] Node is ready to receive and process transaction batches
2023-10-02T15:30:15.789Z INFO  sov_rollup::runner] Received batch of 5 transactions
2023-10-02T15:30:15.890Z INFO  sov_rollup::runner] Processing transactions...
2023-10-02T15:30:16.001Z INFO  sov_rollup::runner] Batch processed. New state root: 0x4d5e6f...
2023-10-02T15:30:16.112Z INFO  sov_rollup::runner] Waiting for next batch...
```

**This output indicates that:**

- Your Spicenet node has started successfully

- It has connected to the Celestia Data Availability (DA) layer

- The initial Spicenet state has been set up

- The node is processing incoming transaction batches received from Celestia

- After processing each batch, a new Spicenet state root is calculated

**Your Spicenet node will continue to run, receiving transaction batches from Celestia and processing them. The output will show ongoing activity, including:**

- Receipt of new transaction batches from Celestia

- Processing of these batches on Spicenet

- Updates to the Spicenet state root after each batch is processed

- Any potential errors or warnings that might occur during the operation

*Keep this terminal window open to maintain your Spicenet node's operation. You can open new terminal windows for interacting with your node or performing other tasks.*


# 3.2 Monitoring Your Node


Understanding Node Output- Your node will output logs that include:

- Current slot number

- Number of blobs processed

- State root hash

- Any errors or warnings

Example output:

```bash
[2023-06-15T10:30:15Z INFO  sov_rollup::runner] Slot 1234: Processed 5 blobs. New state root: 0x1a2b...
```
## Setting Up Monitoring Tools

- Use Prometheus for metrics collection:

  - Install Prometheus

  - Configure it to scrape metrics from your Spicenet node

- Set up Grafana for visualization:

  - Install Grafana

  - Create dashboards for key metrics like processed blobs, latency, and peer count


# 3.3 Performing Sanity Checks


After setting up your node, it's crucial to perform some sanity checks to ensure everything is working correctly. Let's go through the process of creating a token and checking balances.

## Creating a Test Tok (costs gas)

- Open a new terminal tab and submit your first transaction by creating a token:

```bash
make test-create-token
```

- Once the batch is submitted, you should see output containing the transaction hash. For example:

```bash
Your batch was submitted to the sequencer for publication. Response: "Submitted 1 transaction"
0: 0xfce2381221722b8114ba41a632c44f54384d0a31f332a64f7cbc3f667841d7f0
```

- Use this transaction hash to query the REST API endpoint and fetch events belonging to the transaction:

```bash
curl -sS http://127.0.0.1:12346/ledger/txs/0xfce2381221722b8114ba41a632c44f54384d0a31f332a64f7cbc3f667841d7f0/events | jq
```

Replace the transaction hash with the one from your output.

- You should see output similar to:

```bash
{
  "data": [
    {
      "type": "event",
      "number": 0,
      "key": "token_created",
      "value": {
        "token-created": {
          "token_name": "sov-test-token",
          "coins": {
            "amount": 1000000,
            "token_id": "token_1zdwj8thgev2u3yyrrlekmvtsz4av4tp3m7dm5mx5peejnesga27ss0lusz"
          },
          "minter": {
            "User": "sov15vspj48hpttzyvxu8kzq5klhvaczcpyxn6z6k0hwpwtzs4a6wkvqwr57gc"
          },
          "authorized_minters": [
            {
              "user": "sov1l6n2cku82yfqld30lanm2nfw43n2auc8clw7r5u5m6s7p8jrm4zqrr8r94"
            },
            {
              "user": "sov15vspj48hpttzyvxu8kzq5klhvaczcpyxn6z6k0hwpwtzs4a6wkvqwr57gc"
            }
          ]
        }
      },
      "module": {
        "type": "moduleRef",
        "name": "Bank"
      }
    }
  ],
  "meta": {}
}
```

Note the `token_id` in the output. You'll need this for checking balances.

## Checking Balances (Gasless)

- In a new terminal tab, run an RPC query to check the balance of your address:

```bash
curl -Ss http://127.0.0.1:12346/modules/bank/tokens/TOKEN_ID/balances/YOUR_ADDRESS | jq -c -M
```

Replace `TOKEN_ID` with the actual token ID and `ADDRESS` with the address you want to check.

- You should see a response similar to:

```bash
{"data":{"coins": {"amount": 1000000,"token_id": "token_1zdwj8thgev2u3yyrrlekmvtsz4av4tp3m7dm5mx5peejnesga27ss0lusz"}},"meta":{}}
```

this confirms that your address has received the newly created tokens.

By completing these sanity checks, you've verified that your node can create tokens, submit transactions, and query balances. This indicates that your Spicenet validator node is set up correctly and functioning as expected.

## Submitting Test Transactions

- Generate a test transaction:

```bash
./target/debug/sov-cli transactions import from-file bank --max-fee 100000000 --path ./examples/test-data/requests/transfer.json
```

- Submit the transaction:

```bash
./target/debug/sov-cli node submit-batch --wait-for-processing by-address YOUR_ADDRESS
```

Replace `YOUR_ADDRESS` with your actual address.

- Verify the transaction by checking balances again.

By following these steps, you should now have a configured and running Spicenet validator node. Remember to regularly check for updates and maintain your node's security and performance.


# 4.1 Understanding Transaction Structure

In Spicenet, transactions are structured as part of the `CallMessage` enum. This structure defines the various types of operations that can be performed within the Spicenet ecosystem. Let's break down the `CallMessage` enum for the Bank module:


```bash
use sov_bank::CallMessage::Transfer;
use sov_bank::Coins;
use sov_bank::TokenId;
use sov_bank::Amount;

pub enum CallMessage<S: sov_modules_api::Spec> {
    CreateToken {
        salt: u64,
        token_name: String,
        initial_balance: Amount,
        mint_to_address: S::Address,
        authorized_minters: Vec<S::Address>,
    },
    Transfer {
        to: S::Address,
        coins: Coins,
    },
    Burn {
        coins: Coins,
    },
    Mint {
        coins: Coins,
        mint_to_address: S::Address,
    },
    Freeze {
        token_id: TokenId,
    },
}
```


Each variant of this enum represents a different type of transaction:

- `CreateToken`: Used to create a new token on the network.

- `Transfer`: Moves tokens from one address to another.

- `Burn`: Destroys a specified amount of tokens.

- `Mint`: Creates new tokens and assigns them to an address.

- `Freeze`: Prevents further minting or burning of a specific token.

# 4.2 Generating Transactions

To generate a transaction, you'll use the `sov-cli` tool. Let's go through the process of creating a transfer transaction:

- Create a JSON file named `transfer.json` with the following content:

```bash
{
  "transfer": {
    "to": "sov1zgfpyysjzgfpyysjzgfpyysjzgfpyysjzgfpyysjzgfpyysjzgfqve8h6h",
    "coins": {
      "amount": 200,
      "token_id": "token_1rwrh8gn2py0dl4vv65twgctmlwck6esm2as9dftumcw89kqqn3nqrduss6"
    }
  }
}
```

This JSON represents a transfer of 200 tokens to the specified address.

- Use `sov-cli` to import the transaction:

```bash
./target/debug/sov-cli transactions import from-file bank --max-fee 100000000 --path ./transfer.json
```

This command imports the transaction into the local transaction pool. The `--max-fee` parameter sets the maximum gas fee you're willing to pay for this transaction.

# 4.3 Submitting Transactions

Once you've generated a transaction, you need to submit it to the network. Here's how:

```bash
./target/debug/sov-cli node submit-batch --wait-for-processing by-address YOUR_ADDRESS
```

Replace `YOUR_ADDRESS` with your actual Spicenet address.

This command submits all pending transactions in your local pool to the network. The `--wait-for-processing` flag ensures that the command doesn't return until the transactions have been processed by the network.

After submitting, you should see output similar to:

```bash
Submitted batch of 1 transactions.
Transaction hash: 0x1234...5678
Waiting for processing...
Transaction processed successfully.
```

# 4.4 Verifying Transactions


As a validator, one of your key responsibilities is to verify incoming transaction batches. This process involves:

- Receiving transaction batches from the Celestia DA layer.

- Verifying the validity of each transaction in the batch:

  - Check that the sender has sufficient balance for transfers.

  - Ensure the transaction is properly signed.

  - Verify that the transaction doesn't violate any network rules.

-Updating the network state based on valid transactions.

- Detecting any fraudulent activities and producing fraud proofs if necessary.

While the Spicenet node software handles much of this automatically, it's crucial to understand the process and monitor your node's performance in these tasks.



# 5.1 Using REST API

Spicenet provides a comprehensive REST API for interacting with your node. Here are some key endpoints and their usage:

- Get events for a specific slot:

```bash
GET http://localhost:12346/ledger/events/{slot_number}
```

Example: `http://localhost:12346/ledger/events/17`

- Get events for a specific transaction:

```bash
GET http://localhost:12346/ledger/txs/{tx_id}/events/{event_index}
```

Example: `http://localhost:12346/ledger/txs/50/events/0`

- Get events with a specific key:

```bash
GET http://localhost:12346/ledger/batches/{batch_id}/txs/{tx_index}/events/{event_index}
```

Example: `http://localhost:12346/ledger/batches/10/txs/2/events/0`

To use these endpoints, you can use curl in your terminal or any HTTP client. For example:

```bash
curl -s http://localhost:12346/ledger/events/17 | jq
```

This will retrieve all events for slot 17 and format the JSON output for readability.

# 5.2 Using sov-cli

The `sov-cli` tool is your primary interface for interacting with your Spicenet node. Here are some common commands and their usage:

Generate a new key pair:

```bash
./target/debug/sov-cli keys generate
```

This will output a new public and private key pair. Make sure to securely store the private key.

- List transactions in your local pool:

```bash
./target/debug/sov-cli transactions list
```

This shows all transactions that have been imported but not yet submitted to the network.

- Query node status:

```bash
./target/debug/sov-cli node status
```

This provides information about your node's current state, including the latest processed slot and any pending transactions.

- Get help on available commands:

```bash
./target/debug/sov-cli --help
```

This displays all available commands and their brief descriptions.

Remember to run these commands from the directory where `sov-cli` is located.


#  Rewards and Incentives

As a Spicenet validator, you'll earn rewards for:

- Successfully receiving and verifying transaction batches

- Maintaining the network state accurately

- Detecting fraud and producing fraud proofs when necessary

The exact reward structure is determined by the network's governance and may change over time.


# Upgrading Your Node

When a new version of Spicenet is released:

- Check the release notes for any breaking changes

- Stop your node

- Backup your data

- Update the Spicenet software:


```bash
git pull
cargo build --release
```
- Restart your node

Always test upgrades on a testnet before applying them to your mainnet validator.

## DISCLAIMER

This repository was created solely for educational purposes.
All instructions, descriptions, and content are sourced from the official Spicenet documentation. I do not claim authorship or ownership of any software, specifications, or protocols referenced here. If you encounter issues or need help, please refer to the official Spicenet resources and join the community Discord. Any use of this information is at your own risk. CryptoWithShashi (CWS) is not responsible for any losses, damages, or issues arising from following this guide.



## ABOUT ME

Twitter -- https://x.com/cryptowthshashi

Github -- https://github.com/cryptowithshashi

Telegram -- https://t.me/crypto_with_shashi
