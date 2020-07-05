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

After connecting to a node, you can query various pieces of information. The following example shows how Terra's blockchain data is structured by module:

```python
from jigu import Terra

soju = 
```

