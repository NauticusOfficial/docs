# User Information Calls





## GetAvailablePermissionList

 

Retrieves a comma-separated array of all permissions that can be assigned to a user.

An administrator or superuser can set permissions for each user on an API-call by API-call basis, to allow for highly granular control. Common permission sets include *Trading, Deposit,* and *Withdrawal* (which allow trading, deposit of funds, and account withdrawals, respectively); or *AdminUI, UserOperator,* and *AccountOperator* (which allow control of the Order Management System, set of users, or an account). See [“Permissions” on page 4 ](#_bookmark7)for more information, but a complete discussion of permissions and their scope is beyond this API guide.



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

An unsuccessful call to **GetUserConfig** returns an error code. See “Standard Response Object and Common Error Codes” on page 2.



```
	[
		“Street”: “Hillside Road”,
		“Office Number”: 158,
		“Mobile Phone”: “1-702-555-1212”,
		“City”: “Las Vegas”,
	]
```

 