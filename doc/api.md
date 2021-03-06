TPSM API
==============



Web (node.js)
-----------------

### function parseComment(comment){}
Takes the necessary actions given the comment submitted by the user in the chat window.

### function createOrder(userid, action, orderType, ticker, qty[, price]) {}
Creates order with specified parameters:
* userid - name of user
* action - either 'buy' or 'sell'
* orderType - either 'mkt', 'limit', 'stop', or 'stop_loss'
* ticker - ticker
* qty - number of stocks
* price - price of order if limit, stop, or stop_loss
Prints error to chat window if unsuccessful

### function getPortfolioDetails() {}
* GET /portfolio_details
* Content-Type: application/json
* Returns JSON object containing portfolio details.
```
{ startingValue: 10000.00,
  currentValue: 20430.24,
  holdings: [{...}, ...],
  cash: 9100.20,
  numParticipants: 10  // see below }
```

### function getHoldings() {}
* GET /holdings
* Content-Type: application/json
* Returns array of JSON objects in the following format:
```
[{ ticker: 'AAPL',
   stocks: 123,
   lastPrice: 70.92,
   holdingVal: 8723.16,  // = 123 * 70.92
   holdingPct: 0.12  // = 8723.16 / totalPortfolioValue },
 { ticker: 'GOOG', ... },
 ...]
```
The list will be sorted descending by holdingPct. 


Data Services (Python / Flask)
--------------------------------

### def generate_portfolio_prices(tickers, ticker)
* GET /prices?tickers=AAPL,GOOG,YHOO
** Returns JSON object whose properties are tickers containing arrays of 100 ticks of prices:
```
{ GOOG: [602.66, 602.54, 602.43, ...],
  AAPL: [10.20, 10.24, 10.01, ...],
 ... }
```
* GET /prices?ticker=AAPL
** Returns a JSON array of 100 prices for that ticker:
```
[10.20, 10.24, 10.01, ...]
```

Chat Commands
----------------------

The format of commands the application will recognize is:
```
!action ticker qty type [price]
```
Examples:
```
!buy goog 100 mkt
!sell aapl 200 limit 50.19
```