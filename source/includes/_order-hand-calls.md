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
