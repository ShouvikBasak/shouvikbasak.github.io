---
layout: post
title:  "Blockchain - the business game changer"
date:   2018-06-08 07:55:09 +0530
---
It all started with Bitcoin in 2009. The underlying technology used by bitcoin was the first version of blockchain. Since then, blockchain has come a long way. Blockchain not only power the myriad cryptocurrencies, but now is becoming the foundation for many applications. From the public implementation of blockchains, primarily for cryptocurrencies, the evolution is towards the private implementation of blockchain within enterprises, embedded into applications.

We are at a juncture where there are innumerable possibilities that can be foreseen for the use of blockchain. Blockchain is disruptive!

The use of blockchain technology will disrupt the way interactions happen in society, how records are kept and how business and trade happens. In the longer term, there will be a potential shift from trust based economy on central authorities to proof based economy based on collaboration and consensus. The technology is still at a very nascent stage but in all likelihood will have an impact on the economy, society and culture. This potentially will change how frauds happen, all based on immutable records and distributed database not controlled by a single authority and hard to tamper.

The combination of IoT (Internet of Things), Big data, easily accessible on demand computing power in the Cloud, coupled with Blockchain and "smart apps" will lead to many technological advancements in the next few years. It will be a very, very interesting journey from a technology and social perspective. There will be a different approach to creating the business processes and how multiple parties will work together in a supply chain, enabled by *smart contracts*.

But what is this technology that can have such a significant impact on our business and interactions? Let us look at it at a high level.

<h3>What is blockchain?</h3>
Blockchain is a *secure, shared, distributed ledger*. The distributed ledger can be public, private or part of a consortium.

The key aspects of the blockchain database are:
* Secure - Each transaction is secured with cryptography. Cryptography is the fundamental building block based on which blockchain has been developed.
* Shared - The database is shared between parties (peer-to-peer) without the need of any intermediary to verify the authenticity of the records of the database, thereby not needing a central authority to be trusted.
* Distributed - The blockchain database is distributed with multiple replicas of the same database. This also means that the trust on the data is also distributed. How is this achieved? Through a mechanism of consensus.
* Ledger - A ledger where you can only write and cannot alter records once written. The blockchain database can only be appended with immutable records of each transaction and time-stamped. The transactions are recorded in the shared ledger in a secured and verifiable way, reducing chances of tampering the records.

<h3>How does blockchain work?</h3>
The technology is quite complicated but here is what happens in a simplified form. In the blockchain, a block consists of the data, hash of the previous block and hash of the current block. So when the data (or the hash of the previous block) in a block changes, the hash of that block would change, which would need the hash in the following block to change as well ... and so on. All following blocks need to be updated if one block in the chain is updated.

![Blockchain blocks]({{ "/assets/blockchain-blocks.png" }})

This becomes a deterrent for the data on a single block to be changed, however it is not impossible to change the hash of all forward blocks. To overcome this challenge, blockchains use distributed databases. The complete blockchain database is available with each of the participating entities in a peer-to-peer communicating network.

![Blockchain Distributed Database]({{ "/assets/blockchain-distributed-database.png" }})

When one block needs to be added, that block is sent to all the participating entities (peers). Each of the participating entities need to approve that the block is valid. Once more than fifty percent of the entities confirm that the block is valid, the block can be added. At a high level, this prevents tampering of the records in the blocks.

The blockchain technology was introduced with the Bitcoin. Bitcoin was the first generation blockchain. Then came Ethereum, the second generation blockchain, which added *Smart Contracts* on top of the blockchain. Smart contracts are *digital* contracts stored in a distributed ledger. They are computer programmes, written with certain logic and stored in a blockchain. As this sits in a blockchain, the smart contract becomes immutable, making it very hard to tamper with it and manufacture a fraud. The applications are then built to use the smart contracts and often use automated data feeds from IoT sensors and other sources.

It is important to note that the implementation of blockchain in the enterprises need considerations that would be different than the the public implementations of blockchain. Two starting points to explore enterprise blockchain solutions are on AWS and Azure. AWS has launched the capability to build [blockchain networks on AWS](https://aws.amazon.com/blockchain/). Azure also provide capabilities to build apps on blockchain using [Azure Blockchain Workbench](https://azure.microsoft.com/en-in/solutions/blockchain/).

Further reading:
* [Bitcoin: A Peer-to-Peer Electronic Cash System](https://bitcoin.org/bitcoin.pdf) by Satoshi Nakamoto. The original paper published in 2008. No body knows who Satoshi Nakamoto is, whether he is an individual or a group, but this paper has its own place in history, and will be the trigger to many dramatic socio-economic changes in the future.
* [How the blockchain will radically transform the economy](https://www.youtube.com/watch?v=RplnSVTzvnU) - a TED Talk by Bettina Warburg. Without getting into the technology part of blockchain, this talk beautifully sets the perspective of blockchain and its potential future influence.