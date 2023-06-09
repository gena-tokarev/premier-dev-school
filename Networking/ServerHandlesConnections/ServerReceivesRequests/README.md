# Server receives requests

## Server listens for and accepts connections (Node.js)
1. When you start a Node.js server, it creates one process.
2. That process creates a listening socket to accept incoming connections.
3. Despite having only one thread, Node.js can handle many operations concurrently (not exactly in parallel) thanks to its event-driven, non-blocking I/O model, which allows it to initiate an operation and then move on to another operation without waiting for the first one to complete.
4. When a client sends a request, the server's listening socket accepts the connection and a new socket is created for this connection. This is handled by the underlying network libraries and the operating system, not directly by your Node.js code.

One clarification is that Node.js itself (with the help of the underlying system libraries and the operating system) creates a new socket for EACH incoming connection. This new socket is what's used to communicate with the client that initiated the connection, while the original listening socket continues to listen for new connections.

And while these connections are managed in a concurrent manner thanks to Node.js's event-driven architecture, the actual execution of your JavaScript code is still single-threaded -- i.e., only one operation is being executed at any given moment. However, the event loop in Node.js makes it seem like multiple operations are being executed at the same time by quickly switching between different tasks as soon as they are ready to be executed or when an I/O operation is waiting for a response.

The concurrent management of multiple sockets and connections allows a single Node.js process to handle many simultaneous connections efficiently, despite being single-threaded.


### Node.js net module

you can create and manage sockets directly in Node.js using the net module, which provides an asynchronous network API for creating TCP or IPC servers and clients. Here is a simple example of a TCP echo server and client.

**Echo Server:**

This server listens for connections and sends back any data it receives.

```javascript
const net = require('net');

const server = net.createServer((socket) => {
  socket.on('data', (data) => {
    socket.write(data);  // Echo back the incoming data.
  });
});

server.listen(8000, 'localhost', () => {
  console.log('Server is listening on localhost:8000');
});

```

**Echo Client:**

This client connects to the server, sends a message, and logs any data it receives.

```javascript
const net = require('net');

const client = net.createConnection({ port: 8000, host: 'localhost' }, () => {
  console.log('Connected to server');
  client.write('Hello, server!');
});

client.on('data', (data) => {
  console.log('Received from server:', data.toString());
});

client.on('end', () => {
  console.log('Disconnected from server');
});

```

As you can see, when you're working with sockets directly like this, you're dealing with both the listening socket (created by `server.listen()`) and the connection sockets (each individual `socket` object passed to the callback provided to `net.createServer()`).

Note that this is lower-level than what you'd typically do when building a web application with Node.js, where you'd use a higher-level framework like Express.js, which handles much of the socket management for you. But understanding sockets can certainly help you understand what's happening under the hood in those frameworks!


### A few words on NginX

Nginx utilize an event-driven architecture which, conceptually, can be thought of as similar to the event loop in JavaScript.

In an event-driven architecture, a central component (the event loop, in this context) listens for events and then delegates the handling of each event to a handler or callback function.

Similarly, Nginx uses an event-driven, non-blocking, asynchronous architecture to handle requests. This allows Nginx to handle many connections at the same time with a single worker process, making it very efficient.

When a new connection comes in, an event is generated and placed in the event queue. The event loop in Nginx constantly listens to the queue, and when an event is available, it will call the associated event handler to process it. This design allows Nginx to work in an asynchronous, non-blocking manner, as it doesn't have to wait for one request to be fully processed before moving on to the next one. Instead, it can start processing multiple requests concurrently.

This is similar to the event loop in JavaScript (like in Node.js), which also uses an event-driven, non-blocking, asynchronous model to handle operations, allowing it to work with many tasks at the same time without having to wait for one task to complete before starting another.

It's important to note, though, that while these two systems both use an event-driven model, the specifics of their implementation can differ, and the event loop in JavaScript and the event-driven model in Nginx aren't directly equivalent.
