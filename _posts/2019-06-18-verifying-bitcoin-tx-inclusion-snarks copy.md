---
layout: post
title: "SNARK Relay: A First Shot at Verifying Bitcoin TX Inclusion in ZKP"
tags: [jupyter]
---

<a href="https://github.com/dec3ntral/snark-relay" target="__blank">SNARK-relay</a> is a first shot at proving Bitcoin transaction inclusion inclusion on Ethereum more efficiently.


## Quick Overview (more details soon)

We use zkSNARKS are used to verify inclusion proofs in Bitcoin's transaction Merkle tree. 
Specifically, we prove that a given set of double SHA256 hashes is indeed the correct path to a leaf in the Merkle tree containing all transaction identifiers in a block.

Our implementation supports Merkle trees up to a depth of 11 (can be extended to more), i.e., up to 2048 transaction per block.

We use the, <a href="https://github.com/Zokrates/ZoKrates" target="__blank">ZoKrates</a> toolkit to generate both prover and Solidiy verifier.

## Performance 

Our construction requires ~975k constraints.

The main bottleneck thereby is Bitcoin's double SHA256, and the fact that we have no means to easily break out of a loop (which would allow to save costs on shallow trees).

Verification in Solidity has a constant cost of 600k gas.


## Comparison
If we compare the SNARK approach to native Merkle tree verification in Solidity, we observe that the latter is currently cheaper: ~80k gas for a tree of depth 1, ~200k gas for a tree of depth 11. 

The main difference however is the following: SNARK-relay costs 600k <b>per SNARK</b>. The native Solidity implementation costs up to 200k gas <b>per verified transaction</b>.

## Outlook and Future Work
Following the evaluation, our goal is to pack more inclusion proofs into a single SNARK to amortize costs, possible using recursive composition.

After that, we want to try to prove sequences of PoW block headers (~175k gas per header in pure Solidity) and parts of Bitcoin's spend script for UTXOs (~120k gas per parsed transaction in pure Solidity).

## Acknowledgements
Huge thanks to <a href="https://daniel.perez.sh/" target="__blank">Daniel Perez</a> and <a href="https://dominikharz.me/" target="__blank">Dominik Harz</a>, with whom we developed this together.

This project was developed during the hackathon at the Zero-Knowledge Proofs Workshop in London, June 17th 2019, hosted by Binary District.



## Relevant Links
+ https://github.com/dec3ntral/snark-relay
+ https://github.com/Zokrates/ZoKrates
+ https://binarydistrict.com/events/workshop/development/zero-knowledge-proofs-workshop
