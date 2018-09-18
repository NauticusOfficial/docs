# User Information Calls





## GetAvailablePermissionList

 

Retrieves a comma-separated array of all permissions that can be assigned to a user.

An administrator or superuser can set permissions for each user on an API-call by API-call basis, to allow for highly granular control. Common permission sets include *Trading, Deposit,* and *Withdrawal* (which allow trading, deposit of funds, and account withdrawals, respectively); or *AdminUI, UserOperator,* and *AccountOperator* (which allow control of the Order Management System, set of users, or an account). See *“Permissions”* for more information, but a complete discussion of permissions and their scope is beyond this API guide.



### Request

The request for **GetAvailablePermissionList** does not require a UserId. It returns a list of all permissions available.

```
	{ }
```



### Response

A successful response returns an alphabetically sorted list of all permissions that can be assigned by an administrator or superuser.



```
    [
      “AccountReadOnly”,
      “AddAccount”,
      “AddBalanceReconciliationInfo”,
      “AddDepositTicketAttachment”,
      “AddEditAccountBadge”,
      “AddEditExchange”,
      “AddEditExchangeInstrument”,
      “AddEditOMS”,
      “AddEditOperatorEmail”,
      “AddInstrument”,
      “AddOperator”,
      “AddOperatorOms”,
    ...
    ]
```





## GetUserConfig


**GetUserConfig** returns the list of key/value pairs set by the **SetUserConfig** call and associated with a user record. A trading venue can use *Config* strings to store custom information or compliance information with a user record.

  

**Note:** In **RegisterNewUser** (and only in **RegisterNewUser**), the key identifier of the *config* string is called
*name.*



### Request

```
	{
		”UserId”: 1,
		“UserName”: “jsmith”,
	}
```

 

Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| UserId     | **integer.** The ID of the user whose permission information will be returned. A user can only   retrieve his own permissions; an administrator can retrieve information about   the permissions of others. |
| UserName   | **string.** The login name of the user; “jsmith.”            |



### Response

A successful call to **GetUserConfig** returns an array of the current list of *Config* key/value pairs.

Because they are set during the **RegisterNewUser** and **SetUserConfig** calls, *Config* key/value pairs are unique to their trading venues.

An unsuccessful call to **GetUserConfig** returns an error code. See *“Standard Response Object and Common Error Codes”* .



```
	[
		“Street”: “Hillside Road”,
		“Office Number”: 158,
		“Mobile Phone”: “1-702-555-1212”,
		“City”: “Las Vegas”,
	]
```




## GetUserInfo

Retrieves basic information about a user from the Order
Management System. A user may only see information about himself; an
administrator (or superuser) may see, enter, or change information about other
users. See “Permissions”.

### Request

No UserId is required in the request. The system assumes the current use.

```
{ }
```


### Response

A successful response displays the settings for the user. An unsuccessful response generates an


error code. See *“Standard Response Object and Common Error Codes”*.



```
	{
		“UserId”: 1,
		“UserName”: “John Smith”,
		“Email”:“email@company.com”,
		“PasswordHash”: “”,
		“PendingEmailCode”: “”,
		“EmailVerified”: true,
		“AccountId”: 1,
		“DateTimeCreated”:”2017-10-26T17:25:58Z”,
		“AffiliateId”: 1,
		“RefererId”: 1,
		“OMSId”: 1,
		“Use2FA”: false, “Salt”: “”,
		“PendingCodeTime”: “0001-01-01T00:00:00Z”,
	}
```

 

Where:

| **String**       | **Value**                                                    |
| ---------------- | ------------------------------------------------------------ |
| UserId           | **integer.** ID number of   the user whose information is being set. |
| UserName         | **string.** Log-in name   of the user; “jsmith”.             |
| Email            | **string.** Email address   of the user; [“person@company.com”.](mailto:person@company.com) |
| PasswordHash     | **string.** Not currently   used. Returns an empty string.   |
| PendingEmailCode | **string.** Usually   contains an empty string. During the time that a new user has been sent a   registration email and before the user clicks the confirmation link, this   pair contains a GUID — a globally unique ID string.. |
| EmailVerified    | **Boolean.** Has your   organization verified this email as correct and operational?   *True*   if   yes; *false* if no. Defaults to *false*. |
| AccountId        | **integer.** The ID of the   default account with which the user is associated. |
| DateTimeCreated | **long integer.** The   date and time at which this user record was created, in ISO 8601 format. See “Time–   and Date-Stamp Formats”. |
| AffiliatedId        | **integer.** The ID of an   affiliated organization, if the user comes from an affiliated link. This is   set to 0 if the user it not associated with an affiliated organization. |
| RefererId           | **integer.** Captures the   ID of the person who referred this account member to the   trading venue, usually for marketing   purposes. Returns 0 if no referrer. |
| OMSId               | **integer.** The ID of the Order   Management System with which the user is associated. |
| Use2FA              | **Boolean.** *True* if   the user must use two-factor authentication; *false* if the user does not need to use two-factor authentication.   Defaults to *false*. |
| Salt                | **string.** Reserved for   future use. Currently returns an empty string. |
| PendingCodeTime     | **long integer.**   A   date and time in ISO 8601 format. Reserved. See “Time– and Date-Stamp Formats”. |
| ------------------- | ------------------------------------------------------------ |




