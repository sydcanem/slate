---
title: Wavecell System JS SDK Reference

language_tabs:

toc_footers:
  - <a href='wavecell.com'>Copyright © 2015 Wavecell</a>

includes:
  - events
  - errors
  - archives

search: true
---

# Introduction

> Current version

```
v1.1.0
```

Welcome to Wavecell Multichannel Communication System JavaScript SDK. The JavaScript Library will enable you to add voice, video and chat functionality to your webpages.

## Installation

> JavaScript URL

```
http://customer.api.wavecell.com/dist/<version>/engine.js
```

Add a script tag in your page with `src` pointing to the script url. Replace `<version>`
with the current version of the library.

You can check out the sample client application in [here](https://github.com/wavecellsg/voice-call-sample).

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

You can find an example authentication server [here](https://github.com/wavecellsg/sample-authentication-server).

<aside class="warning">You should never expose your client secret publicly.</aside>

# WMCSEngine

```javascript
var options = {
  'access_token' : 'your_access_token',
  'uid' : 'unique_user_id',
  'name' : 'user_display_name'
};

var wmcs = new WMCSEngine(options);
```

The JS SDK base class is used to initialize the connection to Wavecell's Multichannel Communication System and provides methods to initiate voice calls and send messages.

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

Status object will have properties:

Property | Description
---------|------------
uid   | Id of the user checked
value | Value either 0 - inactive, 1 - active.

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

## sendMessage

```javascript
wmcs.sendMessage(userId, message);

wmcs.on('message', function (messageObj) {

});
```

Sends a message to a user identified by userId in the method. The method accepts parameters:

Parameter | Description
---------|------------
userId  | User id of recepient
message | Message string

Triggers a `message` event with the message object passed in the callback.
Callback receives object with properties:

Property | Description
---------|------------
uid        | User id of the sender
message_id | Message id
message    | Message sent

## acknowledgeMessage

```javascript
wmcs.acknowledgeMessage(messageId, uid);

wmcs.on('message.ack', function (ackObj) {

});
```

Acknowledges a message via messageId and userId. The method accepts parameters:

Parameter | Description
---------|------------
messageId  | Message id integer between 0 - 4294967296
uid        | User id of the sender


Triggers a `message.ack` event when a message is successfully acknowledged.
Callback receives object with properties:

Property | Description
---------|------------
uid        | User id of the sender
message_id | Message id

## setPresence

```javascript
wmcs.setPresence(presenceInteger);

wmcs.on('presence.update', function (presenceObjArray) {
  presenceObjArray.forEach(function (p) {
    console.log(p[i].uid +" 's status is:" + p[i].presence);
  });
});
```

Sets the presence status of the current user. Parameter passed to `setPresence` must be an integer value between 0 and 255. The application is responsible for determining the meaning of these values except for 0 which means offline.

Triggers a `presence.update` event passing an array of presence object to the callback.
Each presence object have properties:

Property | Description
---------|------------
uid        | User id of the user which presence was changed/updated.
presence   | Presence integer value

## getPresence

```javascript
// Sample call check user_1 and user_2 presence
wmcs.getPresence(['user_1', 'user_2']);

wmcs.on('presence.status', function (presenceObjArray) {
  presenceObjArray.forEach(function (p) {
    console.log(p[i].uid +" 's status is:" + p[i].presence);
  });
});
```

Gets the presence of users not in current user roster. This method accepts an array of user ids.

Triggers `presence.status` event passing an array of presence object to the callback. Each presence object have properties:

Property | Description
---------|------------
uid        | User id of the user which presence was changed/updated.
presence   | Presence integer value

## addToRoster

```javascript
// Adding one user to the roster
wmcs.addToRoster(['user_1']);

wmcs.on('presence.roster.updated', function (data) {

});
```

Adds a user to your current presence roster. Initial user presence value is set to 0.

Triggers `presence.roster.updated` event passing an object containing the number of user added and the total count of users currently in the roster.

## removeFromRoster

```javascript
// Removing one user from the roster
wmcs.removeFromRoster(['user_1']);

wmcs.on('presence.roster.updated', function (data) {

});
```

Removes a user from the current roster.

Triggers `presence.roster.updated` event passing an object containing the number of user removed and the total count of users currently in the roster.

## getRoster

```javascript
wmcs.getRoster();

wmcs.on('presence.roster', function (presenceObjArray) {
  presenceObjArray.forEach(function (p) {
    console.log(p[i].uid +" 's status is:" + p[i].presence);
  });
})
```

Gets a presence values of all users currently in the roster.

Triggers `presence.status` event passing an array of presence object to the callback. Each presence object have properties:

Property | Description
---------|------------
uid        | User id of the user which presence was changed/updated.
presence   | Presence integer value

## clearRoster

```javascript
wmcs.clearRoster();
```

Empties the current user's roster.