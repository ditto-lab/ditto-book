# FAQs

## General

### What is Ditto?

Ditto a is a decentralized and permissionless protocol used to create tradable non-fungible [futures](https://en.wikipedia.org/wiki/Perpetual_futures) of non-fungible tokens. Anyone may create a future, called a clone, for any NFT or purchase one that has already been created at any time.

### How does Ditto work?

Ditto operates using a hybrid auction mechanism, innspired by [SALSA](https://www.radicalxchange.org/kiosk/blog/millennials-zoomers-and-salsa-just-radical-enough/) and dutch auctions.

A clone of an underlying NFT may be created by anyone at anytime at a self-assessed price. Once the clone is created it is put up for auction similar to SALSA, however the Ditto smart contract calculates the minimum price one must pay to take ownership of the clone, starting at double the clone's assessed price and decreasing towards the assessed price over a set period of time. This auction resets whenever a clone is traded via this mechanism.

While a clone exists for an underlying NFT this effectively gives its owner an option to sell the NFT through Ditto. When the underlying NFT is sold through Ditto its clone is burned, the seller receives the funds represented by the clone, and the clone owner receives the underlying NFT.

### What is a clone?

A clone is an ERC-721 non-fungible token representing the right receive a particular NFT when it is sold via the Ditto protocol. The clone may be traded freely and interface with any applications accepting ERC-721 tokens.

### What fees are associated with Ditto?

When a clone is created a fee is set aside from the position's value, as well as any time the clone is subsequently traded via Ditto's auction mechanism. The fee can range between %0.04882887 and %12.451361868 depending on how frequently it is traded via Ditto's auctions.

### Why sell an NFT through Ditto?

Whenever a clone is traded via Ditto's auction mechanism a fee is taken and pooled towards the underlying NFT. When the underlying NFT is sold via Ditto the pooled fees are transferred to the NFT seller along with the value of the clone.

### What is SALSA

Salsa is an auction mechanism from [radicalxchange](https://twitter.com/RadxChange). You can read more about it {here}(https://en.wikipedia.org/wiki/Perpetual_futures).

### What is a dutch auction?

An [auction](https://en.wikipedia.org/wiki/Dutch_auction) where the selling price starts high and descends until a participant accepts the price.
