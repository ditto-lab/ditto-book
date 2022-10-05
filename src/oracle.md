# Oracle

To track a clone's worth on-chain, Ditto protocol has a oracle built into its smart contract. The oracle design is the same as Uniswap V3 oracle. Any time a clone's worth changes (duplicate, dissolve, trade), the current _accumulated worth_ is stored with the current timestamp.

>**Note:** Not all clones are tracked by the oracle. Only the clones at the front of a `protoId` queue are tracked.

## Accumulated worth
Let's say due to some action, a clone's worth changes from \\(w_{n}\\) to \\(w_{n+1}\\) at timestamp \\(t_{n}\\). The current accumulated worth is \\(W_{n}\\) and was lated updated at time \\(T_{n}\\). After the action is done, these are the new values:
\\[W_{n+1} = W_{n} + (w_n \times (t_{n}-T_{n}))\\]
\\[T_{n+1} = t_{n}\\]

The first time clone is duplicated, accumulated worth and current worth of a clone is `0`, so cumulative worth remains \\(W_1\\) remains `0`, and \\(T_1\\) is set to the current timestamp.

Effectively, _accumulated worth_ is the sum of all the worths of a clone in the past weighted by the duration for which that worth remained unchanged.

>**Note:** From the above equations, the following conclusions can be derived:
>- the oracle update happens _before_ the price change.
>- If multiple updates happen in the same block, writing each of them to oracle is a no-op. So only the first update in a block is written to oracle.

## Time weighted average worth
TWAP (time weighted average price) oracles are widely used to be secure from price manipulation attacks. Historical observations for a clone are persisted by oracle and only overwritten when number of obseravtions written exceed a threshold (defined by _cardinality_). Initially, only 1 observation per clone is persisted, so a new update overwrites the previous observation. However, anyone can call `Oracle.grow()` function to increase the cardinality for a clone up to a maximum of 65535. This means Ditto can store up to a maximum of 65535 observations for a clone.