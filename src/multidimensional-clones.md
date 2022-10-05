# Multidimensional Clones

The Ditto Protocol creates a queue of clones. Each clone in that queue represent an eventual claim on some unique NFT. The clone at the front of the queue has the immediate claim on the underlying NFT when it is sold through the protocol.

As soon as this trade happens, the NFT is transferred to that clone owner, and the clone is burned.

Here's a more technical description for accuracy -

A queue `Q` is identified by a parameter `protoId` which is a hash of
- the underlying `ERC721`/`ERC1155` contract address.
- `tokenId`.
- the `ERC20` contract,.
- and a boolean `floor`.

All this put together means that the clones part of this queue have a claim on the NFT corresponding to the `ERC721` or `ERC1155` smart contract under a specific `tokenId`, and they are bidding using particular `ERC20` tokens. If the `floor boolean` is set to `true` the clones are not bidding for any specific token in the NFT collection. In this case, `id` must have a constant value - `FLOOR_ID`.

The queue now contains a list of indices (non-negative integers), and a `(protoId, index)` tuple uniquely identifies a clone. In fact, `cloneId` is defined as the hash of `protoId` and `index`.

A bidder is allowed to bid on an existing clone, or a new clone which is pushed at the end of the queue. If no clone is dissolved, the index of the clones in the queue will be `0,1,2,...` and so on. But since, a clone can be dissolved, at any moment the queue will be missing the indices corresponding to the dissolved clones.

## Duplicate

## Dissolve
A clone owner can choose to burn the clone, which in effect, removes their claim on the underlying NFT.
- Their bid is returned after subtracting the subsidy and fee.
- The subsidy is passed to the clone next in the queue.
- The corresponding index is removed from the queue.

## Trade
