Development
===========

Application Architecture
------------------------

- bin/setup: (bash) Set up dependencies: mintapi, keyring, chromedriver
- bin/pull-transactions: (bash) A thin wrapper for calling mintapi (CLI tool) with the right arguments to write latest transactions to a json file associated with the mint email. Mintapi uses the keyring library to persist credentials after initial login.
- lib/total_spending.js: Take transaction json and ignore_categories as input, and export the current and prior month’s spending.
- lib/budget_calculator.js: Take monthly spending json, configured savings bucket offset and budget targets (spending allowance and savings), and monthly spending history as input. Calculate how much budget allowance remains this month. Calculate savings bucket based on all previous spending + budgeted savings + configured savings bucket offset. Export budget status numbers (the month’s remaining allowance, percentage used, and savings bucket total.
- lib/notify.js: Take the resulting numbers and recipient phone numbers as input. Generate and send a notification message.
- lib/storage.js: Historical mint-email-spending DB.
- lib/index.js: Take mint email and recipients as input. Glue/Controller: Check dependencies and configuration. Execute pull-transactions. Update persisted spending history. Notify the given recipients using the calculated numbers.
- bin/sms-update: (js) Executable wrapper script for index.js, taking mint email and SMS recipients as arguments. Intended to be run by a cron service, avoiding the need for a persistent daemon process.
