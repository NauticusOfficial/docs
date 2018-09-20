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

