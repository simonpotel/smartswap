
# Smartswap

<img src="https://github.com/simonpotel/smartswap/blob/60d883cf4e96af5005bfeb8d5061aa992c14cafd/logo.jpeg" width="250" height="250">

## 📈 Manual Positions
The aim of this project is to be able to manage your real crypto funds, directly in the blockchain using smartcontracts.

It offers users the possibility of managing positions on your wallets.
You can create positions **manually**, indicating pairs, enter price, take profit, stop loss, gas price limit, select the network, smart contract used or/native eth and the function.
You can carry out a market analysis, and make your own transaction using only a smart contract, without going through big structures.

## 🤖 Trading Bot
Possibility of activating a bot on the wallet, which you configure for your wallet and define your investment fund. The bot then takes care of all the calculations and transactions.
The bot uses a database that collects values from the pairs you select as a user. Then you have a website at your disposal to get a concrete visual on the peer, on the smart contract you're using. The values are exact and calculated. 

Then, it determines a bullish trend, then follows other recoded indicators, if the situation seems favorable by comparing with the trends of layers 1, it plans transactions with risks of gains higher than losses.

## 🤖 Discord Bot
The project has a bot discord, which makes it easier for users to create positions, follow them, get news and modify them.

For administration, the bot uses a C-coded system: tmux on linux, which lets you create consoles within consoles. 

In this way, it takes care of relaunching each wallet script, enabling you to carry out all your transactions for each wallet in real time. It can also control the host, update scripts, etc.

## 🔗 Authors

- [@simonpotel](https://www.github.com/simonpotel) -> Transactions logic, Bot Strategy, project structure and host SRE.
- [@nicolas-potel](https://www.github.com/nicolas-potel) -> Website creation. Graphic and visual part of the project. Main code maintenance and improvement

## 🗂️ Reperitories

| Name | Git                | Description                |
| :-------- | :------------------------- | :------------------------- |
| `smartswap` | https://github.com/simonpotel/smartswap|Main project reperitories, project structure
| `DEXcryptoLib` | https://github.com/simonpotel/DEXcryptoLib|Library to interract with Web3, DEX, Smart Contracts
| `smartswap-web` | https://github.com/simonpotel/smartswap-web| A website for exploiting useful data to take positions. (pairs data)
| `smartswap-discord` | https://github.com/simonpotel/smartswap-discord|
| `smartswap-internal` | https://github.com/simonpotel/smartswap-internal| Main logic. Follow a progression with the database and perform interractions with web3 when necessary/right time.

- ## 📊 How does it work?
Manual:
The user creates his position on the discord in the lounge that corresponds to the one configured in the database for the X wallet.
The order is then retrieved by the discord bot, processed and inserted into the database.
A smartswap-internal python script takes care of this, scanning the current transactions in the database, and if the conditions are met to enter the position, then it performs the transactions, and vice versa to exit.

For the trading bot:
A script runs continuously and retrieves the prices for each of the pairs configured by the users.
Then, if the bot is activated on the wallet, the smartswap-internal script will analyze and calculate whether it's the right time to take a position or not. If it is, it will itself insert the transaction into the database, then the same script will carry out the transactions.

## Environment
__Databases__:
- smartswap : Stock clients' infos, wallets infos and their access, their state, bot usage
- smartswap-positions : One table per wallet. For each wallet, orders are displayed.
- smartswap-data : One table for one pair. Data collection (date/time, price) for each pair configured and their routers.

## Deployment

To create the databases:
```
CREATE USER 'smartswap'@'localhost' IDENTIFIED BY '?';

CREATE DATABASE smartswap;
CREATE DATABASE smartswap_positions;
CREATE DATABASE smartswap_data;

GRANT ALL PRIVILEGES ON smartswap.* TO 'smartswap'@'localhost';
GRANT ALL PRIVILEGES ON smartswap_positions.* TO 'smartswap'@'localhost';
GRANT ALL PRIVILEGES ON smartswap_data.* TO 'smartswap'@'localhost';

FLUSH PRIVILEGES;
```

To setup smartswap database:
```
USE smartswap;
CREATE TABLE wallets (
    name VARCHAR(255) PRIMARY KEY,
    address VARCHAR(255),
    private_key VARCHAR(255),
    discord_chanmsglogs_id VARCHAR(255)
);

CREATE TABLE clients (
    user CHAR(255) PRIMARY KEY,
    discord_user_id VARCHAR(255),
    password VARCHAR(32)
);

CREATE TABLE wallets_access (
    client_user CHAR(255),
    wallet_name VARCHAR(255),
    PRIMARY KEY (client_user, wallet_name),
    FOREIGN KEY (client_user) REFERENCES clients(user),
    FOREIGN KEY (wallet_name) REFERENCES wallets(name)
);
```

## Importants Advices
> [!CAUTION]
> - smartswap-internal : Everytime you create and run a wallet, u need to configure a tmux session using smartswap-discord bot which gonna run smartswap-internal script for a single wallet.
> - The project is still in developement and not in a stable version, the main smartswap-internal script is not yet available to the public.
> - The project is a collaborative one, learning as you go the principles of Web3, its environment and its technologies. This means that each of the project's reperitories can be totally modified when a new system is discovered.

## Rights
- Project logo made by Bing (Microsoft)
- Project is using multiple public libraries on python such as Web3.
