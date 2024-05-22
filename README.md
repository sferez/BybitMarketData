# Bybit Market Data Collector

This repository serves as a collection point for market data from the Bybit cryptocurrency exchange, aimed at
facilitating further analysis or model creation using AI techniques.

## Overview

- [Data Collection](#data-collection)
    - [Tickers](#tickers)
    - [Order Book](#order-book)
    - [Trades](#trades)
    - [Liquidations](#liquidations)
- [Data Storage](#data-storage)
- [JSONL Structure](#jsonl-structure)
- [Usage](#usage)
- [Disclaimer](#disclaimer)
- [Contributing](#contributing)
- [License](#license)

The data collected includes various types of market events such as `trades`, `liquidations`, `ticker informations`,
and `order
book updates`. All the data is sourced from the public Bybit WebSocket.

Currently, the data is available for `BTC`, `ETH`, and `SOL` on the Future market. Data is logged every second, ensuring
a
high level of granularity. The data is stored in JSONL format. Data is uploaded every few days to ensure the data is up
to date.

## Data Collection

Start date: 12th February 2024: 1707755825000

The data is collected through the public Bybit WebSocket connected to the Future market (wss:
//stream.bybit.com/v5/public/linear)

### Tickers

The ticker data is collected from the **tickers.{symbol}** channel, which provides the following information:

- `symbol`: the trading pair
- `tickDirection`: the direction of the last tick
- `price24hPcnt`: the percentage change in price over the last 24 hours
- `lastPrice`: the last traded price
- `prevPrice24h`: the price 24 hours ago
- `highPrice24h`: the highest price over the last 24 hours
- `lowPrice24h`: the lowest price over the last 24 hours
- `prevPrice1h`: the price 1 hour ago
- `markPrice`: the mark price
- `indexPrice`: the index price
- `openInterest`: the open interest
- `openInterestValue`: the open interest value
- `turnover24h`: the turnover over the last 24 hours
- `volume24h`: the volume over the last 24 hours
- `nextFundingTime`: the next funding time
- `fundingRate`: the funding rate
- `bid1Price`: the best bid price
- `bid1Size`: the best bid size
- `ask1Price`: the best ask price
- `ask1Size`: the best ask size

#### Example

The following JSON data is dumped in the JSONL file every second with the updated ticker information:

```json
{
  "t": 1707751741002,
  "d": {
    "symbol": "BTCUSDT",
    "tickDirection": "PlusTick",
    "price24hPcnt": "0.019519",
    "lastPrice": "49004.10",
    "prevPrice24h": "48065.90",
    "highPrice24h": "49259.70",
    "lowPrice24h": "47712.80",
    "prevPrice1h": "48179.80",
    "markPrice": "49004.50",
    "indexPrice": "48987.28",
    "openInterest": "63082.064",
    "openInterestValue": "3091305005.29",
    "turnover24h": "5924202545.7277",
    "volume24h": "122568.8290",
    "nextFundingTime": "1707753600000",
    "fundingRate": "0.0001",
    "bid1Price": "49004.00",
    "bid1Size": "0.106",
    "ask1Price": "49004.10",
    "ask1Size": "6.035"
  }
}
```

### Order Book

The order book data is collected from the **orderbook.{depth}.{symbol}** channel, using a depth of 200. The data
provides the following information:

- `b`: the bid side of the order book
- `a`: the ask side of the order book

#### Example

The following JSON data is dumped in the JSONL file every second with the updated order book information:

```json
{
  "t": 1707752086000,
  "d": {
    "b": {
      "49400.00": "10.749",
      "49396.80": "0.020",
      "49392.60": "0.001",
      "49392.50": "0.342"
      " ... {200 entries}": ""
    },
    "a": {
      "49400.10": "0.001",
      "49400.20": "0.001",
      "49400.30": "0.001",
      "49400.40": "0.001",
      " ... {200 entries}": ""
    }
  }
}
```

### Trades

The trade data is collected from the **trade.{symbol}** channel, which provides the following information:

- `T`: the timestamp of the trade
- `s`: the trading pair
- `S`: the side of the trade (Buy or Sell)
- `v`: the quantity of the trade
- `p`: the price of the trade
- `L`: the tick direction of the trade
- `i`: the trade ID
- `BT`: whether the trade is a block trade

#### Example

The following JSON data is dumped in the JSONL file every second with the updated trade information of the last second:

```json
{
  "t": 1707752085000,
  "d": [
    {
      "T": 1707752083964,
      "s": "BTCUSDT",
      "S": "Sell",
      "v": "0.003",
      "p": "49407.90",
      "L": "ZeroMinusTick",
      "i": "2c994d6a-0804-52dd-94db-091619469f02",
      "BT": false
    },
    {
      " ... all trades in the last second": ""
    }
  ]
}
```

### Liquidations

The liquidation data is collected from the **liquidation.{symbol}** channel, which provides the following information:

- `updateTime`: the timestamp of the liquidation
- `symbol`: the trading pair
- `side`: the side of the liquidation (Buy or Sell). Buy liquidations are long positions being liquidated.
- `size`: the quantity of the liquidation
- `price`: the price of the liquidation

#### Example

The following JSON data is dumped in the JSONL file every second with the updated liquidation information of the last
second:

```json
{
  "t": 1707752085000,
  "d": [
    {
      "updateTime": 1707752083964,
      "symbol": "BTCUSDT",
      "side": "Buy",
      "size": "0.003",
      "price": "49407.90"
    },
    {
      " ... all liquidations in the last second": ""
    }
  ]
}
```

## Data Storage

The collected data is stored in JSONL format, with each day's data organized into separate directories. The data is
logged every second, ensuring a high level of granularity.

### Directory Structure

- **/data**
    - **/COIN**
        - **/DAY**
            - *zipped jsonl files*

## JSONL Structure

Each JSONL entry follows a structure where:

- `t`: represents the timestamp of the event
- `d`: contains the event data

## Known Issues

| Date                    | Timestamp                       | Nbr Ticks | Details                                               |
|-------------------------|---------------------------------|-----------|-------------------------------------------------------|
| 2024-02-12              | 1707772970001<br/>1707773030020 | 61        | Data is potentially incomplete due to a network issue |
| 2024-02-22              | All day                         | All day   | Lost data                                             |
| 2024-02-25              | 1708827432002<br/>1708828412000 | 981       | Data is potentially incomplete due to a network issue |
| 2024-02-28              | 1709099834000<br/>1709100721001 | 888       | Data is potentially incomplete due to a network issue |
| 2024-03-04              | 1709512392001<br/>1709512967001 | 576       | Data is potentially incomplete due to a network issue |
| 2024-03-06              | 1709692315001<br/>1709693284000 | 970       | Data is potentially incomplete due to a network issue |
| 2024-03-21              | 1710983701000<br>1710984640000  | 940       | Data is potentially incomplete due to a network issue |
| 2024-03-21              | 1711034885002<br>1711035136001  | 252       | Data is potentially incomplete due to a network issue |
| 2024-03-24              | 1711247020000<br>1711248009000  | 990       | Data is potentially incomplete due to a network issue |
| 2024-03-24              | 1711238975001<br>1711239321002  | 347       | Data is potentially incomplete due to a network issue |
| 2024-03-26              | 1711414123000<br>1711414239001  | 117       | Data is potentially incomplete due to a network issue |
| 2024-03-26              | 1711464233001<br>1711464275000  | 43        | Data is potentially incomplete due to a network issue |
| 2024-03-27              | 1711500402000<br>1711500521001  | 120       | Data is potentially incomplete due to a network issue |
| 2024-03-29              | 1711679154000<br>1711680133001  | 980       | Data is potentially incomplete due to a network issue |
| 2024-03-29              | 1711671054001<br>1711671400001  | 347       | Data is potentially incomplete due to a network issue |
| 2024-03-31 - 2024-05-06 | All days                        | All days  | Lost data                                             |
| 2024-05-06              | 1715037365002<br>1715038229000  | 865       | Data is potentially incomplete due to a network issue |
| 2024-05-08              | 1715181908001<br>1715184785001  | 34        | Data is potentially incomplete due to a network issue |
| 2024-05-09              | 1715261548004<br>1715268563000  | 40        | Data is potentially incomplete due to a network issue |
| 2024-05-10              | 1715343521002<br>1715376321000  | 48        | Data is potentially incomplete due to a network issue |
| 2024-05-13              | 1715642017001<br>1715642363001  | 347       | Data is potentially incomplete due to a network issue |
| 2024-05-15              | 1715751502001<br>1715752472000  | 971       | Data is potentially incomplete due to a network issue |
| 2024-05-15              | 1715784042001<br>1715784099001  | 58        | Data is potentially incomplete due to a network issue |
| 2024-05-15              | 1715791437001<br>1715791486001  | 50        | Data is potentially incomplete due to a network issue |
| 2024-05-19              | 1716097200000<br>1716098182999  | 984       | Data is potentially incomplete due to a network issue |
| 2024-05-21              | 1716280086000<br>1716280142001  | 57        | Data is potentially incomplete due to a network issue |

## Usage

To use the data collected in this repository, simply clone or download the repository and access the relevant JSONL
files stored in the `/data` directory.

## Disclaimer

This repository is provided for educational and research purposes only. The data collected from Bybit exchange is
publicly available, and usage should comply with Bybit's terms of service and any relevant regulations.

## Contributing

Contributions to improve the data collection process or enhance the repository's functionality are welcome. Please open
an issue or pull request to discuss any proposed changes.