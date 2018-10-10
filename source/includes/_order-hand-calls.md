# Order-handling calls




## CancelAllOrders


Cancels all open matching orders for the specified instrument, account, user (subject to permission level) or a combination of them on a specific Order Management System. User and account permissions govern cancellation actions. See “Permissions” . For more information on quotes and orders, see the explanation of “Quotes and Orders” .


**Note:**      Multiple users may have access to the same account.


| **Specifying   this information…** | **Cancels all   orders for…** |                |                                                              |
| ---------------------------------- | ----------------------------- | -------------- | ------------------------------------------------------------ |
| **User   37**                      | **Acc’t   14**                | **Instr   25** |                                                              |
| **X**                              | **X**                         | **X**          | Account #14 belonging to user #37 for instrument #25.        |
| **X**                              | **X**                         |                | Account #14 belonging to user #37 for all instruments.       |
| **X**                              |                               | **X**          | All accounts belonging to user #37 for instrument #25.       |
| **X**                              |                               |                | All accounts belonging to user #37 for all instruments.      |
|                                    | **X**                         | **X**          | All users of account #14 for instrument #25.                 |
|                                    | **X**                         |                | All users of account #14 for all instruments.                |
|                                    |                               | **X**          | All accounts of all users for instrument #25. (requires   special permission) |
|                                    |                               |                | All accounts of all users for all instruments (requires   special permission) |

 

### Request


```
	{
		“AccountId”: 0, // conditionally optional 
		“UserId”: 0, // conditionally optional 
		“OMSId”: 0
		“InstrumentId”: 0, // conditionally optional
	}
```



Where:

| **String**   | **Value**                                                    |
| ------------ | ------------------------------------------------------------ |
| AccountId    | **integer.** The account for   which all orders are being canceled. Conditionally optional. |
| UserId       | **integer.** The ID of the user   whose orders are being canceled. Conditionally optional. |
| OMSId        | **integer.** The Order   Management System under which the account operates. Required. |
| InstrumentId | **long integer.** The ID of the instrument for which all orders are being cancelled Conditionally optional |



### Response

The response to **CancelAllOrders** verifies that the call was received, not that the orders have been canceled successfully. Individual event updates to the user show orders as they cancel. To verify that an order has been canceled, use **GetOrderStatus** or **GetOpenOrders**. :

```
    {
      “result”: true, 
      “errormsg”: “”, 
      “errorcode”: 0, 
      “detail”: “”,
    }
```

 



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.**   If   the call has been successfully received by the Order Management   System, result is *true*; otherwise, it is *false*. |
| errormsg   | **string.** A   successful receipt of the call returns null; the *errormsg* parameter for an unsuccessful call returns one of the   following messages:   Not   Authorized (errorcode 20) Invalid Request (errorcode 100) Operation Failed   (errorcode 101) Server Error (errorcode 102)   Resource Not Found (errorcode 104) |
| errorcode  | **integer.** A   successful receipt of the call returns 0. An unsuccessful receipt of the call   returns one of the *errorcodes* shown   in the *errormsg* list. |
| detail     | **string.** Message   text that the system may send. The content of this parameter is usually *null*. |







## CancelOrder


Cancels an open order that has been placed but has not yet been executed. Only a trading venue operator can cancel orders for another user or account. See the explanation of “Quotes and Orders” .



### Request

The OMS ID and the Order ID precisely identify the order you wish to cancel. The Order ID is unique across an OMS.

If you specify the OMS ID and the Account ID, you must also specify at least the Client Order ID. The OMS is unable to identify the order using only the OMS ID and the Client Order ID, as the Client Order ID may not be unique.



```
    {
      “OMSId”: 0,
      “AccountId”: 0 // conditionally optional
      “ClientOrderId”: 0 // conditionally optional
      “OrderId”: 0, // conditionally optional
    }
```




Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| OMSId         | **integer.**   The   Order Management System on which the order exists. Required. |
| AccountId     | **integer.** The   ID of account under which the order was placed. Conditionally optional. |
| ClientOrderId | **long   integer.** A user-assigned ID for the order (like a purchase-order number   assigned by a company).   ClientOrderId defaults to 0. Conditionally optional. |
| OrderId       | **long   integer.** The order to be cancelled. Conditionally optional. |

  


