---
description: An overview of oracle used on Buffer Finance
---

# Oracles

An oracle refers to a smart contract providing readable on-chain data. Buffer protocol uses a native high-speed oracle to meet the protocol trading requirements.

The publisher fetches prices for the required asset pairs every second from Buffer’s off-chain oracle. The price is then stored in DB along with a signature. The keeper then picks up the data to open and close the traders. The [keeper ](../developer-docs/keepers.md)contract which is trustless then validates this data by generating a signature and checking if the signer is the publisher.&#x20;

_\* Publisher: a smart contract that signs the data for opening and closing trades for validation_

_\* Keeper: trustless smart contracts that take the signed data from the publishes and run bots to open and close trades._
