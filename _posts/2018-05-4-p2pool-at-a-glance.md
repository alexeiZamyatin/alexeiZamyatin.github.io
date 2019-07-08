---
layout: post
title: "P2Pool at a Glance: Proof-of-Work Reusing for Trustless Share Validation"
tags: [jupyter]
---


P2Pool is a decentralized Bitcoin mining pool and was announced and launched in 2011. In contrast to conventional mining pools, P2Pool requires no operator to verify each miner’s contribution to the mining operation of the pool. Instead, a network of peer-to-peer miner nodes is created parallel to Bitcoin and the proof-of-work of mining pool shares is reused for verification of each miner’s contribution.

<br />

(<i>This post is also avalable on <a href="https://medium.com/@alexeiZamyatin/a-technical-deep-dive-into-merged-mining-5b67706e1a19" target="__blank">Medium</a></i>)

<br />

The key concept behind P2Pool is the so called *sharechain*. The sharechain is a fully functional blockchain, which runs in parallel to Bitcoin but maintains a significantly lower difficulty *D_share < D_BTC*, targeting a block interval of 30 seconds. Sharechain blocks are fully functional Bitcoin blocks, which meet *D_share* but not *D_BTC*, i.e., equate to shares submitted by miners to a mining pool.

<br />


Miners participating in P2Pool initially follow the normal mining process, as if solo mining. When building the block, the miner inserts his own payout address(es) in the outputs of the coinbase transaction and starts to search for solution candidates for the resulting PoW puzzle. However, in contrast to solo mining, P2Pool miners do not keep the complete block reward to themselves. Each time the miner finds a valid PoW solution where *D_share < D_PoW < D_BTC*, i.e., a valid share but not a full Bitcoin block, he publishes this block to the network of P2Pool miners. After the majority of the peers has verified that the block is valid, it is appended to the sharechain and all miners resume their search for the next Bitcoin block.

<br />


However, when building the preliminary block structure, miners now must include the payout address(es) of the previous sharechain block in the outputs of the coinbase transaction. Otherwise, P2Pool peers will discard the block as invalid and the respective miner will not be rewarded for the submitted share when a full block is found. Whenever any miner participating in the scheme finds a full Bitcoin block, he publishes it to the Bitcoin network. As a result, all miners who have submitted sharechain blocks during this round will receive a portion of the reward, directly distributed through the coinbase transaction of the block. 
<br />

A visualization of P2Pool’s sharechain is provided in the Figure below:
<br />
<br />

![Exemplary visualization of the P2Pool sharechain and its connection to Bitcoin. Each sharechain block (denoted as “Share”) references the previous Bitcoin block. With each found share, a new address is added to the outputs of the coinbase transaction of the to-be-found Bitcoin block. Once a full block is found, all miners which have submitted shares receive rewards. In our case miner A will receive half of the generated revenue.](https://cdn-images-1.medium.com/max/10350/1*VDjL7DVffaxgVft6pjgm0g.png)*Exemplary visualization of the P2Pool sharechain and its connection to Bitcoin. Each sharechain block (denoted as “Share”) references the previous Bitcoin block. With each found share, a new address is added to the outputs of the coinbase transaction of the to-be-found Bitcoin block. Once a full block is found, all miners which have submitted shares receive rewards. In our case miner A will receive half of the generated revenue.*

<br />
<br />

A mining round in P2Pool represents a sliding window 8.640 sharechain blocks, i.e, approximately three days, and the Bitcoin block rewards are split according to each miners contribution during this period. The sharechain hence only contains 8.640 block at any given point in time, always discarding the oldest share each time a new is found. Since each sharechain block references a full Bitcoin block, participating peers can verify work performed in the past by checking the Bitcoin blockchain. Since P2Pool has no central operator, no fees are charged for participation.

<br />

(Disclaimer: The text in this article is an edited excerpt from my Thesis: “*Merged Mining: Analysis and Implications”, A. Zamyatin, MSc Thesis, TU Wien, 2017, *PDF available [here](http://repositum.tuwien.ac.at/obvutwhs/download/pdf/2315652?originalFilename=true))