### Response


The response to **CancelOrder** verifies that the call was received, not that the order has been canceled successfully. Individual event updates to the user show order cancellation. To verify that an order has been canceled, call **GetOrderStatus** or **GetOpenOrders**. :



```
    {
      “result”: true,
      “errormsg”: “”,
      “errorcode”: 0,
      “detail”: “”,
    }
```




Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.**   Returns   true if the call to cancel the order has been successfully   received, otherwise returns false. |
| errormsg   | **string.**   A successful receipt of a call to cancel an order returns   null; the errormsg parameter for an unsuccessful call to cancel an order   returns one of the following messages:   <br />Not   Authorized (errorcode 20) <br />Invalid Request (errorcode 100) <br />Operation Failed   (errorcode 101) <br />Server Error (errorcode 102)   <br />Resource Not Found (errorcode 104) |
| errorcode  | **integer.** A   successfully received call to cancel an order returns 0. An unsuccessfully   recieved call to cancel an order returns one of the errorcodes shown in the   errormsg list. |
| detail     | **string.** Message   text that the system may send. The contents of this parameter are usually   null. |







## CancelReplaceOrder



**CancelReplaceOrder** is single API call that both cancels an existing order and replaces it with a new order. Canceling one order and replacing it with another also cancels the order’s priority in the order book. You can use **ModifyOrder** to preserve priority in the book; but **ModifyOrder** only allows a reduction in order quantity.



**Note:  CancelReplaceOrder** sacrifices the order’s priority in the order book.



### Request



```
    {
      “OMSId”: 0,
      “OrderIdToReplace”: 0,
      “ClientOrdId”: 0,
      “OrderType”: {
        “Options”: [
          “Unknown”,
          “Market”,
          “Limit”,
          “StopMarket”,
          “StopLimit”,
          “TrailingStopMarket”,
          “TrailingStopLimit”,
          “BlockTrade”
        ]
      },
      “Side”: {
        “Options”: [
          “Buy”,
          “Sell”,
          “Short”,
          “Unknown”,
        ]
      },
      “AccountId”: 0,
      “InstrumentId”: 0,
      “TrailingAmount”: 0,
      “LimitOffset”: 0,
      “DisplayQuantity”: 0,
      “LimitPrice”: 0,
      “StopPrice”: 0, // conditionally optional
      “PegPriceType”: {
        “Options”: [
          “Unknown”,
          “Last”,
          “Bid”,
          “Ask”,
          “Midpoint”
        ]
      },
      “TimeInForce”: {
        “Options”: [
          “Unknown”,
          “GTC”,
          “IOC”,
          “FOK”,
        ]
      },
      “OrderIdOCO”: 0,
      “Quantity”: 0,
    }
```



Where:

| **String**       | **Value**                                                    |
| ---------------- | ------------------------------------------------------------ |
| OMSId            | **integer.** The ID of the Order   Management System on which the order is being canceled and replaced by   another order. |
| OrderIdToReplace | **long integer.**   The   ID of the order to replace with this order. |
| ClientOrderId    | **long integer.**   A   user-assigned ID for the new, replacement order (like a   purchase-order number assigned by a   company). This ID is useful for recognizing   future states related to this order.   *ClientOrderId* defaults to 0. |
| OrderType        | **string.** The   type of the replacement order: See Order Types in “Contents common to many   API calls.   <br />0   Unknown   <br />1   Market   <br />2   Limit   <br />3   StopMarket   <br />4   StopLimit   <br />5   TrailingStopMarket   <br />6 TrailingStopLimit   <br />7   BlockTrade |
| Side             | **string.** The side of the replacement order: <br />0   Buy   <br />1   Sell   <br />2   Short   (reserved for future use) <br />3   Unknown   (error condition) |
| AccountId        | **integer.** The ID of the   account under which the original order was placed and the new order will be   placed. |
| InstrumentId     | **integer.** The ID of the   instrument being traded.        |
| TrailingAmount   | **real.** The   offset by which to trail the market in one of the trailing order types. Set   this to the current price of the market to ensure that the trailing offset is   the amount intended in a fast-moving market. |
| LimitPrice       | **real.** The price at   which to execute the new order, if the order is a Limit order. |
| StopPrice        | **real.** The   price at which to execute the new order, if the order is a Stop order (either   buy or sell). |
| PegPriceType     | **string.** When entering a stop/trailing order, set *PegPriceType* to the type of price that pegs the stop.   <br />1   Last   <br />2   Bid   <br />3   Ask   <br />4   Midpoint |
| TimeInForce | **string.**  The period   during which the new order is executable.   <br />0   Unknown (error condition)   <br />1   GTC good ’til canceled   <br />3 IOC immediate   or canceled   <br />4 FOK fill or   kill — fill the order immediately, or cancel it immediately   <br /><br />There may be other settings for   TimeInForce depending on the trading venue. |
| OrderIdOCO  | **integer.** One   Cancels the Other — If the order being canceled in this call is order A, and   the order replacing order A in this call is order B, then *OrderIdOCO* refers to an order C that   is currently open. If order C executes, then order B is canceled. You can   also set up order C to watch order B in this way, but that will require an   update to order C. |
| Quantity    | **real.** The amount of   the order (buy or sell).           |