## GetUserPermissions

Retrieves an array of permissions for the logged-in user. Permissions can be set only by an
administrator or superuser.

 An administrator or superuser can set permissions for each user on an API-call by API-call basis, to allow for highly granular control. Common permission sets include *Trading, Deposit, and Withdrawa*l (which allow trading, deposit of funds, and account withdrawals, respectively); or *AdminUI, UserOperator*, and *AccountOperator* (which allow control of the Order Management System, set of users, or an account). See “Permissions” for more information, but a complete discussion of permissions and their scope is beyond this API guide.



### Request



```
	{
		“UserId”: 1,
	}
```




Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| UserId     | **integer.** The ID of the user whose permission information will be returned. A user can only   retrieve his own permissions; an administrator can retrieve information about   the permissions of others. |

 

### Response


A successful response returns an array of permission strings. An unsuccessful response returns

an error code. See *“Standard Response Object and Common Error Codes”* for more information about error codes.



```
	[
		“Withdraw”, “Deposit”, “Trading”
	]	
```







## RemoveUserConfig



Retrieves an array of permissions for the logged-in user. Permissions can be set only by an

administrator or superuser.

An administrator or superuser can set permissions for each user on an API-call by API-call basis, to allow for highly granular control. Common permission sets include *Trading, Deposit,* and *Withdrawal* (which allow trading, deposit of funds, and account withdrawals, respectively); or *AdminUI, UserOperator,* and *AccountOperator* (which allow control of the Order Management System, set of users, or an account). See *“Permissions”*  for more information, but a complete discussion of permissions and their scope is beyond this API guide.



### Request



```
	{
		“UserId”: 1,
	}
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| UserId     | **integer.** The   ID of the user whose permission information will be returned. A user can only   retrieve his own permissions; an administrator can retrieve information about   the permissions of others. |

 

### Response

A successful response returns an array of permission strings. An unsuccessful response returns

an error code. See “Standard Response Object and Common Error Codes” for more information about error codes.

```
	[
		“Withdraw”,
		“Deposit”,
		“Trading”
	]
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.**   If   the call has been successfully received by the Order Management   System, result is *true*; otherwise, it is *false*. |
| errormsg   | **string.** A   successful receipt of the call returns *null*;   the *errormsg* parameter for an   unsuccessful call returns one of the following messages:   Not   Authorized (errorcode 20) Invalid Request (errorcode 100) Operation Failed   (errorcode 101) Server Error (errorcode 102)   Resource Not Found (errorcode 104) |
| errorcode  | **integer.** A   successful receipt of the call returns 0. An unsuccessful receipt of the call   returns one of the *errorcodes* shown   in the *errormsg* list. |
| detail     | **string.** Message   text that the system may send. The content of this parameter is usually null. |





## SetUserConfig



**SetUserConfig** adds an array of one or more arbitrary key/value pairs to a user record. A trading venue can use *Config* strings to store custom information or compliance information with a user’s record.



**Note:** In **RegisterNewUser** (and only in **RegisterNewUser**), the *key* identifier of the config strings is called *name*.



### Request



A *Key* is always a string, but the associated *Value* of the *Key* can be of any data type.

```
	{
		“UserId”: 1,
		“UserName”: “jsmith”,
		“Config”: [
			“Key”: “Street Name”,
			“Value”: “Hillside Road”,
			“Key”: “Suite Number”,
			“Value”: 158,
		…		
		]		
	}
```

 

Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| UserId     | **integer.** The   ID of the user to whose record you’re adding the custom key/value pairs. |
| UserName   | **string.** The   name of the user to whose record you’re adding the custom key/ value pairs. |
| Config     | array of key/value pairs.   “Key” is always a string; but the associated *Value* of *Key*   can be of any data type. |

  

### Response



A response from the server (even a successful one) indicates only that the Order Management

System has received the request; not that the key/value pairs have been stored. The **GetUserConfig** call returns the current list of *Config* key/value pairs; use it to verify that your new pairs have been successfully stored.

