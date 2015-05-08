# Events

Events emitted by the library.

Event | Description
---------- | -------
authorizing 	| This event fires when the engine is authorizing to the WMCS backend.
authorized 		| This event is fired once engine has been authorized.
initialize 		| This event is fired when engine has been initialized. The callback will receive the connection type: "WebRTC" or "RtccDriver".
connected 		| This event is fired once connection to the WMCS cloud is established.
disconnected 	| This event is fired when connection to the WMCS cloud is cut-off.
error 			| This event fires once an error is encountered by the library. The callback will receive the error.
call.active 	| This event fires once the call has started.
call.hangup 	| This event fires once a call is terminated by either side of call.
call.error 		| This event fires once an error is encountered during the call. The callback will receive an error message.
user.status 	| This event fires once `status` method is called. The callback will receive a status object. See [status](#status).
message			| This event fires once a message has been received. See [sendMessage](#sendmessage).
presence.connected | This event fires once the user is connected to the presence server.
presence.disconnected | This event fires once the user is disconnected from the presence server.
presence.update | This event fires after a call to [setPresence](#setpresence) method. Also fires when a user in a roster changes his presence value.
presence.status | This event fires after a call to [getPresence](#getpresence) method.
presence.roster.updated | This event fires when a user is added to a roster via [addToRoster](#addtoroster), [removeFromRoster](#removefromroster) and [clearRoster](#clearroster) methods.
presence.roster | This event fires after a call to [getRoster](#getroster) method.
