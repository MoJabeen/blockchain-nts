Description: Summary of the points from the book Mastering BTC.

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