A successful response from the OMS returns a *true* result, null *errormsg*, and a 0 *errorcode*; an unsuccessful response returns a *false* result, a string *errormsg*, and a numeric *errorcode*. For more information see *“Message Frame”*  and  *“Standard Response Object and Common"*.



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
| errormsg   | **string.** A   successful receipt of the call returns *null*;   the *errormsg* parameter for an   unsuccessful call returns one of the following messages:   Not   Authorized (errorcode 20) Invalid Request (errorcode 100) Operation Failed   (errorcode 101) Server Error (errorcode 102)   Resource Not Found (errorcode 104) |
| errorcode  | **integer.** A   successful receipt of the call returns 0. An unsuccessful receipt of the call   returns one of the *errorcodes* shown   in the *errormsg* list. |
| detail     | **string.** Message   text that the system may send. The content of this parameter is usually null. |





SetUserInfo



Enters basic information about a user into the Order Management System. A user may only enter or change information about himself; an administrator (or superuser) may enter or change information about other users. See “Permissions”.



Request

 

```
    {
      “UserId”: 1,
      “UserName”: “John Smith”,
      “Password”: “password”,
      “Email”: “email@company.com”,
      “EmailVerified”: true,
      “AccountId”: 1,
      “Use2FA”: false,
    }
```



Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| UserId        | **integer.**   The   ID of the user; set by the system when the user registers. |
| UserName      | **string.**   User’s   name; “John Smith.”                   |
| Password      | **string.**   User’s   password.                             |
| Email         | **string.**   User’s   email address.                        |
| EmailVerified | **Boolean.**   Send   *true* if you have verified the   user’s email; send *false* if you   have   not verified the email address.   Default is *false*. |
| AccountId     | **integer.** The   ID of the default account with which the user is associated. A user may be   associated with more than one account, and more than one user may be   associated with a single account. An admin or superuser can specify   additional accounts. |
| Use2FA        | **Boolean.** Set   to *true* if this user must use   two-factor authentication; set to *false*   if this user does not need to user two-factor authentication. Default is *false*. |

 

### Response



A successful response echoes the settings in the request, and provides additional information about the user information from the database. An unsuccessful response generates an error code. See *“Standard Response Object and Common Error Codes”.*



```
    {
      “UserId”: 1,
      “UserName”: “John Smith”,
      “Email”: “email@company.com”,
      “PasswordHash”: “”,
      **“PendingEmailCode”: “”,**
      “EmailVerified”: true,
      “AccountId”: 1,
      “DateTimeCreated”:”2017-10-26T17:25:58Z”,
      “AffiliateId”: 1,
      “RefererId”: 1,
      “OMSId”: 1,
      “Use2FA”: false, 
      “Salt”: “”,
      “PendingCodeTime”: “0001-01-01T00:00:00Z”,
    }
```

 

 

Where:

| **String**       | **Value**                                                    |
| ---------------- | ------------------------------------------------------------ |
| UserId           | **integer.**   ID   number of the user whose information is being set. |
| UserName         | **string.**   Log-in   name of the user; “jsmith”.           |
| Email            | **string.**   Email   address of the user; [“person@company.com”.](mailto:person@company.com) |
| PasswordHash     | **string.**   Not   currently used. Returns an empty string. |
| PendingEmailCode | **string.** Usually   contains an empty string. Contains a GUID — a globally unique ID string —   during the time that a new user has been sent a registration email and before   the user clicks the confirmation link. |
| EmailVerified    | **Boolean.**   Has   your organization verified this email as correct and operational?   *True*   if   yes; *false* if no. Defaults to *false*. |
| AccountId        | **integer.**   The   ID of the default account with which the user is associated. |
| DateTimeCreated  | **long integer.** The   date and time at which this user record was created, in ISO 8601 format.  See *“Time– and Date-Stamp Formats”.* |
| AffiliatedId     | **integer.** The   ID of an affiliated organization, if the user comes from an affiliated link.   This is set to 0 if the user it not associated with an affiliated   organization. |
| RefererId        | **integer.**   Captures   the ID of the person who referred this account member to the   trading venue, usually for marketing   purposes. Returns 0 if no referrer. |
| OMSId            | **integer.** The   ID of the Order Management System with which the user is associated. |
| Use2FA           | **Boolean.** *True* if   the user must use two-factor authentication; *false* if the user does not need to use two-factor authentication.   Defaults to *false*. |
| Salt             | **string.**   Reserved   for future use. Currently returns an empty string. |
| PendingCodeTime  | **long   integer.** A date and time in ISO 8601 format. Reserved. See *“Time– and Date-Stamp Formats”.* |