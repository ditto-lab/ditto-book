# Multidimensional Clones

Ditto Protocol creates a queue of clones. Each clone in that queue represents an eventual claim on some unique NFT. The clone at the front of the queue has the immediate claim on the underlying NFT when sold through the protocol.

As soon as this trade happens, the NFT is transferred to that clone owner, and the clone is burned.

Here's a more technical description for accuracy -

A queue `Q` is identified by a parameter `protoId` which is a hash of
- the underlying `ERC721`/`ERC1155` contract address, say `A`,
- `tokenId` of `A`, say `id`,
- the `ERC20` contract, say `B`,
- and a boolean `floor`.

All together, this means that the clones part of this queue want a claim on the NFT represented by address `A` with tokenId `id`, and they are bidding using ERC20 token `B`. If `floor` is `true`, the clones are not bidding for any specific token in the NFT collection. In this case, `id` must have a constant value - `FLOOR_ID`.

A `(protoId, index)` tuple uniquely identifies a clone, where `index` refers to the `0`-indexed position in the queue. In fact, `cloneId` is defined as the hash of `protoId` and `index`.

A bidder is allowed to bid on an existing clone, or a new clone which is pushed at the end of the queue. If no clone is dissolved, the index of the clones in the queue will be `0,1,2,...` and so on. But since, a clone can be dissolved, at any moment the queue will be missing the indexes corresponding to the dissolved clones.

Next, we go into detail on the actions you can take through Ditto protocol:

## Duplicate
Open or buy out a future on a particular NFT or floor Future of an ERC721 or ERC1155 collection. The ownership of this Future is represented via an ERC1155 token called a `clone`. The caller specifies:
- `_ERC721Contract`: NFT contract, also referred to as the "Underlying NFT".
- `_tokenId`: the specific token ID of the NFT contract. For floors, `_tokenId` has to be `FLOOR_ID`.
- `_ERC20Contract`: ERC20 token which is used to buy the clone,
- `_amount`: amount of ERC20 token to buy the clone. Has to be at least the amount returned by `getMinAmountForCloneTransfer(cloneId)`,
- `floor`: a boolean value determining if the purchase if for a floor Future.
- `index`: position in the queue of the claim on the underlying NFT.

If the bid is successful, this call returns two parameters:
- `cloneId`: an ERC1155 token ID owned by the buyer.
- `protoId`: identifier of the queue in which the clone was minted.

```javascript
function duplicate(
        address _ERC721Contract,
        uint _tokenId,
        address _ERC20Contract,
        uint128 _amount,
        bool floor,
        uint index
    ) external returns (uint cloneId, uint protoId);
```

## Dissolve
A clone owner can choose to burn the clone, which in effect, removes their claim on the underlying NFT.
- Their bid is returned after subtracting the subsidy and fee.
- The subsidy is passed to the clone next in the queue.
- The corresponding index is removed from the queue.

```javascript
function dissolve(uint protoId, uint cloneId);
```

## Underlying NFT Trade
If an underlying NFT owner decides to sell the NFT via Ditto, they just have to transfer it `DittoMachine` contract. This triggers a trade where the NFT to the clone owner at the front of the queue, and the subsidy amount plus clone's worth is transferred to the seller. It also burns the clone and moves the head of the queue to the next clone.
