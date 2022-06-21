Description: Summary of the points from the book Mastering BTC by Andreas Antonopoulos.

# Mastering BTC

## Overview of BTC

### What is BTC ?

- BTC (*Bitcoin•) is a fully distibuted peer to peer system, with no real coins.
- Value is implied via cryptographically signed transactions
  - The signature is the authentication layer preventing countefeit use
- The bitcoin protocol stack is open source and the protocol can be communicated via the internet.
- BTC currency is issued via mining at fixed rate with 4 year halfing period, this will lead to a cap of 21 million in **2140**.

### What is the short history of BTC?

- 2008 White paper released by satoshi
- 2009 Bitcoin network started
- Satoshi withdrew in 2011, leaving the development to a group of volunteers

## BTC Clients

### What is a client?

- End software for the user to gen private key and an address, enables transactions and communicates to the btc protocol. Connected to the users wallet.

### What are the types of clients?

- **Full Index Node**: Access to the full btc protocol and holds backup of all transactions

- **Light Node**: Control wallets, relies on servers for transations and btc network access.

- **Web**: Holds nothing, acts as api to servers for protocols 

## Transactions

### How is a transaction made?

- The inputs for a transaction are the previous transactions, this is then *encumbered*

*Encumbered : Using the wallets key and the recipients address the transaction is locked so it can only be used by the receiver.*

- As all transactions are based on prev transactions, a full coin amount received previously is used. The change then is sent back to the sender if there was any extra in the amount.

- The inputs are pointers to UTXO (Unspent transaction outputs)

- All nodes broadcast any transaction they receive to other nodes until the recipients client receives it.

- Clients can be confident if the transactions uses validated unspent transactions and contains enough fee to be mined; it will be.

> Transaction Output == Inputs + Mining Fee

### How are transactions validated by the different nodes ?

- When full clients receive a transaction they check the entire transaction history. Light clients will check if it exists in the blockchain and theres block on top.

### How are private keys generated ?

- Via a secure source of entropy or randomness a number between 1 and 2^256(slightly below this as set by the elliptic curve) is chosen. Generally to ensure randomness, there is a human element of entropy used.

### How is the public key calculated ?

- Public key is calculated from the private key using elliptic curve multiplication which is irreversible.
- Using a fixed generator point from elliptic curve maths allows for all priv keys to be calculated into pub keys. But only this direction.

### How is an address made ?

> Address = RIPEMD160(SHA256(Public Key))

### How does BTC avoid human errors when using addresses ?

BTC and other currencies used Base-58.

- Base58 creates a nice balance between compact representation, readability, error detection and prevention.
- Base58 is Base64 (upper and lowercase letters, 0-10, +, /) with commonly mistaken symbols (+,/,0,O,l,I) removed.

- Base58check adds an extra 4 bytes at the end of the hash of the data, allowing for easy checks for mistakes.

### Key compression

To save data during transmission keys are compressed.

- As the elliptic curve produces two sets of coordinates for the public key if only the x is taken its a compressed key.
- The choice between the positive and negative coordinates is shown by the prefix (02 for even and 03 for odd)

- Private keys are not compressed, wallet import format (WIF) adds a suffix to show it is from an new wallet and should be used to create an compressed pub key.

### How can you make the priv key memorable ?

- Seeded wallets or deterministic use a seed and hash to generate the private key. This has the benefit of allowing you to remember the keywords to regenerate the private key if needed.
- A lot wallets use 12 mnemonic words as the seed.

### What do you do if a child transaction is received before the parent ?

A child transaction is one based of the parent transaction.

- The transaction is stored in the temp orphan pool with set number of max stored children allowed.

### How are transactions unlocked using scripting ?

- Bitcoin clients validate transactions by executing a script, in a specific btc fort like scripting language.
- The locking script ensures conditions need to be met to spend the transaction: **scriptPubKey**. Using the recipients address.
- The unlocking script satisfies the need conditions set by the locking script: **ScriptSig**. Using the wallets priv key signature.
- The scripting language has the features of operating on the stack data structure, it is not general purpose (turing incomplete) limited in complexity and has predictable execution, it is also stateless.

- Generally process before a transaction is spent: Unlock -> Lock

- Multi sig is possible, where multiple users are required to unlock a transaction.

- OP_RETURN : Used for storing unspendable data, added to the transaction output.

- P2SH (Pay to script hash) in the locking script uses a hash of the redeem script instead of multiple address. The redeem script is in the unlocking script. 
  - Much smaller transactions
  - Used for multi sig - that require more complex sigs.
  - Burden in on the recipient not sender

## The Bitcoin Network

### How is the bitcoin network structured ?

The bitcoin network is a peer to peer network, there is no centralised server all nodes facilitate the network. The nodes provide and consume services.

- **The bitcoin network runs the bitcoin P2P protocol**. Other protocols can communicate with the network via bridge servers through the P2P protocol (Stratum is a protocol used for pooled mining).

- On node startup, there will generally be stored seeded nodes to be used for gathering required initial data. 

### What are some of the different types of nodes on the network?

- Reference Client (Contains wallet, miner, full blockchain database and Network routing)

- Full blockchain node (Full blockchain database and network routing)

- Solo Miner (Miner, blockchain, networking)

- Light-weight (Simple Payment Verification) (Wallet and network) ; Checks if transactions are at least 6 blocks deep to verify.
- Mining Nodes (Mining function and mining protocol)

### How do Lightweight nodes work?

