# ArbBot
A simple inter-exchange cryptocurrency arbitrage bot.

# Overview
Cryptocurrency markets are volatile, and there are often cases in which the
trade price of a certain pair (e.g. LTC-BTC) differs across multiple exchanges. 
If an application can ingest trading data, interpret it, and act 
upon it fast enough, then there is potential for daily gains on a 
cryptocurrency portfolio consisting of multiple cryptocurrencies.

The goal of ArbBot is not to make someone or a group of people cryptocurrency 
rich; instead, it's to provide small but meaningful daily / bi-daily gains on 
a cryptocurrency portfolio. The intended user of ArbBot is someone who is 
comfortable holding a diverse portfolio of different coins for a long period of 
time (as far as crypto goes), tentatively 6 months, barring apocalyptic-level 
changes in the market. There will be a crash, and ArbBot 
is not intended to provide any exit protection in the event of a crash. 
Manual trading is required for that.  

# Terminology
**Pair:** LTC-BTC, ZEC-BTC, ETC-ETH, etc.  
**Exchange:** A cryptocurrency exchange, e.g. Poloniex, Bittrex, 
GDAX, Kraken   
**Order Book:**: a complete list of "buy orders" and "sell orders," sourced 
directly from the exchanges.  
**Volume:**: Amount of cryptocurrency traded during a given timeframe  
**Transaction Speed:** Time between sending cryptocurrency from wallet A and 
receiving it at wallet B  
**Transaction Cost:** The cost of sending a given cryptocurrency from point A 
to point B

## Exchanges
Bittrex, GDAX (Coinbase), Gemini, Kraken, Poloniex

## Coins
### Base coins
Bitcoin (BTC)  
Ethereum (ETH)

## Altcoins (i.e. not BTC or ETH)
**High transaction speed, low transaction cost:**  
Litecoin (LTC)  
Stellar Lumens (XLM)  
Ripple (XRP)  

**Because Ty holds these anyways:**  
ZCash (ZEC)  
Ethereum Classic (ETC)

# Methodology
Equal amounts of BTC and ETH are placed on all exchanges used by ArbBot. 
Altcoins are also placed on the exchanges, amounts are in sum roughly equal to 
the amount of BTC + ETH on each exchange.

ArbBot pulls order book data from exchanges that are U.S. based, both for 
volume and trust reasons (Chinese exchanges cannot be trusted). The pull is in 
a synchronized manner so that timestamps match up, making price comparisons as
accurate as possible. Order book data must be used because relying on, say, the 
last price and volume in a given direction does not guarantee that an order 
placed by ArbBot will be filled immediately. Using order book data allows us to 
simply buy from the sell side of the book or sell to the buy side of the book, 
guaranteeing a trade will execute as long as someone or another bot hasn't beat 
us to it.  

Once a discrepancy above a certain threshold between the price of a currency on 
the buy side of the order book at Exchange A and the sell side of the order book 
on Exchange B, ArbBot places orders in both exchanges. The exchanges should 
output an `order_id` once an order is placed, and ArbBot will then monitor that 
`order_id` to ensure it has completed. Since altcoins are even more volatile 
than Bitcoin or Ethereum, ArbBot might try to wait for confirmation that the 
sell order of a given altcoin has occurred before placing a buy order. If BTC 
or ETH is particularly bearish on a given day, this logic could be reversed.  

At a TBD time interval, the pots of cryptocurrency on each exchange is evened 
out, and the process repeats. For security reasons, this part will be manual 
initially. Exchanges have the ability to restrict what can be done via API, 
and since I know very little about security I'm going to disable API withdrawls.

## Complications
1. Daily withdrawl limits on exchanges
2. Enough buy and sell volume at discrepancy pricing to realize gains
3. Trading fees, BTC and ETH transaction fees
4. Capital gains record keeping
5. Risk inherent in holding cryptocurrency on exchanges (i.e. it's always best 
to hold your own private keys)
6. Risk of devaluation of altcoin, or market crash
7. Wallet address changes on exchanges
8. Other people are doing this, does that mean we can't do it too?
9. Exchanges being abruptly shutdown (Cryptsy, MtGox, possibly anything in China)

## Potential Solutions to Complications
1. Look for higher percentage gains at a lower frequency and then stagger transfers
2. Trade more than one pair
3. ETH is generally cheaper to transfer than BTC
4. CSV's and [Bitcoin.tax](https://bitcoin.tax)
5. No solution as of right now. Some percentage of the portfolio should be kept 
off exchanges in a secure manner, which limits potential profits. Once ArbBot is 
live, we might be able to capture data from its activities along with other data 
to build some sort of momentum indicator that indicates discrepancies are likely 
to occur.
6. See (2) + periodically liquidate everyting to BTC, ETH -> USD at some 
interval when advantageous to do so.
7. Figure it out
8. Get someone more skilled at automation than Ty to help with ArbBot
9. If exchanges get shut down, everything is fucked.