# Multi-Armed Bandit Simulation

A Python simulation that compares three strategies for choosing between options with unknown payoffs: **explore only**, **exploit only**, and **ε-greedy**.

Built for ECS 101 at Syracuse University (Nov to Dec 2025) as a team project with 4 people.

## The problem

You can eat at one of three cafeterias each day for 300 days. Each cafeteria gives a random happiness score each visit, but you don't know which one is best.

| Cafeteria | Average happiness | Spread (standard deviation) |
|-----------|-------------------|-----------------------------|
| 1         | 7                 | 3                           |
| 2         | 10                | 10                          |
| 3         | 4                 | 6                           |

Cafeteria 2 is the best on average, but it also swings the most, so a bad first visit can make it look worse than it is.

## The strategies

- **Explore only:** visits every cafeteria equally (100 times each) and never uses what it learns.
- **Exploit only:** visits each cafeteria once, picks the one with the highest score, and stays there for the rest of the days.
- **ε-greedy:** visits each cafeteria once, then keeps a running average of each one. Each day it explores a random cafeteria 10% of the time and picks the best average so far the other 90%.

## How results are measured

The best possible total is 3,000 (300 days at the best cafeteria's average of 10). **Regret** is that best total minus what a strategy actually earned, so lower is better.

The simulation runs each strategy 300 times and averages the results.

## Results

A typical run (numbers vary slightly each time, since the random seed isn't fixed):

| Strategy     | Average happiness | Average regret |
|--------------|-------------------|----------------|
| Explore only | about 2,100       | about 900      |
| Exploit only | about 2,440       | about 560      |
| ε-greedy     | about 2,670       | about 325      |

- ε-greedy cut regret by roughly 40% compared to exploit only, and 65% compared to explore only.
- Exploit only often locks onto the wrong cafeteria after an unlucky first visit to cafeteria 2. It picked the best one only about 57% of the time in a longer test.
- Explore only matches its expected value (2,100), which is a useful check that the simulation works.

## How to run

You need Python 3. There are no extra libraries to install.

```
python bandit_simulation.py
```

It prints the optimum happiness and then the simulated happiness and regret for each strategy. You can change the number of trials by editing the number at the bottom of the file (`simulation(300)`) and the exploration rate with the `e` argument (default 10).

## Files

- `bandit_simulation.py`: the full simulation.

## What I learned

- Simple strategies can look fine until you measure regret over many trials.
- A small amount of exploring (10%) beats both extremes when the best option isn't obvious.
- Fixing bugs mattered. Early versions reused a fixed random seed and let ε-greedy use the true averages, which gave misleading results. The final version uses a fresh random draw each time and has ε-greedy learn from the scores it has seen.
