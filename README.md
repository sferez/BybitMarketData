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

The data collected includes various types of market events such as trades, liquidations, ticker information, and order
book updates. All the data is sourced from the public Bybit WebSocket.

Currently, the data is available for BTC, ETH, and SOL on the Future market. Data is logged every second, ensuring a
high level of granularity. The data is stored in JSONL format. Data is uploaded every few days to ensure the data is up
to date.

## Data Collection

The data is collected through the public Bybit WebSocket connected to the Future market (wss:
//stream.bybit.com/v5/public/linear)

### Tickers

The ticker data is collected from the **tickers.{symbol}** channel, which provides the following information:

- 'symbol': the trading pair
- 'tickDirection': the direction of the last tick
- 'price24hPcnt': the percentage change in price over the last 24 hours
- 'lastPrice': the last traded price
- 'prevPrice24h': the price 24 hours ago
- 'highPrice24h': the highest price over the last 24 hours
- 'lowPrice24h': the lowest price over the last 24 hours
- 'prevPrice1h': the price 1 hour ago
- 'markPrice': the mark price
- 'indexPrice': the index price
- 'openInterest': the open interest
- 'openInterestValue': the open interest value
- 'turnover24h': the turnover over the last 24 hours
- 'volume24h': the volume over the last 24 hours
- 'nextFundingTime': the next funding time
- 'fundingRate': the funding rate
- 'bid1Price': the best bid price
- 'bid1Size': the best bid size
- 'ask1Price': the best ask price
- 'ask1Size': the best ask size

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

- 'b': the bid side of the order book
- 'a': the ask side of the order book

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
      //      ... {200 entries}
    },
    "a": {
      "49400.10": "0.001",
      "49400.20": "0.001",
      "49400.30": "0.001",
      "49400.40": "0.001"
      //... {200 entries}
    }
  }
}
```

### Trades

The trade data is collected from the **trade.{symbol}** channel, which provides the following information:

- 'T': the timestamp of the trade
- 's': the trading pair
- 'S': the side of the trade (Buy or Sell)
- 'v': the quantity of the trade
- 'p': the price of the trade
- 'L': the tick direction of the trade
- 'i': the trade ID
- 'BT': whether the trade is a block trade

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
    }
    //    ... {all trades in the last second}
  ]
}
```

### Liquidations

The liquidation data is collected from the **liquidation.{symbol}** channel, which provides the following information:

- 'updateTime': the timestamp of the liquidation
- 'symbol': the trading pair
- 'side': the side of the liquidation (Buy or Sell). Buy liquidations are long positions being liquidated.
- 'size': the quantity of the liquidation
- 'price': the price of the liquidation

#### Example

The following JSON data is dumped in the JSONL file every second with the updated liquidation information of the last second:

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
    }
    //    ... {all liquidations in the last second}
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
            - **/type** (e.g. trades, liquidations, orderbook, ticker)
                - *jsonl files* (potentially split into several parts of 100MB each)

## JSONL Structure

Each JSONL entry follows a structure where:

- `t`: represents the timestamp of the event
- `d`: contains the event data

## Usage

To use the data collected in this repository, simply clone or download the repository and access the relevant JSONL
files stored in the `/data` directory.

## Disclaimer

This repository is provided for educational and research purposes only. The data collected from Bybit exchange is
publicly available, and usage should comply with Bybit's terms of service and any relevant regulations.

## Contributing

Contributions to improve the data collection process or enhance the repository's functionality are welcome. Please open
an issue or pull request to discuss any proposed changes.

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.