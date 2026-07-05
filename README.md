##Autocall Pricer (Monte Carlo)

A minimal Python implementation of a Monte Carlo pricer for a **Phoenix autocall
note** (a structured product commonly sold to retail and institutional clients).

## What it does

The underlying is simulated thousands of times as a random path (geometric
Brownian motion). For each simulated path, the note's cashflow rules are applied:

1. **Conditional coupon** — paid each year if the underlying is above the coupon
   barrier (70%). If missed, it is memorised and paid later once the barrier is
   met again (memory / snowball effect).
2. **Autocall (early redemption)** — if the underlying is above the autocall
   barrier (100%) on any observation date before maturity, the note redeems
   early at par and stops.
3. **Maturity redemption** — if the note survives to year 5, capital is returned
   in full above the protection barrier (60%), otherwise the investor takes a
   loss equal to the underlying's performance.

The **price** is the average of the discounted cashflows across all simulated
paths (100,000 by default). Each cashflow is discounted to today using its own
observation date, not a single blanket discount factor.

## Run it

```bash
pip install numpy matplotlib
python pricer_simple.py
```

Output:

```
Prix de l'autocall : 1015.5  (pour un nominal de 1000)
Graphique enregistre dans trajectoires.png
```

## Example output

Simulated underlying paths with the three barriers:

<img width="1080" height="600" alt="trajectoires" src="https://github.com/user-attachments/assets/4d332e03-eeda-47f2-9e73-4957023a4b4e" />


## Parameters

| Parameter | Meaning | Value |
|---|---|---|
| `r` | risk-free rate | 3% |
| `q` | dividend yield | 2% |
| `sigma` | volatility | 22% |
| `autocall_barrier` | early redemption trigger | 100% of spot |
| `coupon_barrier` | coupon trigger | 70% of spot |
| `protection_barrier` | capital protection at maturity | 60% of spot |
| `coupon` | conditional coupon per year | 8% of notional |
| `n_annees` | number of yearly observations | 5 |

## Notes

This is an educational, single-underlying model (constant rate and volatility).
It doesn't include a volatility surface, stochastic rates, or issuer credit risk.
The goal is to illustrate the mechanics and Monte Carlo valuation of an
autocall, not to be a production pricing library.

## License

MIT
**
