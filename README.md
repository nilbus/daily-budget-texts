daily-budget-texts
==================

Send a daily text message with how much of your monthly budget remains, based on spending transaction data from [Mint][].

    Budget remaining: $3040.66 (76%)
    Savings bucket: $1042

How it works
------------

- You set a monthly _spending allowance_ and _savings target_. The spending allowance is for typical monthly expenses, and the savings target is intended to build up for larger purchases, vacations, etc.). For most situations, these combined will equal your monthly net income.
- Every day, daily-budget-texts logs into your Mint account to pull the month's latest transactions from your bank and credit accounts.
- You receive a daily text message showing how much money is left in the budget for the month (budget allowance – spending).
- For transaction details and budget category breakdown, just log into Mint. These text messages focus on the overall spending allowance and savings accumulation only.
- On the 1st of each month, any remaining budget (or negative overage) is added to the savings bucket, along with the monthly savings target (savings = savings + budget_remaining + savings_target).

Current assumptions
-------------------

- Your budget totals to a fixed dollar amount each month.
- Budget category allocations are fungible (you can decide to spend less in one area to spend more in another), so only the total matters in the end.
- A single Mint account tracks all budget-related spending.
- Rolling budget: Over- or under-spending from the monthly budget goes into/out of a savings bucket.

Additional features
-------------------

- Exclude Mint categories of your choice from counting as budget spending.
- Send a text to one person for review (e.g. for things that should be ignored) before sending the final version to another person.

Example usage
-------------

- Default ignore categories are used:
    - Transactions categorized as _Income_ are ignored. Income is already expected and accounted for in the budget.
    - Transactions categorized as _Credit Card Payment_ are ignored to avoid double-counting spending, as credit card transactions are also tracked.
    - Transactions categorized as _Hide from Budgets & Trends_ are also ignored, for various reasons.
- Two roles: The money manager and the primary spender.
    - The money manager takes care of retirement savings allocations, investments, transfers, etc.
    - The primary spender does most of the shopping and wants to make sure spending stays on target.
- At 9pm, the money manager receives a budget update text. If anything looks surprising, that person checks Mint and adjust ignore categories if needed.
- At 8am, the primary spender receives a budget update text. Since the numbers were reviewed the night prior, there should be fewer surprises.
- To reduce end-of-month surprise expenses, the money manager works to move automatic payments toward the beginning of the month.
- To handle fixed monthly expenses that may fall on either side of a month boundary (e.g. rent check may be deposited on the 30th or 1st), the money manager creates a Mint rule to assign a special category to these transactions, ignores the category in daily-budget-texts, and reduces the monthly spending allowance by this amount, as if it's automatically spent each month.

[Mint]: https://mint.com

Status
------

Early proof of concept

Pulls transactions, and prints a message showing the remaining spending allowance from a [fixed, example budget amount](https://git.io/Jvri2).

1. Set up dependencies (once)

        bin/setup

2. Pull latest transactions, and output an example message.

        bin/pull-transactions --verbose $MINT_EMAIL
        bin/calculate-text $MINT_EMAIL
