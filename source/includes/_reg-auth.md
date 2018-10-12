# Registration and authentication

<aside class="info">To send a session token to re-establish an interrupted session, send:</aside>

## AddUserAPIKey

### Request

(*UserId* and *Permissions* simulated)

```json
    {
      "UserId": 10,
      "Permission": ["Trading","Withdraw","Deposit"]
    }
```

Where:

| **String** | **Value**                                                    |
| :--------- | ------------------------------------------------------------ |
| UserId   | **string.** The id of the user        |
| Password   | **string**. What permission that this particular user should be given |


### Response

A successful response returns the following (with *UserId* and *Permissions* simulated):

```json
    {
      "APIKEY":"<API_KEY>",
      "APISecret":"<API_SECRET>",
      "UserId":10,
      "Permissions":["Trading","Withdraw","Deposit"]
    }
```

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| APIKEY | **Boolean.** The API key that will be used to interact with Nauticus |
| APISecret  | **String.** The secret key  |
| UserId        | **Integer.** Returns the user ID   |
| Permissions        | **Array.** Returns you back the list of permissions this user has be given.   |


## AuthenticateUser


### Request

```json
    {
      "APIKey":"<API_KEY>",
      "Signature":"HMAC(Nonce + UserId + API_KEY,APISecret)",
      "UserId":"10",
      "Nonce":"111111"
    }
```

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| APIKEY | **String.** The API key |
| Signature  | **String.** The signiture key, which is produced by combaining, Nonce, UserId, APIKey and APISecret  |
| UserId        | **Integer.** The user's ID   |
| Nounce        | **String.** The retunred Nounce   |




### Response

```json
    {
      "User":{
        UserName,
        Email,
        EmailVerified,
        AccountId,
        OMSId,
        Use2FA
      },
      "Authenticated":true,
      "Requires2FA":false
    }
```

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| User | **String.** The user object which will include the user's info |
| User.UserName  | **String.** The username of the User  |
| User.Email        | **String.** The user's email address   |
| User.EmailVerified        | **Boolean** The user is verification status   |
| User.OMSId        | **Integer.** The OMSId   |
| User.Use2FA        | **Boolean** The user 2FA auth status  |
| Authenticated        | **Boolean.** The user authencated status   |
| Required2FA        | **Boolean** The user is required to use 2FA.  |


## RemoveUserAPIKey

### Request

```json
    {
      "UserId":10,
      "APIKey":"<API_KEY>"
    }
```

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| UserId        | **Integer.** The user's ID   |
| APIKEY | **String.** The API key |



### Response

```json
    {
      "result":true,
      "errormsg":null,
      "errorcode":0,
      "detail":null
    }
```

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| result | **Boolean.** The result was either successful or not  |
| errormsg  | **null / String.** Error will be set to `null` if no error otherwise a message will be accompanying  |
| errorcode        | **Integer.** Error code or `0` if no error   |
| detail        | **null /String.** Detail about the call if it has failed otherwise `null`   |



## WebAuthenticateUser

#### No authentication required

`WebAuthenticateUser authenticates a user (logs in a user) for the current websocket session. You must call WebAuthenticateUser in order to use the calls in this document not otherwise shown as "No authentication required."`



### Request

```json
    {
      "UserName": "UserName",
      "Password": "Password"
    }
```



Where:

| **String** | **Value**                                                    |
| :--------- | ------------------------------------------------------------ |
| UserName   | **string.** The name of the user, for example, jsmith        |
| Password   | **string**. The user password. The user logs into a specific Order Management<br/>System via Secure Socket Layer (SSL and HTTPS). |

 

### Response

Unsuccessful response:

```json
    {
      "Authenticated": false
    }
```



Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| Authenticated | **Boolean.**   The   default response is *false* for an   unsuccessful authentication. |

​	

A successful response returns the following (with *UserId* and *SessionToken* simulated):

