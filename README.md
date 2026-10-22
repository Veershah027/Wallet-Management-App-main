# Wallet Management App

A command-line expense tracker in Python backed by PostgreSQL. Users create an account, add money to a wallet, log purchases, issue refunds, see their balance and purchase history, and export their history to a text file.

![Python](https://img.shields.io/badge/python-3.10%2B-blue?logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-psycopg2-336791?logo=postgresql&logoColor=white)

## Features

- **Accounts**: sign up and log in, with an optional "stay logged in" session saved per device
- **Wallet**: add money and check your balance (shown in green when positive and red when negative)
- **Purchases**: log an item and its price; the balance updates automatically
- **Refunds**: remove a purchase by product ID and get the money back in the wallet
- **History and export**: view your timestamped purchase history or export it to `history.txt`
- **Terminal UI**: ANSI colors and automatic screen clearing on Windows, macOS and Linux

## Tech stack

- **Language:** Python 3.10+ (uses `match` / `case`)
- **Database:** PostgreSQL via `psycopg2`, with **parameterized queries** throughout
- **Config:** `python-dotenv` for database credentials

## Database schema

```text
users      (user_id SERIAL PK, username UNIQUE, password)
wallet     (user_id PK → users ON DELETE CASCADE, balance)
purchases  (product_id SERIAL PK, user_id → users, product_name, product_price, purchase_date DEFAULT NOW())
```

## Run it locally

```bash
git clone https://github.com/Veershah027/Wallet-Management-App-main.git
cd Wallet-Management-App-main
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt

cp .env.example .env              # then set your PostgreSQL user, password and database
python initialize_database.py     # creates the users, wallet and purchases tables
python WalletManagement.py
```

## Project structure

```text
WalletManagement.py     Main menu loop
signupLogin.py          Sign-up / login flow
database.py             All PostgreSQL operations (parameterized)
initialize_database.py  Creates the schema
session.txt             Stores the logged-in user ID if "stay logged in" is chosen
```

## Next steps

- Hash passwords with `bcrypt` instead of storing them as entered
- Store money as `NUMERIC` instead of `FLOAT` to avoid rounding errors
- Add spending categories and monthly summaries
- Add unit tests for the database layer
