# Oracle

To track a clone's worth on-chain, Ditto protocol has a oracle built into its smart contract. The oracle design is the same as the Uniswap V3 oracle. Any time a clone's worth changes (duplicate, dissolve, trade), the current _accumulated worth_ is stored with the current timestamp.

>**Note:** Not all clones are tracked by the oracle. Only the clones at the front of a `protoId` queue are tracked.

## Accumulated worth
Let's say due to some action, a clone's worth changes from \\(w_{n}\\) to \\(w_{n+1}\\) at timestamp \\(t_{n}\\). The current accumulated worth is \\(W_{n}\\) and was last updated at time \\(T_{n}\\). After the action is done, these are the new values:
\\[W_{n+1} = W_{n} + (w_n \times (t_{n}-T_{n}))\\]
\\[T_{n+1} = t_{n}\\]

The first time a clone is duplicated, the accumulated worth and current worth of a clone is `0`, so cumulative worth remains \\(W_1\\) remains `0`, and \\(T_1\\) is set to the current timestamp.

Effectively, _accumulated worth_ is the sum of all the worths of a clone in the past weighted by the duration for which that worth remained unchanged.

>**Note:** From the above equations, the following conclusions can be derived:
>- the oracle update happens _before_ the price change.
>- If multiple updates happen in the same block, writing each of them to oracle is a no-op. So only the first update in a block is written to oracle.

## Time weighted average worth
TWAP (time weighted average price) oracles are widely used to be secure from price manipulation attacks. Historical observations for a clone are persisted by oracle and only overwritten when the number of observations written exceeds a threshold (defined by _cardinality_). Initially, only 1 observation per clone is persisted, so a new update overwrites the previous observation. However, anyone can call `grow(protoId, cardinality)` function to increase the cardinality for a clone up to a maximum of 65535. This means Ditto can store up to a maximum of 65535 observations for a clone.

Even though the technically correct term is Time weighted average worth (TWAW), we'll continue to use the term _TWAP_ to refer to it.

## Reading observations
To read from oracle, call `observe(protoId, secondsAgos)`. `secondsAgos` is an array of length `n`. This call returns an array `cumulativeWorth` of the same length `n`.

`cumulativeWorth[i]` represents the accumulated worth at time `t-secondsAgos[i]`. Effectively, you can look back in the past and read the cumulative worth of a clone. We now dive into how different possible values of `secondsAgos` are handled:

- **`secondsAgos[i]` is `0`**: This returns the latest observation.
- **`secondsAgos[i]` points to a timestamp of an existing observation**: This returns that specific observation.
- **`secondsAgos[i]` points to a time older than the timestamp of the oldest existing observation**: This reverts.
- **`secondsAgos[i]` points to a time between 2 consecutive observations**: This returns a time weighted cumulative worth of 2 consecutive observations.

  Let's say the two consecutive observations are: \\(w_1, t_1\\) and \\(w_2, t_2\\), and `secondsAgos[i]` refers to time \\(t\\). Then the time weight cumulative worth is \\(w_1 + (w_2 \times (t-t_1))\\). Effectively, this refers to the observation if it would have been written at time \\(t\\).

- **`secondsAgos[i]` points to a time newer than the timestamp of the newest observation**: This returns a time weighted cumulative worth of the latest observation and current worth.

  Let's say the latest observation is: \\(w_1, t_1\\) and the current worth is \\(w_2\\), and `secondsAgos[i]` refers to time \\(t\\). Then the time weight cumulative worth is \\(w_1 + (w_2 \times (t-t_1))\\). Effectively, this refers to the observation if it would have been written at time \\(t\\). It's the same concept noted in the previous point.

## Calculating TWAP

Now what callers would generally need is time weighted average worth between 2 timestamps \\(t_0\\) and \\(t_1\\);  \\(t_1\\) being the current time. To get a TWAP:
- call `cw = observe(protoId, [t_0, t_1])`. (`cw` refers to cumulative worth).
- then TWAP is given by `(cw[1]-cw[0])/(t_1-t_0)`.

## Effects of increasing time interval for TWAP
TWAP model is suggested for safety from any recent volatility or price manipulation attacks on Ditto clones. Increase in the time interval makes it less sensitive towards these kinds of attacks. However, this also makes the TWAP deviate from the market price. So the callers should determine a suitable time interval depending on the use case.
