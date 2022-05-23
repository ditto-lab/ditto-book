# Introduction

Ditto Protocol is a trustless and governance-free NFT Futures protocol.

Protocol Overview
-----------------
Interested buyers of an NFT can mint or buy a Future, called a Clone.

Each clone encodes –
- an NFT collection,
- a specific token or the floor of that NFT collection,
- its current worth,
- minimum bid amount to purchase the clone decided by the smart contract.

If a bidder bids on a clone with an amount greater than the minimum bid amount, the clone is transferred to them from the current owner. On each Clone transfer, a fee is taken from the bid amount which is split between the previous holder of the Clone and a separate pool.

A clone represents a claim to receive the NFT when it’s sold via Ditto Protocol. A clone can be a floor Future, which means its worth is the minimum amount the NFT seller gets.

As that Clone gets traded, the pool size increases, incentivizing the NFT holder to sell their NFT via the Ditto Protocol.

Protocol features –
- Ditto protocol is fully trustless and governance free. All the parameters are hardcoded in the contract with no way to change it after deployment.
- There is no protocol fee, and any other fee is to incentivize protocol participation.
- We are looking at Verifiable Delay Functions (VDF) to combat front running.
- TWAP oracle for clone prices.
- On a clone transfer from a bidder, in addition to receiving a part of the fee from the higher bid, they’ll receive an on-chain NFT. We believe this will act as an additional incentive to participate in the protocol, and we want to work with an artist to make these NFTs something people will want to own.

The protocol is inspired by the [SALSA](https://www.radicalxchange.org/kiosk/blog/millennials-zoomers-and-salsa-just-radical-enough/) concept from [radicalxchange](https://twitter.com/RadxChange).

Gitcoin Grant
-------------
If you want to support us, we are accepting funding via our [Gitcoin grant](https://gitcoin.co/grants/4866/ditto-protocol-trustless-nft-futures).

Reference
---------
- [Github](https://github.com/ditto-lab/)
- [Twitter](https://twitter.com/ditto_lab)
- [Website](https://ditto-lab.github.io/)
