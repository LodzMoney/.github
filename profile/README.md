<p align="center">
  <a href="https://lodz.money">
    <img
      src="https://lodz.money/images/org-banner.jpg"
      width="100%"
      alt="An underground adit. Rock walls threaded with gold seams, a single headlamp at the end of a straight shaft, and an orecart standing on the rails."
    >
  </a>
</p>

<h1 align="center">Lodz</h1>

<p align="center"><strong>Yield from the bedrock.</strong></p>

<p align="center">
  Bitcoin yield attribution on Solana. Every quoted rate split by who pays it.
</p>

<p align="center">
  <a href="https://lodz.money"><img alt="Website: lodz.money" src="https://img.shields.io/static/v1?style=flat-square&labelColor=14171A&label=WEBSITE&message=lodz.money&color=F2C230"></a>
  <a href="https://x.com/lodzmoney"><img alt="X: @lodzmoney" src="https://img.shields.io/static/v1?style=flat-square&labelColor=14171A&label=X&message=%40lodzmoney&color=2B2F33&logo=x&logoColor=F2C230"></a>
  <a href="https://www.npmjs.com/package/@lodz/cli"><img alt="npm: @lodz/cli" src="https://img.shields.io/static/v1?style=flat-square&labelColor=14171A&label=NPM&message=%40lodz%2Fcli&color=C98F2E&logo=npm&logoColor=F2C230"></a>
  <a href="https://github.com/LodzMoney/lodz"><img alt="Solana, Anchor 0.31" src="https://img.shields.io/static/v1?style=flat-square&labelColor=14171A&label=SOLANA&message=ANCHOR%200.31&color=8C4A2F&logo=solana&logoColor=F2C230"></a>
  <a href="https://github.com/LodzMoney/lodz/blob/main/LICENSE"><img alt="Licence: MIT" src="https://img.shields.io/static/v1?style=flat-square&labelColor=14171A&label=LICENSE&message=MIT&color=4E6B52"></a>
</p>

---

## The problem with a single number

A BTC yield product quotes one APY. That number hides the only question worth asking:
**who is paying it, and will they still be paying it next quarter?**

We measured the Solana BTC yield landscape on 2026-08-15 using public endpoints only,
before writing the program. Four results changed the design.

### 1. BTC lending on Solana pays approximately nothing

| Venue | Asset | Supply APY |
|---|---|---|
| Kamino | cbBTC | 0.00459% |
| Kamino | xBTC | 0.00063% |
| Kamino | FBTC | 0% |
| Jupiter Lend | all BTC markets | 0% |
| Loopscale | zBTC | 1.06% |

About $75.4M of BTC sits in these markets earning nothing, because interest is paid by
borrowers and there are no borrowers. Kamino's cbBTC reserve is 3.2% utilised.

"Deposit BTC, earn lending interest" is not a seam that exists on this chain today.

### 2. The real yield is liquidity provision fees, quoted gross

| Pool | APY (fees) | TVL |
|---|---|---|
| Orca cbBTC-USDC | 14.996% | $6.32M |
| Orca SOL-cbBTC | 16.046% | $4.58M |

Traders genuinely pay these. But the figure is fee revenue **before divergence loss**,
and DefiLlama reports `il7d` as null for every one of these pools. The number quoted
elsewhere is a gross figure presented as a net one.

We estimate the loss and subtract it. Where we cannot, the seam is marked `ilUnknown`
and no net figure is given. We do not invent the estimate.

### 3. Token emissions on Solana BTC pools are zero

All 94 BTC-related pools, 647 days of history, not one with a non-zero reward APY.

This is a measurement, not missing data: the same snapshot finds **15 pools with
`apyReward > 0` elsewhere on Solana**. The collector works. The number is zero.

That zero is the most useful thing on our dashboard. When a competitor advertises a
double-digit BTC yield, one of three things is true: fee revenue is shown without
divergence loss deducted, a points programme has been converted into an APY, or leverage
is folded into the headline. Separating the three is the entire product.

### 4. There is a third kind of yield

GMTrade's BTC-USDC vault reported 214.828% on $1.71M. Its source is **trader losses**.

On a chart it looks like fee revenue. In character it is the opposite: it depends on
someone else continuing to lose, and it inverts when they stop.

```
sustainable    trading fees, borrow interest -- money an outside user actually paid
emissions      protocol token emissions      -- money the issuer printed
counterparty   the other side's losses       -- money a trader lost
```

Most attribution models have two categories. The measurements say there are three.

---

## Repositories

| Repository | Contents |
|---|---|
| [**lodz**](https://github.com/LodzMoney/lodz) | Protocol core. Anchor vault program, generated IDL, and the specifications it enforces -- risk layers, seam schema, display rules |
| [**lodz-sdk**](https://github.com/LodzMoney/lodz-sdk) | Tooling. Attribution engine, risk model, redemption queue, seam router, client SDK and command line client |

The specifications cite the source lines that enforce them, so a claim in the docs can be
checked against the program without taking either on trust.

---

## What we will not do

Assets are identified by mint, never by symbol. Solana carries two different tokens
called WBTC with eight decimals each, and one of them has $46K of liquidity.

Spot rates are never rendered. The Orca cbBTC-USDC history contains a single day printing
74,187% -- a low-TVL calculation artefact -- so display uses a seven-day value or a
ninety-day median.

A rate on capacity nobody can use is not a seam. Zeus Bitcoin Market USDC advertised
104.6% against $10,927 of capacity, and sits below our TVL floor.

Points programmes are never converted to an APY. Attaching a price to unissued points
relabels emissions as fee revenue, so that yield is absent from our figures rather than
estimated into them. We would rather understate.

Basis seams are unsupported and say so. Drift and Zeta both return 403 without
authentication, and Drift's BTC-PERP has not processed a funding update since
2026-04-01 while holding 250 BTC of open interest. Open interest is not liveness. The
category is declared missing rather than quietly dropped.

---

## What this is not

Every asset routed is a wrapped or bridged claim on bitcoin held by a third party. It is
not a coin on the Bitcoin base chain, and this codebase does not describe it as one.

Principal is at risk. Deposits are not bank deposits, carry no insurance, and can be
impaired by a venue exploit, an issuer failure or a custody failure. Redemption is a
claim on open positions and the queue lengthens under stress.

The program builds and its local test suite passes. It is not deployed to mainnet and it
has not been audited.
