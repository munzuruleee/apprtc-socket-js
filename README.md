# AppRtc Socket

#### Socket using the AppRtc WebSocket server to exchange messages between *two* peers. This socket is developed to exchange lightweight signaling messages, *not* for transmitting (large) binaries.

## Features

- supports text message exchange between two peers identified by a unique key
- offers callback and promised based API

## Install

```
npm install apprtc-socket
```

## Usage

### Callbacks

```js
var socket = require('apprtc-socket')

var myId = 'foo' // replace with unique key
var peerId = 'bar' // replace with unique key

var mySocket = socket(myId, peerId)
// socket is connected and ready to use
mySocket.on('ready', function() {
  // send text message to peer
  mySocket.send('test message')
  // close socket
  mySocket.close()
})
mySocket.on('message', function(message) {
  // incoming text message
})
mySocket.on('close', function() {
  // socket is closed
})
mySocket.on('error', function(error) {
  // ooops
})

// activate the connection
mySocket.connect()
```

### Promises

```js
var socket = require('apprtc-socket')

var myId = 'foo' // replace with unique key
var peerId = 'bar' // replace with unique key

var mySocket = socket(myId, peerId)
mySocket.on('message', function(message) {
  // incoming text message
})
mySocket.on('error', function(error) {
  // ooops
})

// activate the connection
mySocket.connectP()
  // socket is connected and ready to use
  .then(function () {
    // send text message to peer
    mySocket.send('test message')
    // close socket
    return mySocket.closeP()
  })
  .then(function () {
    // socket is closed
  })
  .catch(function (error) {
    // ooops
  })
```

## API

### `mySocket = socket(myId, peerId)`

Create a new AppRtcSocket connection. Both interacting peers must be identified by a unique key.

### `mySocket.connect()`

Connect to the AppRtc WebSocket server and register a connection. The id of the AppRtc room that both peers use to exchange messages is a commutative hash calculated from both peers' unique keys. This function fires a `ready` event once the registration succeeds.

### `mySocket.connectP()`

Connect to the AppRtc WebSocket server and register a connection. Instead of firing a `ready` event, this function returns a promise that gets fulfilled once the registration succeeds.

### `mySocket.send(data)`

Send text data to the connected peer. `data` should be of type
`String`, other data types can be transformed into a `String` with `JSON.stringify`.

### `mySocket.close()`

Close a connection. Fires a `close` event once complete.

### `mySocket.closeP()`

Close a connection. Instead of firing a `close` event, this function returns a promise that gets fulfilled once the connection has closed.

## Events

### `mySocket.on('ready', function () {})`

Fired when the socket is ready to use.

### `mySocket.on('message', function (message) {})`

Received a message from the AppRtc server. `message` is always a `String`.

### `mySocket.on('close', function () {})`   

Fired when the connection has closed.   

### `mySocket.on('error', function () {})`

Fired when a fatal error occurs.     

## Examples

See examples directory.