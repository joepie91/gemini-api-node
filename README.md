# @joepie91/gemini-api

Gemini cryptocurrency exchange API wrapper for Node.js

## This is a fork!

This fork adds more API methods as-needed, and updates some dependencies that were vulnerable. The original library can be found [here](https://github.com/mjesuele/gemini-api-node/).

## Installation

```
yarn add @joepie91/gemini-api
```

## Usage

Clients for both the [REST API](https://docs.gemini.com/rest-api/) and
[streaming WebSocket API](https://docs.gemini.com/websocket-api/) are included.
Private endpoints as indicated in the API docs require authentication with an API
key and secret key.

### Example usage:

```javascript
import GeminiAPI from 'gemini-api';

const restClient = new GeminiAPI({ key, secret, sandbox: false });
const websocketClient =
  new GeminiAPI.WebsocketClient({ key, secret, sandbox: false });

restClient.getOrderBook('btcusd', { limit_asks: 10, limit_bids: 10 })
  .then(console.log)
  .catch(console.error);

websocketClient.openMarketSocket('btcusd', () => {
  websocketClient.addMarketMessageListener(data =>
    doSomethingCool(data)
  );
});

// The methods are bound properly, so feel free to destructure them:
const { getTicker } = restClient;
getTicker('btcusd')
  .then(data =>
    console.log(`Last trade: $${data.last} / BTC`)
  )
```

## API

### REST
All methods return promises.
* getAllSymbols()
* getTicker(symbol)
* getOrderBook(symbol, params = {})
* getTradeHistory(symbol, params = {})
* getCurrentAuction(symbol)
* getAuctionHistory(symbol, params = {})
* newOrder(params = {})
* cancelOrder({ order_id })
* cancelAllSessionOrders()
* cancelAllActiveOrders()
* getMyOrderStatus({ order_id })
* getMyActiveOrders()
* getMyPastTrades(params = {})
* getMyNotionalVolume()
* getMyTradeVolume()
* getMyAvailableBalances()
* newAddress(currency)

### WebSocket
* openMarketSocket(symbol, onOpen)
* openOrderSocket(onOpen)
* addMarketMessageListener(listener)
* addOrderMessageListener(listener)
* removeMarketMessageListener(listener)
* removeOrderMessageListener(listener)
* addMarketListener(event, listener)
* addOrderListener(event, listener)
* removeMarketListener(event, listener)
* removeOrderListener(event, listener)

## To Do
* Improved documentation
* More robust error handling

Feedback and pull requests welcome!

## Changelog

### v3.0.0 (July 12, 2019)

- __BREAKING:__ Support for Node.js < 8 dropped.
- Added `getMyNotionalVolume` method.
- Upgraded `axios`, `ws`, `lodash`, and various other dependencies; this fixes the audit warnings.
- Added `watch` npm/Yarn command for development