### Response

The response returns the new replacement order ID and echoes back any replacement client ID  you have supplied, along with the original order ID and the original client order ID.



```
    {
      “ReplacementOrderId”: 1234,
      “ReplacementClOrdId”: 1561,
      “OrigOrderId”: 5678,
      “OrigClOrdId”: 91011,
    }
```





Where:

| **String**         | **Value**                                                    |
| ------------------ | ------------------------------------------------------------ |
| ReplacementOrderId | **integer.**   The   order ID assigned to the replacement order by the server. |
| ReplacementClOrdId | **long   integer.** Echoes the contents of the *ClientOrderId*   value from the request. |
| OrigOrderId        | **integer.**   Echoes   *OrderIdToReplace*, which is the   original order you are replacing. |
| OrigClOrdId        | **long integer.** Provides   the client order ID of the original order (not specified in the requesting   call). |







## GetAccountInfo



Returns detailed information about one specific account belonging to the authenticated user and existing on a specific Order Management System.



### Request



```
    {
      “OMSId”: 0,
      “AccountId”: 0, 
      “AccountHandle”: “”,
    }
```





Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| OMSId         | **integer.**   The   ID of the Order Management System on which the account exists. |
| AccountId     | **integer.** The   ID of the account on the Order Management System for which information will   be returned. |
| AccountHandle | **string.** *AccountHandle* is   a unique user-assigned name that is checked at create time by the Order   Management System. Alternate to Account ID. |





### Response



```
    {
      “OMSID”: 0,
      “AccountId”: 0,
      “AccountName”: “”,
      “AccountHandle”: “”,
      “FirmId”: “”,
      “FirmName”: “”,
      “AccountType”: {
          “Options”: [
          “Asset”,
          “Liability”,
          “ProfitLoss”
        ]
      },
      “FeeGroupID”: 0,
      “ParentID”: 0,
      “RiskType”: {
          “Options”: [
          “Unknown”,
          “Normal”,
          “NoRiskCheck”,
          “NoTrading”
        ]
      },
      “VerificationLevel”: 0,
      “FeeProductType”: {
          “Options”: [
          “BaseProduct”,
          “SingleProduct”
        ]
      },
      “FeeProduct”: 0,
      “RefererId”: 0,
      “SupportedVenueIds”: [
        0
      ],
    }
```



Where:

 

