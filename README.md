# rtc

The `rtc` package is a convenience layer for working with the rtc.io toolkit.
Consider it a boxed set of lego of the most common pieces required to build
the front-end component of a WebRTC application.

## Getting Started

TO BE COMPLETED

## rtc/detect

Provide the [rtc-core/detect](https://github.com/rtc-io/rtc-core#detect) 
functionality.

## rtc/generators

The generators package provides some utility methods for generating
constraint objects and similar constructs.  While this is primarily used
internally within the rtc module, it can be used via the
`require('rtc/generators')` statement.

### generators.config(config)

Generate a configuration object suitable for passing into an W3C 
RTCPeerConnection constructor first argument, based on our custom config.

### generators.mediaConstraints(flags, context)

Generate mediaConstraints appropriate for the context in which they are 
being called (i.e. either constructing an RTCPeerConnection object, or
on the `createOffer` or `createAnswer` calls).

## parseFlags(opts)

This is a helper function that will extract known flags from a generic 
options object.

## rtc/media

Provide the core [rtc-media](https://github.com/rtc-io/rtc-media) for
convenience.

## rtc/peerconnection

### PeerConnection prototype reference

### close()

Cleanup the peer connection.

### initiate(targetId, callback)

Initiate a connection to the specified target peer id.  Once the 
offer/accept dance has been completed, then trigger the callback.  If we
have been unable to connect for any reason the callback will contain an
error as the first argument.

### negotiate

### setChannel(channel)

Initialise the signalling channel that will be used to communicate
the actual RTCPeerConnection state to it's friend.

## PeerConnection Data Channel Helper Methods

The PeerConnection wrapper provides some methods that make working
with data channels simpler a simpler affair.

### createWriter(channelName?)

Create a new [pull-stream](https://github.com/dominictarr/pull-stream)
sink for data that should be sent to the peer connection.  Like the
`createReader` function if a suitable data channel has not be created
then calling this method will initiate that behaviour.

### _createBaseConnection()

This will create a new base RTCPeerConnection object based
on the currently configuration and media constraints.

### _setBaseConnection()

Used to update the underlying base connection.

### _handleICECandidate()

### _handleNegotiationNeeded

Trigger when the peer connection and it's remote counterpart need to 
renegotiate due to streams being added, removed, etc.

### _handleRemoteAdd()

### _handleRemoteUpdate

This method responds to updates in the remote RTCPeerConnection updating
it's local session description and sending that via the signalling channel.

### _handleRemoteIceCandidate(candidate)

This event is triggered in response to receiving a candidate from its
peer connection via the signalling channel.  Once ice candidates have been 
received and synchronized we are able to properly establish the 
communication between two peer connections.

### _handleRemoteRemove()

### _handleStateChange(evt)

This is a generate state change handler that will inspect the various states
of the peer connection and make a determination on whether the connection is
ready for use.  In the event that the connection is ready, it will trigger
a `ready` event.

## rtc/signaller

The `rtc/signaller` provides a higher level signalling implementation than
the pure [rtc-signaller](https://github.com/rtc-io/rtc-signaller) package.

The signaller included in this packge provides some convenience methods for
making connections with a peer given a typical rtc.io setup.

## Signaller prototype reference

### dial(targetId)

Connect to the specified target peer.  This method implements some helpful
connection management logic that will cater for the majority of use cases
for creating new peer connections.

### _handlePeerLeave

A peer:leave event has been broadcast through the signalling channel.  We need
to check if the peer that has left is connected to any of our connections. If
it is, then those connections should be closed.

## Signaller factory methods (for sugar)

### Signaller.create(opts)

Create a new Signaller instance

### Signaller.join(name)

Create a new signaller instance, and join the specified channel