- Only block headers are downloaded and not the full transaction list. The UTXOs available for spending is built by relying on peers to provide views of the relevant parts on demand.
- SPV nodes are dependant on other full nodes doing the complete verification and use this as a proxy to validate transactions.

### What is a bloom filter ?

Used by SPV filters to communicate with peer nodes asking for transaction data without revealing exactly which address they are searching for.

- A bloom filter filters outs the transactions it is being sent to leave only transactions needed by the wallet. The choice between accuracy and privacy (sending more non relevant transaction) can be made when setting the bloom filter.

### Who can trigger alert messages ?

Few keys devs with the correct private key can trigger an alert messages to show a serious blockchain issue.

## The blockchain

### What is the blockchain made of ?

A blockchain is an ordered linked list of blocks containing data. Each block refers to the previous block creating a chain.

- Each block is easily identified by its hash and will contain the previous blocks hash.
  - Creating a hash uses the data in the block and therefore changes in that data result in changes in the hash.

- As each block points to the previous block, changes in a block will require changes in all subsequent blocks.

### What is a block made of ?

- A blocks structure contains 80 bytes of header
- The header includes:
  - Software version
  - Previous block hash
  - Merkle Root
  - Timestamp
  - Difficulty target
  - Nonce

### How do you create a blocks hash ?

- The blocks header hashed twice
- The blocks header includes the previous blocks hash, so changing the previous block will change the next blocks hash

### What is the genesis block?

- The genesis block is the first ever block on the bitcoin chain.
- This block is stored statically in clients code as a reference point to build the chain on.
  
### What are Merkle trees ?

- Merkle trees are a binary hash tree which is structured as each leaf being a pair of transactions hashed, the resulting hash paired with another leaf and hashed again.
  - Until only on root leaf at the top.
- A more efficent method of validating transactions by validating a path through the tree for a transaction. Gives at worst 0 = 2log(N)
- SPV nodes ask for a merkle path and block header, allowing the node to validate if the transaction is in the block
- Rebuilds the path and then confirms if it matches the root leaf hash.

## Mining

Mining is the method of adding security to the blockchain, meaning a method of preventing malicious tampering of the data stored in the blockchain. It also enables the consensus mechanism proof of work.

Miners hash the blocks header attempting to solve the network problem by altering the nonce, when a miner solves this problem the associated block is added to the chain.

### How often is a block mined ?

- A new block is mined every 10 minutes, the network maintains this rate by varying the 'difficulty' causing blocks to be mined as the number of miners fluctuates.

### How do you incentivize mining ?

- Mining is incentivised by providing new coins to the miners, to keep the market deflationary the number of new coins mined is halved every 4 years. 
- Total bitcoins mined will be 21 million by 2140.
- Transaction fees are also collected by miners.

### What do miners do ?

- Miners are competing to solve a math problem of producing a hash lower than a threshold number for a given block.
- This competition enables the consensus mechanism proof of work. 

**Decentralized consensus is from four processes:** 
- Verification of each transaction by nodes
- Aggregate transactions into a new block verified via proof of work
- Verify new blocks by nodes
- Selection of the chain with the most cumulative computation

### How is a block built ?

- Due to the independent verification of each transaction, all nodes generally fill a pool of transactions that are the same.
- Transactions will be prioritized, with the highest priority placed into the block first.

> Priority = Value of input * input Age / Transaction size
  - Age of the UTXO is the blocks deep, transaction size is in bytes.

- The transactions are considered high priority if they have a value over 57,600,000 which equals (1 btc * 1 one day)/250 B
- High priority transactions are the first 50 kB of the blocks
- The rest is prioritized based on transaction fee per kb

- The first transaction added to the block is called the generation transaction; this is the miners reward (minted coins + transaction fee)
  - The generation transaction does not use UTXO as an input, instead uses **coinbase**

### How is the difficulty value normally represented ?

- The difficulty value is represented in coefficient exponent form:
- The first 2 hex digits are the exponent
- Next 6 digits are the coefficient

> Difficulty Target = coefficient * 2^(8*(exponent-3)) 

### How is the difficulty adjusted ?

> New Difficulty = Old Difficulty * (Actual Time of Last 2016 blocks/ 20160 mins)

### What happens if there is a fork of the chain (two different chains) ?

- The chain with the **the most cumulative work** will be chosen (the longest chain)
- Forks can occur when differing block solutions are found at a similar time and two chains temporarily exist
  - This is solved when a block is next added to either chain as this will then be taken as the main chain as it has the most work.

- The 10 min gap is a choice of how much security each block is receiving

### How has mining hardware changed ?

- Initially all miners were CPU based, however as the market adjusted with the price of bitcoin it became economically viable to use GPUs.
- This was then taken to the next level of custom ASIC devices, designed specifically to maximise the performance of bitcoin miners (maximizing the density, power and hashrate)

### How do mining pools work ?

- Pools are a method of combining the work of large collections of miners to greatly increase the likelihood of receiving the reward from mining.
- Pools use shares which are roughly 1000x easier than the difficulty target, to measure miner contribution.
- Pools are connected to a full node to construct blocks, but centralised :(

### Are there decentralised pools ?

- P2Pool is a decentralised version using a sharechain to keep track of miner contribution and rewards.

### What is a consensus attack ?

- A large group of miners attack the consensus mechanism deciding the new block
- This attack will allow double spending or denial of service, forking off the chain and creating enough blocks to shift the consensus to the new chain
  - This attack does not allow non signed transactions