| **String**        | **Value**                                                    |
| ----------------- | ------------------------------------------------------------ |
| OMSId             | **integer.** The ID of the   Order Management System on which the account resides. |
| AccountId         | **integer.** The ID of the   account for which information was requested. |
| AccountName       | **string.** A non-unique   name for the account assigned by the user. |
| AccountHandle     | **string.** *AccountHandle* is   a unique user-assigned name that is checked at create time by the Order   Management System to assure its uniqueness. |
| FirmId            | **string.** An   arbitrary identifier assigned by a trading venue operator to a trading firm   as part of the initial company, user, and account set up process. For   example, Smith Financial Partners might have the ID SMFP. |
| FirmName          | **string.** A longer,   non-unique version of the trading firm’s name; for example,   Smith Financial Partners. |
| AccountType       | **string.** The type of the account for which   information is being returned. One of: <br />Asset   <br />Liability   <br />ProfitLoss   <br /><br />Responses for this string/value pair for Market Participants are almost exclusively Asset. |
| FeeGroupID        | **integer.** Defines   account attributes relating to how fees are calculated and   assessed. Set by trading venue   operator. |
| ParentID          | **integer.** Reserved for   future development.              |
| RiskType          | **string.** One of:   <br />Unkown (an error condition)   <br />Normal   <br />NoRiskCheck <br />NoTrading   <br /><br />Returns Normal for virtually all   market participants. Other types indicate account   configurations assignable by the   trading venue operator. |
| VerificationLevel | **integer.** Verification level   ID (how much verification does this account require) defined by and set by   the trading venue operator for this account. |
| FeeProductType    | **string.** One of:   <br />BaseProduct   <br />SingleProduct   <br /><br />Trading   fees may be charged by a trading venue operator. This value shows whether   fees for this account’s trades are charged in the product being traded (*BaseProduct*, for example BitCoin) or   whether the account has a preferred fee-paying product (*SingleProduct*, for example USD) to use in all cases and   regardless of product being traded. |
| FeeProduct        | **integer.** The ID of the   preferred fee product, if any. Defaults to 0. |
| RefererId         | **integer.** Captures the   ID of the person who referred this account to the trading   venue, usually for marketing   purposes. |
| SupportedVenueIds | **integer array.**   Comma-separated array. Reserved for future expansion. |







## GetAccountPositions

Retrieves a list of positions (balances) for a specific user account running under a specific Order Management System. The trading day runs from UTC Midnight to UTC Midnight. See “The Trading Day” for more information.

 

### Request



```
    {
      “AccountId”:4, 
      “OMSId”: 1
    }
```





Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| AccountId  | **integer.** The   ID of the authenticated user’s account on the Order Management System for   which positions will be returned. |
| OMSId      | **integer.** The   ID of the Order Management System to which the user belongs. A user will   belong only to one OMS. |





### Response

The response returns an array of one or more positions for the account. This example response has returned two positions:



```
    [
      { // first position
        “OMSId”:1,
        “AccountId”:4,
        “ProductSymbol”:”BTC”
        “ProductId”:1
        “Amount”:0,
        “Hold”:0,
        “PendingDeposits”:0,
        “PendingWithdraws”:0,
        “TotalDayDeposits”:0,
        “TotalDayWithdraws”:0,
        “TotalMonthWithdraws”:0
      },
      { //second position
        “OMSId”:1,
        “AccountId”:4,
        “ProductSymbol”:”USD”,
        “ProductId”:2,
        “Amount”:0,
        “Hold”:0,
        “PendingDeposits”:0,
        “PendingWithdraws”:0,
        “TotalDayDeposits”:0,
        “TotalDayWithdraws”:0,
        “TotalMonthWithdraws”:0
      }
    ]
```





Where:

