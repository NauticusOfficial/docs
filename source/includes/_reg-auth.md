# WebAuthenticateUser

#### No authentication required

`WebAuthenticateUser authenticates a user (logs in a user) for the current websocket session. You must call WebAuthenticateUser in order to use the calls in this document not otherwise shown as “No authentication required.”`



### Request	

```
	{
		“UserName”: “UserName”,
		“Password”: “Password”
	}
```



Where:

| **String** | **Value**                                                    |
| :--------- | ------------------------------------------------------------ |
| UserName   | **string.** The name of the user, for example, jsmith        |
| Password   | **string**. The user password. The user logs into a specific Order Management<br/>System via Secure Socket Layer (SSL and HTTPS). |

 

### Response

Unsuccessful response:

```
	{
		“Authenticated”: false
	}
```



Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| Authenticated | **Boolean.**   The   default response is *false* for an   unsuccessful authentication. |

​	

A successful response returns the following (with *UserId* and *SessionToken* simulated):

```
	{
		“Authenticated”: true,
		“SessionToken”:”7d0ccf3a-ae63-44f5-a409-2301d80228bc”, “UserId”: 1
	}
```

 

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| Authenticated | **Boolean.** The response is true for a successful authentication. |
| SessionToken  | **string.** SessionToken uniquely identifies the session on the OMS. By returning the SessionToken in the response, the user can log in again if the session is interrupted without going through two-factor authentication. |
| UserId        | **integer.** Returns the user ID of the authenticated user   |

------

###### *See Also:* 

**Authenticate2FA, LogOut**

------





# Authenticate2FA

#### No authentication required



`Completes the second part of a two-factor authentication by sending the authentication token from the non-Nauticus authentication system to the Order Management System.`

```
The call returns a  !"#$%&'($)* (+'( (+" ,-"# .)//$*/ $* +'- 0""* ',(+"*($&'("12 '*1 ' ()3"*4
```



###### Here is how the two-factor authentication process works:

1. Call [**WebAuthenticateUser**](#_bookmark27). The response includes values for *TwoFAType* and *TwoFAToken*. For example, *TwoFAType* may return “Google,” and the *TwoFAToken* then returns a Google-appropriate token (which in this case would be a QR code).
2. Enter the *TwoFAToken* into the two-factor authentication program, for example, Google

Authenticator. The authentication program returns a different token.

1. Call **Authenticate2FA** with the token you received from the two-factor authentication

program (shown as *YourCode* in the request example below).



### Request

```
	{
		“Code”: “YourCode”
	}
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| Code       | **string.**   Code   holds the token obtained from the other authentication source. |



### Response

```
	{
		“Authenticated”: true, “SessionToken”: “YourSessionToken”
	}
```



Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| Authenticated | **Boolean.**   A   successful authentication returns *true*.   Unsuccessful returns *false*. |
| SessionToken  | **string.** The   *SessionToken* is valid during the   current session for connections from the same IP address. If the connection   is interrupted during the session, you   can sign back in using the *SessionToken*   instead of repeating the full two-factor authentication process. A   session lasts one hour after last-detected activity or until logout. |



```
*To send a session token to re-establish an interrupted session, send:*
```

```
	{
		“SessionToken”: “YourSessionToken”
	}
```



------

###### *See Also*

**WebAuthenticateUser, LogOut**

------





# LogOut

#### No authentication required



`Logout* ends the current websocket session.`



### Request

`There is no payload for a *Logout* request.`

```
	{ }
```



### Response

```
	{
		“result”:true,
		“errormsg”:null,
		“errorcode”:0,
		“detail”:null
	}
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.** A   successful logout returns true; and unsuccessful logout (an error   condition) returns false. |
| errormsg   | **string.**  A   successful logout returns null; the errormsg parameter for an unsuccessful   logout returns one of the following messages:   Not   Authorized (errorcode 20) Invalid Request (errorcode 100) Operation Failed   (errorcode 101) Server Error (errorcode 102)   Resource Not Found (errorcode 104)   Not Authorized and Resource Not   Found are unlikely errors for a LogOut. |
| errorcode  | **integer.**  A   successful logout returns 0. An unsuccessful logout returns one of the   errorcodes shown in the errormsg list. |
| detail     | **string.** Message   text that the system may send. Usually null. |



------

###### *See Also:*

**WebAuthenticateUser, Authenticate2FA**

------





# ResetPassword

  

***ResetPassword*** is a two-step process. The first step calls ***ResetPassword*** with the user’s username. The Order Management System then sends an email to the user’s registered email address. The email contains a reset link. Clicking the link sends the user to a web page where he can enter a new password.



------

**Note:**      Passwords must be longer than eight characters, with at least one case change (upper-to-lower/ lower-to-upper) and one numeral. Venue operators may impose additional rules.

------



### Request

```
	{
		“UserName”: “UserName”,
	}
```



Where:

| **String** | **Value**                                                  |
| ---------- | ---------------------------------------------------------- |
| UserName   | **string.**   The   name of the user, for example, jsmith. |



### Response

```
	{
		“result”: true, “errormsg”: null, “errorcode”: 0, “detail”: null,
	}
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.** Returns   *true* if the UserName is valid; *false* if not. See *“Standard Response Object and Common Error Codes”* on page 2 for an explanation of the other   string/value pairs. |



------

###### *See Also*

**RegisterNewUser**

------

