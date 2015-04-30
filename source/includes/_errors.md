# Errors

```javascript
// Error object received by the callback
{
	type : 'api.token_expired',
	details : 'Your access token has expired.'
}
```

The `error` event callback will receive an error object so you act properly on it.


Error Code | Meaning
---------- | -------
api.token_expired | The access token provided has expired.
api.bad_request | The access token provided was invalid or the user_id was missing.
api.request_error | An error occurred in the WMCS backend.

If you receive error type `api.token_expired`, you should set a new access token for the Engine using `setAccessToken` [method](#setaccesstoken).