| **String**          | **Value**                                                    |
| ------------------- | ------------------------------------------------------------ |
| OMSId               | **Integer.**   The   ID of the Order Management System (OMS) to which the user   belongs. A user will only ever   belong to one Order Management System. |
| AccountId           | **integer.**   Returns   the ID of the user’s account to which the positions belong. |
| ProductSymbol       | **string.** The   symbol of the product on this account’s side of the trade. For example:   <br />BTC   — BitCoin <br />USD — US Dollar   <br />NZD — New Zealand Dollar   <br /><br />Many   other values are possible depending on the nature of the trading venue. “Products and Instruments” for the difference between these terms. |
| ProductId           | **integer.** The   ID of the product being traded. The system assigns product IDs as they are   entered into [the system. See “Products and Instruments” on for the difference between products and instruments. Use **GetProduct** to return information about the product by its ID. |
| Amount              | **real.**   Unit   amount of the product; for example, 10 or 138.5. |
| Hold                | **real.** Amount   of currency held and not available for trade. A pending trade of 100 units at   $1 each will reduce the amount in the account available for trading by   $100. Amounts on hold cannot be   withdrawn while a trade is pending. |
| PendingDeposits     | **real.**   Deposits   accepted but not yet cleared for trade. |
| PendingWithdraws    | **real.** Withdrawals   acknowledged but not yet cleared from the account. Amounts in *PendingWithdraws* are not available for   trade. |
| TotalDayDeposits    | **real.** Total   deposits on today’s date. The trading day runs between UTC Midnight and UTC   Midnight. |
| TotalDayWithdraws   | **real.** Total   withdrawals on today’s date. The trading day runs between UTC Midnight and   UTC Midnight. |
| TotalMonthWithdraws | **real.** Total   withdrawals during this month to date. The trading day runs between UTC   Midnight and UTC Midnight — likewise a month begins at UTC Midnight on the   first day of the month. |







## GetAccountTrades



Requests the details on up to 200 past trade executions for a single specific user account and its Order Management System, starting at index *i*, where *i* is an integer identifying a specific execution in reverse order; that is, the most recent execution has an index of 0, and increments by one as trade executions recede into the past.

The operator of the trading venue determines how long to retain an accessible trading history before archiving.





### Request



```
    {
      “AccountId”:4, 
      “OMSId”: 1,
      “StartIndex”:0, 
      “Count”:2
    }
```





Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| AccountId  | **integer.**   The   ID of the authenticated user’s account. |
| OMSId      | **integer.** The   ID of the Order Management System to which the user belongs. A user will   belong only to one OMS. |
| StartIndex | **integer.** The   starting index into the history of trades, from 0 (the most recent trade). |
| Count      | **integer.**   The   number of trades to return. The system can return up to 200 trades. |







### Response

The response is an array of objects, each of which represents the account’s side of a trade (either buy or sell). 

The example shows an array of two buy executions.



```
    [
      {
        “TradeTimeMS”: -62135446664520,
        “Fee”: 0,
        “FeeProductId”: 0,
        “OrderOriginator”: 1,
        “OMSId”: 1,
        “ExecutionId”: 1,
        “TradeId”: 1,
        “OrderId”: 1,
        “AccountId”: 4,
        “SubAccountId”: 0,
        “ClientOrderId”: 0,
        “InstrumentId”: 1,
        “Side”: “Buy”,
        “Quantity”: 1,
        “RemainingQuantity”: 0,
        “Price”: 100,
        “Value”: 100,
        “TradeTime”: 1501354796406,
        “CounterParty”: null,
        “OrderTradeRevision”: 1,
        “Direction”: “NoChange”,
        “IsBlockTrade”: false
      },
      {
        “TradeTimeMS”: -62135446664520,
        “Fee”: 0,
        “FeeProductId”: 0,
        “OrderOriginator”: 1,
        “OMSId”: 1,
        “ExecutionId”: 3,
        “TradeId”: 2,
        “OrderId”: 3,
        “AccountId”: 4,
        “SubAccountId”: 0,
        “ClientOrderId”: 0,
        “InstrumentId”: 1,
        “Side”: “Buy”,
        “Quantity”: 1,
        “RemainingQuantity”: 0,
        “Price”: 1,
        “Value”: 1,
        “TradeTime”: 1501354796418,
        “CounterParty”: null,
        “OrderTradeRevision”: 1,
        “Direction”: “NoChange”,
        “IsBlockTrade”: false
      }
    ]
```





Where:

| **String**      | **Value**                                                    |
| --------------- | ------------------------------------------------------------ |
| TradeTimeMS     | **long integer.** The date and time   stamp of the trade in Microsoft tick format and UTC time zone. See “Time–and Date-Stamp Formats”. |
| Fee             | **real.** The fee for   this trade in units and fractions of units (a $10 USD fee would be   10.00, a .5-BitCoin fee would be   0.5). |
| FeeProductId    | **integer.** The   ID of the product that denominates the fee. Product types will vary on each   trading venue. See **GetProduct**. |
| OrderOriginator | **integer.** The user ID   of the user who entered the order that caused the trade for   this account. (Multiple users can   have access to an account.) |
| OMSId           | **integer.** The ID of the Order   Management System to which the user belongs. A user will belong only to one   OMS. |
| ExecutionId     | **integer.** The ID of   this account’s side of the trade. Every trade has two sides. |
| TradeId         | **integer.** The ID of the   overall trade.                  |
| OrderId         | **long integer.**   The   ID of the order causing the trade. |
| AccountId       | **integer.** The Account   ID that made the trade.           |
| SubAccountId    | **integer.** Not currently   used.                           |
| InstrumentId       | **long integer.** The ID of the instrument being traded. See **“Products and Instruments”** for the difference. See **GetInstrument** to   find information about this instrument by its ID. |
| Side               | **string.** Buy or Sell 0 Buy   1   Sell   2 Short   (reserved for future use) 3 Unknown   (error condition) |
| Quantity           | **real.** The unit   quantity of the trade.                  |
| RemainingQuantity  | **integer.** The   number of units remaining to be traded by the order after this execution.   This number is not revealed to the other party in the trade. This value is   also known as “leave size” or “leave quantity.” |
| Price              | **real.** The unit   price at which the instrument traded.   |
| Value              | **real.** The total value of the deal. The   system calculates this as: unit price X quantity executed. |
| TradeTime          | **integer.** The time at   which the trade took place, in POSIX format and UTC time   zone. See “Time–and Date-Stamp Formats”. |
| CounterParty       | **long integer.**   Shows   0.                               |
| OrderTradeRevision | **integer.** This   value increments if the trade has changed. Default is 1. For example, if the   trade busts (fails to conclude), the trade will need to be modified and a   revision number then will apply. |
| Direction          | **string.** Shows if this   trade has moved the book price up, down, or no change.   Values:   NoChange   UpTick DownTick |
| IsBlockTrade       | **Boolean.** Returns true   if the trade was a reported trade; false otherwise. |







## GetAccountTransactions



Returns a list of transactions for a specific account on an Order Management System. The owner of

the trading venue determines how long to retain order history before archiving.



**Note:**  In this call, “Depth” refers not to the depth of the order book, but to the count of trades to report.





### Request



```
    {
      “OMSId”: 1,
      “AccountId”: 1,
      “Depth”: 200
    }
```





Where:
| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| OMSId      | **integer.** The   ID of the Order Management System from which the account’s transactions will   be returned. |
| AccountId  | **integer.** The   ID of the account for which transactions will be returned. If not specified,   the call returns transactions for the default account for the logged-in user. |
| Depth      | **integer.**   The   number of transactions that will be returned, starting with the most   recent transaction. |





### Response



The response returns an array of transaction objects.



```
    [
      {
        {
          “TransactionId”: 0,
          “OMSId”: 0,
          “AccountId”: 0,
          “CR”: 0,
          “DR”: 0,
          “Counterparty”: 0,
          “TransactionType”: {
              “Options”: [
              “Fee”,
              “Trade”,
              “Other”,
              “Reverse”,
              “Hold”
            ]
          },
          “ReferenceId”: 0,
          “ReferenceType”: {
              “Options”: [
              “Trade”,
              “Deposit”,
              “Withdraw”,
              “Transfer”,
              “OrderHold”,
              “WithdrawHold”,
              “DepositHold”,
              “MarginHold”
            ]
          },
          “ProductId”: 0,
          “Balance”: 0,
          “TimeStamp”: 0,
        },
      }
    ]
```





Where:

| **String**      | **Value**                                                    |
| --------------- | ------------------------------------------------------------ |
| TransactionId   | **Integer.**   The   ID of the transaction.                  |
| OMSId           | **Integer.** The   ID of the Order Management System under which the requested transactions took   place. |
| AccountId       | **Integer.**   The   single account under which the transactions took place. |
| CR              | **real.**   Credit   entry for the account on the order book. Funds entering an account. |
| DR              | **real.**   Debit   entry for the account on the order book. Funds leaving an account. |
| Counterparty    | *long   integer.** Shows 0.                                  |
| TransactionType | **string.**   One   of:   <br />Fee — transaction is payment of a fee   <br />Trade — transaction is a trade (most   usual entry)   <br />Other   — non-trading transactions such as deposits and withdrawals <br />Reverse — a hold   has been reversed by this transaction   <br />Hold — funds are held while a   transaction closes |
| ReferenceId     | **long   integer.** The ID of the action or event that triggered this transaction. |
| ReferenceType   | **string.** The type of action or event that   triggered this transaction. One of: <br />Trade   <br />Deposit   <br />Withdraw <br />Transfer <br />OrderHold <br />WithdrawHold <br />DepositHold <br />MarginHold |
| ProductId       | **integer.** The   ID of the product on this account’s side of the transaction. For example, in   a dollars-for-BitCoin transaction, one side will have the product Dollar and   the other side will have the product BitCoin. See “Products and Instruments” for   more information about how these two items differ. Use **GetProduct** to   return information about a product based on its ID. |
| Balance         | **real.**   The   balance in the account after the transaction. |
| TimeStamp       | **long   integer.** Time at which the transaction took place, in POSIX format and   UTC   time zone. |







## GetInstrument
**No authentication required**



Retrieves the details of a specific instrument from the Order Management System of the trading venue. An instrument is a pair of exchanged products (or fractions of them) such as US dollars and ounces of gold. See “Products and Instruments” for more information about how products and instruments differ.



### Request



```
    {
      “OMSId”: 1,
      “InstrumentId”: 1
    }
```





Where:

| **String**   | **Value**                                                    |
| ------------ | ------------------------------------------------------------ |
| OMSId        | **integer.** The   ID of the Order Management System from where the instrument is traded. |
| InstrumentId | **long   integer.** The ID of the instrument.                |





### Response



```
    {
      “OMSId”: 0,
      “InstrumentId”: 0,
      “Symbol”: “”,
      “Product1”: 0,
      “Product1Symbol”: “”,
      “Product2”: 0,
      “Product2Symbol”: “”,
      “InstrumentType”: {
        “Options”: [
          “Unknown”,
          “Standard”
        ]
      },
      “VenueInstrumentId”: 0,
      “VenueId”: 0,
      “SortIndex”: 0,
      “SessionStatus”: {
        “Options”: [
          “Unknown”,
          “Running”,
          “Paused”,
          “Stopped”,
          “Starting”
        ]
      },
      “PreviousSessionStatus”: {
        “Options”: [
          “Unknown”,
          “Running”,
          “Paused”,
          “Stopped”,
          “Starting”
        ]
      },
      “SessionStatusDateTime”: “0001-01-01T05:00:00Z”,
      “SelfTradePrevention”: false,
      “QuantityIncrement”: 0,
    }
```





Where:

| **String**            | **Value**                                                    |
| --------------------- | ------------------------------------------------------------ |
| OMSId                 | **integer.** The ID of the Order   Management System on which the instrument is traded. |
| InstrumentId          | **long integer.**   The   ID of the instrument.              |
| Symbol                | **string.** Trading   symbol of the instrument.              |
| Product1              | **integer.** The first   product comprising the instrument. For example, USD in a USD/   BitCoin instrument. |
| Product1Symbol        | **string.** The symbol   for Product 1 on the trading venue. For example, USD. |
| Product2              | **integer.** The second   product comprising the instrument. For example, BitCoin in   a USD/BitCoin instrument. |
| Product2Symbol        | **string.** The symbol   for Product 1 on the trading venue. For example, BTC. |
| InstrumentType        | **string.** The   type of the instrument. All instrument types currently are *standard*, an exchange of one product for another (or unknown, an error condition), but this may expand   to new types in the future.   Unknown   Standard |
| VenueInstrumentId     | **long integer.**   If   the instrument trades on another trading venue to which the user   has access, this value is the   instrument ID on that other venue. See *VenueId*. |
| VenueId               | **integer.** The ID of the   trading venue on which the instrument trades, if not this   venue. See *VenueInstrumentId*. |
| SortIndex             | **integer.** The   numerical position in which to sort the returned list of instruments on a   visual display. Since this call returns information about a single   instrument, *SortIndex* should return   0. |
| SessionStatus         | **string.** Is the market   for this instrument currently open and operational? Returns   one of:   <br />Unknown   <br />Running <br />Paused <br />Stopped <br />Starting |
| PreviousSessionStatus | **string.** What was the   previous session status for this instrument? One of:   Unknown   <br />Running <br />Paused <br />Stopped <br />Starting |
| SessionStatusDateTime | **string.**   The   time and date at which the session status was reported, in ISO 8601   format. See “Time– and Date-Stamp   Formats”. |
| SelfTradePrevention   | **Boolean.** An   account trading with itself still incurs fees. If this instrument prevents an   account from trading the instrument with itself, the value returns *true*; otherwise defaults to *false*. |
| QuantityIncrement     | **integer.** The   number of decimal places for the smallest quantity of the instrument that can   trade (analogous to smallest lot size). For example, the smallest increment   of a US Dollar that can trade is 0.01 (one cent, or 2 decimal places).   Current maximum is 8 decimal places. The default is 0. |







## GetInstruments
**No authentication required**



Retrieves an array of instrument objects describing all instruments available on a trading venue to the user. An instrument is a pair of exchanged products (or fractions of them) such as US dollars and ounces of gold. See “Products and Instruments” for more information about how products and instruments differ.



### Request



```
    {
      “OMSId”: 1
    }
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| OMSId      | **integer.** The   ID of the Order Management System on which the instruments are available. |





### Response

The response for GetInstruments is an array of objects describing all the instruments available to the authenticated user on the Order Management System.



```
    [
      {
        {
        “OMSId”: 0,
        “InstrumentId”: 0,
        “Symbol”: “”,
        “Product1”: 0,
        “Product1Symbol”: “”,
        “Product2”: 0,
        “Product2Symbol”: “”,
        “InstrumentType”: {
          “Options”:   [
            “Unknown”,
            “Standard”
            ]
          },
        “VenueInstrumentId”: 0,
        “VenueId”: 0,
        “SortIndex”: 0,
        “SessionStatus”: {
          “Options”:  [
            “Unknown”,
            “Running”,
            “Paused”,
            “Stopped”,
            “Starting”
            ]
          },
        “PreviousSessionStatus”: {
          “Options”:   [
            “Unknown”,
            “Running”,
            “Paused”,
            “Stopped”,
            “Starting”
            ]
          },
          “SessionStatusDateTime”: “0001-01-01T05:00:00Z”,
          “SelfTradePrevention”: false,
          “QuantityIncrement”: 0,
        },
      }
    ]
```





Where:

| **String**        | **Value**                                                    |
| ----------------- | ------------------------------------------------------------ |
| OMSId             | **integer.** The ID of the Order   Management System on which the instrument is traded. |
| InstrumentId      | **long integer.**   The   ID of the instrument.              |
| Symbol            | **string.** Trading   symbol of the instrument.              |
| Product1          | **integer.** The first   product comprising the instrument. For example, USD in a USD/   BitCoin instrument. |
| Product1Symbol    | **string.** The symbol   for Product 1 on the trading venue. For example, USD. |
| Product2          | **integer.** The second   product comprising the instrument. For example, BitCoin in   a USD/BitCoin instrument. |
| Product2Symbol    | **string.** The symbol   for Product 2 on the trading venue. For example, BTC. |
| InstrumentType    | **string.** The   type of the instrument. All instrument types currently are standard, an exchange of one product   for another (or unknown, an error condition), but this may expand   to new types in the future.   Unknown   Standard |
| VenueInstrumentId | **long integer.**   If   the instrument trades on another trading venue to which the user   has access, this value is the instrument   ID on that other venue. See *VenueId*. |
| VenueId           | **integer.** The ID of the   trading venue on which the instrument trades, if not this   venue. See *VenueInstrumentId*. |
| SortIndex         | **integer.** The   numerical position in which to sort the returned list of instruments on a   visual display. |
| SessionStatus     | **string.** Is the market   for this instrument currently open and operational? Returns   one of:   Unknown   Running Paused Stopped Starting |
| PreviousSessionStatus | **string.** What was the   previous session status for this instrument? One of:   Unknown   Running Paused Stopped Starting |
| SessionStatusDateTime | **string.** The time and   date at which the session status was reported, in ISO 8601   format. See “Time– and   Date-Stamp Formats”. |
| SelfTradePrevention   | **Boolean.** An   account trading with itself still incurs fees. If this instrument prevents an   account from trading the instrument with itself, the value returns *true*; otherwise defaults to *false*. |
| QuantityIncrement     | **integer.** The   number of decimal places for the smallest quantity of the instrument that can   trade (analogous to smallest lot size). For example, the smallest increment   of a US Dollar that can trade is 0.01 (one cent, or 2 decimal places).   Current maximum is 8 decimal places. The default is 0. |


