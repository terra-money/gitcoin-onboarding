# Terra Gitcoin Onboarding

## Welcome to Terra

Terra's mission is to set money free, and liberate human value and productivity from the systemic inefficiencies of traditional finance and payment systems. The Terra project is based around the Terra blockchain, which drives a stablecoin protocol and various other fundamental mechanisms built on top that provide the necessary infrastructure for a decentralized economy to see enable real-world user adoption. With the introduction of smart contracts, Terra will become a new space for developers to develop and launch DApps to power the innovation of money.

## Why build on Terra?

### Robust consensus and fast block finality

Terra is powered by Tendermint BFT consensus, using a dPoS-like scheme driven by a set of 100 top validators. This efficient consensus model enables batches of transations to occur in only 6 seconds (only a fraction of the time it takes for Bitcoin and Ethereum).

### Ready for DeFi applications

With fundamental infrastructure such as price oracles, on-chain swaps, stablecoin assets in a variety of denominations, community governance and automated monetary and fiscal policy, the Terra blockchain acts as its own autonomous sovereign economy driven by its users, and provides all the necessary incentive mechanics and modular plumbing to power modern DeFi smart contracts.

### Growing, active user base with real-world usage

With over 1.5 million total users and a highly active user base from a variety of integrations (like Terra-powered payment gateways such as CHAI and MemePay), the Terra economy is a thriving new home for the future of innovative DeFi products. And unlike many other stablecoin protocols, Terra stablecoins are directly integrated in payments solutions where they are used everyday purchases such as groceries, movie tickets, taxis, and more.

## The Terra SDKs

The primary way to interact with Terra programmatically are through the SDKs. These SDKs provide data structures and a REST client that interacts with a Terra node, providing serialization / deserialization so that you can focus on your application logic rather than technical minutiae. We currently have 2 SDKs:

1. [Jigu](https://jigu.terra.money) for Python 3

2. [Terra.js](https://github.com/terra-project/terra.js) for JavaScript

The easiest way to get started writing programs that interact with the Terra blockchain is through those SDKs. For this document, I'll be using Jigu but the concepts can be translated with little effort to JavaScript

## Using the SDKs

There are 2 primary ways that you can interact with the Terra blockchain.

1. Getting data from the blockchain

2. Broadcasting transactions to change the state

### Getting data from the blockchain

THe blockchain can be seen as a distributed database whose state is constantly being altered by participants, in a decentralized and permissionless manner. As such, you can consider it as an online service from which to query data, and the node which is connected to other nodes in the network can be considered your gateway to information on the blockchain.

After connecting to a node, you can query various pieces of information. The following example shows how Terra's blockchain data is structured by module, and can be accessed easily with a simple function call.

```python
from jigu import Terra

# connect to mainnet
terra = Terra("columbus-3", "https://lcd.terra.dev")

a_block = terra.block(45)

for block in terra.blocks[45:65]:
    print(block.height)

# get account balance in LUNA
balance = terra.bank.balance_for("terra...")
balance.uluna

terra.oracle.exchange_rate("uusd")
terra.market.params()._pp
```

You can explore the rest of the modules on the official Jigu documentation. For instance, here is the full coverage of the [Oracle](http://jigu.terra.money/docs/modules/oracle.html#votes) module.

### Broadcasting transactions to change state

The Terra blockchain is subdivided in different **module** which define ways its state can be changed through **message handlers**. If you want to perform operations that change the blockchain, you will need to create a **message**, and include it in a **transaction**, which will get picked up by a validator and included in a block. Each module defines the types of messages it can understand, how they will be processed and verified, and finally how they will affect the state of the chain.

When creating transactions, you don't need to know which module you will be using, just the message that is relevant to the actions that you wish to perform. The blockchain will automatically route the message to the proper module's message handler.

In order to create a transaction, you will need to first define the messages to include, and then sign the transaction prior to broadcasting. This requires a **wallet** -- which is a private and public key pair. In the SDKs, you can create a wallet which is just a mnemonic key that automatically fetches and includes the account and sequence numbers from the blockchain prior to signing.

The following example goes through the process of creating a random mnemonic and signing a transaction containing a `MsgSend`, the basic message for sending funds. It is recommended to follow along using an interaction Python shell:

```python
from jigu import Terra

soju = Terra("soju-0014", "https://soju-lcd.terra.dev")
assert soju.is_connected()

from jigu.key.mnemonic import MnemonicKey

wallet = soju.wallet(MnemonicKey.generate())
wallet.address
# terra17w4ppj92dwdf93jjtply08nav2ldzw3z2l3wzl

wallet.balance("uluna")
# Coin('uluna', 10000000000)

from jigu.core import Coins, StdFee
from jigu.core.msg import MsgSend

send = MsgSend(
    from_address=wallet.address,
    to_address="terra1wg2mlrxdmnnkkykgqg4znky86nyrtc45q336yv",
    amount=Coins(uluna=23_000_000)
)

fee = StdFee.make(50000, uluna=1000) # include a small fee..

tx = wallet.create_and_sign_tx(send, memo="Hello Jigu!", fee=fee)
res = wallet.broadcast(tx)
```

You can include multiple messages in a transaction:

```python
from jigu.core import StdFee
from jigu.core.msg import MsgSwap

swap1 = MsgSwap(
    trader=wallet.address,
    offer_coin=Coin("uluna", 150_000_000),
    ask_denom="uusd"
)

swap2 = MsgSwap(
    trader=wallet.address,
    offer_coin=Coin("uluna", 200_000_000),
    ask_denom="ukrw"
)

# Set our gas limit to 250,000 and pay 1 LUNA
fee = StdFee.make(gas=250_000, uluna=1_000_000)

# multiple msgs in 1 transaction
tx = wallet.create_and_sign_tx(swap1, swap2, fee=fee)
res = wallet.broadcast(tx)
```

## Working with Smart Contracts

If you followed the above examples, you should now be familiar with the general flow of working with the Terra blockchain from a programming environment. The next step is getting your feet wet with smart contracts.

A smart contract is an automated agent whose logic and state is persisted on the blockchain. This is enabled by a new module called the **WASM module**, whose majority part has been implemented by the [CosmWasm](https://cosmwasm.com). WASM stands for WebAssembly, a new, portable compilation target made for the web, but whose safety and performance properties and universal aim and reach make it suitable for usage for blockchain smart contracts.

In short, WebAssembly contracts enable us to have:

- small binary size
- efficient execution
- minimal runtime overhead
- easily integrate into our blockchain

Currently, smart contracts have to be written in Rust, a modern low-level systems programming language. There is a bit of a learning curve to it, especially around its new approach to memory management. Luckily, there are plenty of resources online (check [here](https://github.com/ctjhoa/rust-learning)), and it shouldn't take you too long before you're conversant with enough of the fundamentals to develop smart contracts in it. After all, smart contract logic need not be too complicated: changing balances, performing basic arithmetic, and managing data comprise the majority of the requisite operations that cover the gist of even the most interesting smart contracts.

Another benefit of choosing the WebAssembly/Rust ecosystem for our smart contracts is that the binary format has a much more diverse ecosystem than what is available for Solidity smart contracts. You should be able to use many interesting crates for advanced math or even machine learning in your smart contracts, if you wish to test the limits of the smart contract platform (though not advised due to gas).

### How Smart Contracts Work

There are 3 additional messages that will be handled by the WASM module:

- `MsgStoreCode`, which uploads the code to the blockchain
- `MsgInstantiateContract`, which creates a contract from some code previously uploaded
- `MsgExecuteContract`, which performs a contract call