```json
    {
      "Authenticated": true,
      "SessionToken": "7d0ccf3a-ae63-44f5-a409-2301d80228bc",
      "UserId": 1
    }
```

 

Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| Authenticated | **Boolean.** The response is true for a successful authentication. |
| SessionToken  | **string.** SessionToken uniquely identifies the session on the OMS. By returning the SessionToken in the response, the user can log in again if the session is interrupted without going through two-factor authentication. |
| UserId        | **integer.** Returns the user ID of the authenticated user   |






## Authenticate2FA

#### No authentication required



`Completes the second part of a two-factor authentication by sending the authentication token from the non-Nauticus authentication system to the Order Management System.`



###### Here is how the two-factor authentication process works:

1. Call **WebAuthenticateUser**. The response includes values for *TwoFAType* and *TwoFAToken*. For example, *TwoFAType* may return "Google," and the *TwoFAToken* then returns a Google-appropriate token (which in this case would be a QR code).
2. Enter the *TwoFAToken* into the two-factor authentication program, for example, Google

Authenticator. The authentication program returns a different token.

1. Call **Authenticate2FA** with the token you received from the two-factor authentication

program (shown as *YourCode* in the request example below).



### Request

```json
    {
      "Code": "YourCode"
    }
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| Code       | **string.**   Code   holds the token obtained from the other authentication source. |



### Response

```json
    {
      "Authenticated": true,
      "SessionToken": "YourSessionToken"
    }
```



Where:

| **String**    | **Value**                                                    |
| ------------- | ------------------------------------------------------------ |
| Authenticated | **Boolean.**   A   successful authentication returns *true*.   Unsuccessful returns *false*. |
| SessionToken  | **string.** The   *SessionToken* is valid during the   current session for connections from the same IP address. If the connection   is interrupted during the session, you   can sign back in using the *SessionToken*   instead of repeating the full two-factor authentication process. A   session lasts one hour after last-detected activity or until logout. |


```json
    {
      "SessionToken": "YourSessionToken"
    }
```




## LogOut

#### No authentication required



`Logout* ends the current websocket session.`



### Request

`There is no payload for a *Logout* request.`

```
	{ }
```



### Response

```json
    {
      "result":true,
      "errormsg":null,
      "errorcode":0,
      "detail":null
    }
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.** A   successful logout returns true; and unsuccessful logout (an error   condition) returns false. |
| errormsg   | **string.**  A   successful logout returns null; the errormsg parameter for an unsuccessful   logout returns one of the following messages:   Not   Authorized (errorcode 20) Invalid Request (errorcode 100) Operation Failed   (errorcode 101) Server Error (errorcode 102)   Resource Not Found (errorcode 104)   Not Authorized and Resource Not   Found are unlikely errors for a LogOut. |
| errorcode  | **integer.**  A   successful logout returns 0. An unsuccessful logout returns one of the   errorcodes shown in the errormsg list. |
| detail     | **string.** Message   text that the system may send. Usually null. |







## ResetPassword

  

***ResetPassword*** is a two-step process. The first step calls ***ResetPassword*** with the user’s username. The Order Management System then sends an email to the user’s registered email address. The email contains a reset link. Clicking the link sends the user to a web page where he can enter a new password.



`Note: Passwords must be longer than eight characters, with at least one case change (upper-to-lower/ lower-to-upper) and one numeral. Venue operators may impose additional rules.`



### Request

```json
	{
		"UserName": "UserName",
	}
```



Where:

| **String** | **Value**                                                  |
| ---------- | ---------------------------------------------------------- |
| UserName   | **string.**   The   name of the user, for example, jsmith. |



### Response

```json
    {
      "result": true,
      "errormsg": null,
      "errorcode": 0,
      "detail": null,
    }
```



Where:

| **String** | **Value**                                                    |
| ---------- | ------------------------------------------------------------ |
| result     | **Boolean.** Returns   *true* if the UserName is valid; *false* if not. See *"Standard Response Object and Common Error Codes"* on page 2 for an explanation of the other   string/value pairs. |

