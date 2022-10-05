# Vouchers

The Ditto smart contract keeps track of `vouchers`, a kind of receipt for having owned a clone with a position on an underlying NFT.

`vouchers` are recorded any time a larger position on a clone's underlying NFT replaces the current position.

The important information about the position is hashed into a unique 32 byte identifier and stored in the contract. Anyone may read the smart contract to see if a voucher's identifier has been recorded. This allows for any party to construct a reward system or incentivize trading on top of Ditto.

## Encoded Information

- If the clone was at the head of the auction queue at time of voucher issue.
- `cloneId` The clone ID itself contains a lot of useful information.
  - The clone's `protoId` containing the NFT contract address, token ID, ERC20 address, and a `floor` boolean.
  - The clone's Index.
- The recipient of the `voucher`.
- `heat` property at the time.
- The smaller position's value.
- The larger position's value.
- The time at which the smaller position was opened.
- The time at which the voucher was issued.

These variables should be helpful for any party interested in constructing incentives on top of Ditto.
