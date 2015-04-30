---
title: Wavecell System JS SDK Reference

language_tabs:

toc_footers:
  - <a href='wavecell.com'>Copyright © 2015 Wavecell</a>

includes:
  - events

search: true
---

# Introduction

> Current version

```
v1.1.0
```

Welcome to Wavecell Multichannel Communication System JavaScript SDK. The JavaScript Library will enable you to add voice, video and chat functionality to your webpages.

## About the documentation

All method description will have examples on the right side.

Parameters passed to the methods are samples only. You are provided a detailed description of the parameters in each method block.

# Authorization

> API Authorization Endpoint

```
http://api.wavecell.com/access/token
```
Wavecell JS SDK requires `access token` to authorize customer applications to the Multichannel Communication System.

To start you must have a `client id` and `client secret`. Wavecell will provide you this credentials.

Once you have your credentials, you can get an `access token` using OAUTH 2.0.

Wavecell supports two OAUTH 2.0 flows:

- Client credentials
- Authorization Code

# WMCSEngine

```javascript
var options = {
  'access_token' : 'your_access_token',
  'uid' : 'unique_user_id',
  'name' : 'user_display_name'
};

var wmcs = new WMCSEngine(options);
```

The JS SDK base class used to initialize the connection to the Multichannel System and perform voice and chat.

### Options Parameters

Parameter | Default | Description
--------- | ------- | -----------
access_token |  | Access token from the authentication server.
uid |  | Unique identifier for this user
name |  | Name to display on the call box.
debugLevel | 0 | 0 - No debug messages, 1 - First level of debug messages, 3 - Detailed debug messages

## initialize

```javascript
// Event emitted afer method `initialize` is executed
wmcs.on('initialize', function (connectionType) {});

wmcs.initialize();
```

Launches the connection between JS SDK and the Multichannel Communication System.

When connection has succeeded you should receive an `initialize` event.  
The callback will receive the connection type string as the first parameter.  

If the browser supports Web RTC, the connection type will be `"WebRTC"`.  
If the browser does not support Web RTC, the SDK will prompt you to download the RtccDriver for your specific OS platform.

Once the driver has started and connection is established, the connection type receive in the callback of `initialize` event will be `"RtccDriver"`.

## call

```javascript
// Events emitted afer method `call` is executed
wmcs.on('call.error', function (error) {});
wmcs.on('call.active', function () {});

wmcs.call('receiver_uid', 'Receiver');
```
Create and initiates a 1-1 voice call.

Once call is active will emit `call.active` event. On error will emit `call.error` event with the `error` given as first parameter of the callback.

### Parameters

Parameter | Description
--------- | -----------
uid | The unique id of the receiving user.
             | - Min size = 6 characters
             | - Max size = 90 characters
             | - Valid characters = UTF8 - unicode - Latin basic, except: & " # \ % ?
             | - Case sensitive and no whitespace characters
name | The name displayed on the call box of when call is initiated.
              | - Max size = 127 characters
              | - Not null
              | - UTF-8 Characters with exemption of characters " and '

## hangup

```javascript
// Event emitted when method `hangup` was successful
wmcs.on('call.hangup', function () {});

wmcs.hangup();
```

Hangs-up the current call

## disconnect

```javascript
// Event emitted when method `disconnect` is executed
wmcs.on('disconnected', function () {});

wmcs.disconnect();
```

Disconnects the user from the Multichannel Communication System.

## status

```javascript
wmcs.on('user.status', function (status) {
  
});

wmcs.status('receiver');

// Status object
{
  uid : 'uid', // uid of the user
  value : 0 // 0 - inactive, 1 - active
}
```

Checks for the status of the uid passed in the system.

Event `user.status` is emitted with the status of the user.

The callback will receive the status object.

## mute

```javascript
wmcs.mute();
```

Mutes your microphone. The other participant won't hear your audio stream.


## unmute

```javascript
wmcs.unmute();
```

Unmutes your microphone. The other participant can hear your audio stream.

## toggleMute

```javascript
wmcs.toggleMute();
```

Toggles mute state of the current call.

## setAccessToken

```javascript
wmcs.setAccessToken('sample_token');
````

Sets a new access token for the current session

### Parameters

Parameter | Description
--------- | -----------
access_token | Access token received from WMCS server


## setName

```javascript
wmcs.setName('receiver');
```

Sets the user's name displayed in the call box UI.

Parameter | Description
--------- | -----------
name | Value of name must respect naming rules:
     | - Max size = 127 characters
     | - UTF-8 Characters with exemption of characters " and '
     | - Case sensitive and no whitespace allowed