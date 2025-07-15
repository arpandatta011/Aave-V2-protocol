# Aave Wallet Credit Scoring

## Overview

This project builds a simple credit scoring system for wallets interacting with the Aave V2 protocol, using historical DeFi transaction behavior. Each wallet is scored from **0 to 1000**, where:

- **Higher scores** represent responsible and safe behavior.
- **Lower scores** indicate risky, bot-like, or exploitative usage.

## Dataset

The dataset contains transaction-level logs of 100,000 entries in a JSON file format. Each transaction includes:
- `userWallet`
- `action`: deposit, borrow, repay, redeemunderlying, liquidationcall
- `amount`, `assetSymbol`, `assetPriceUSD`, and `timestamp`

## Methodology

### 1. **Feature Engineering**
We extracted per-wallet metrics such as:
- Total **deposited**, **borrowed**, and **repaid** amounts in USD
- Number of **liquidations**
- Total **transaction count** (`tx_count`)
- **Activity duration** (based on timestamps)

### 2. **Scoring Logic**
The scoring logic is based on heuristic rules:

| Feature        | Impact on Score                           |
|----------------|--------------------------------------------|
| High Repayment | Increases score                            |
| High Borrowing | Decreases score                            |
| Liquidations   | Strong penalty                             |
| Active Usage   | Small bonus                                |
| Long Duration  | Small bonus (in months)                    |

Final score is clamped between `0` and `1000`.

### 3. **Technology Stack**
- Python 3
- Pandas
- Seaborn & Matplotlib (for visualizations)

### 4. **Output**
- `wallet_scores.json`: wallet-wise scores and summary
- `score_distribution.png`: histogram of wallet scores

## How to Run

```bash
python score_wallets.py
