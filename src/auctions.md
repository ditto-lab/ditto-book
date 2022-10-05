# Ditto Auction Mechanism & Heat

## Auctions

When a Ditto clone is created it is put up for auction at all times via SALSA.

A clone has a self-assessed value called its `worth`. When the clone is minted its auction price is calculated as `2 * worth` with its price descending linearly until it is equal to its `worth` over a set dutch auction period. Once the dutch auction period has finished the minimum price to purchase the clone is its `worth` until it is sold via the auction.

After the clone is sold through the auction mechanism, the dutch auction period is then reset for the process to start over.

## Heat

`heat` is a variable of each clone determining fee amounts, and dutch auction period lengths. The `heat` of a clone is increased by `1` anytime a larger position is opened on a clone, and decreases relative to how much time has elapsed during its dutch auction.

`heat` has a maximum value of `255`.

If a clone's heat is 200 and half it's dutch auction period has elapsed when a larger position is opened on the underlying then its `heat` will become `100 + 1`, halved because of the elapsed time and `+ 1` because a larger positioned was opened on top of it.

### Fee Calculation

Fees are calculated as `heat * 32 / 2**16-1`. The minimum fee to be paid on an auction is `%0.04882887` while the maximum fee is `%12.451361868` as `heat` cannot exceed `255`.

### Dutch Auction Length Curve

Dutch auction length periods increase in time relative to heat according to this formula:

```
2^18 + (  (1 day) * 7 * log_2(cbrt(heat)) * sqrt(heat)  )
```

Dutch auction lengths can range between ~3 days to ~297 days. These auction lengths give a clone owner a reasonable assurance that they will own the position on the underlying until another outbids them for a higher value position.
