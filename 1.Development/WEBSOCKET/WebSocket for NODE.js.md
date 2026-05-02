There are various libraries that let you create a WS server (similar to how `express` lets you create an HTTP server)

1. [https://www.npmjs.com/package/websocket](https://www.npmjs.com/package/websocket)
2. [https://github.com/websockets/ws](https://github.com/websockets/ws)
3. [https://socket.io/](https://socket.io/)

We’ll be using the `ws` library today.

Problems with [socket.io](http://socket.io/) - Even though socket.io is great (it gives you constructs like `rooms` to make the API much cleaner), it’s harder to support multiple platforms in it (Android, IOS, Rust) There are implementations in most platforms but not very up to date [https://socket.io/blog/native-socket-io-and-android/](https://socket.io/blog/native-socket-io-and-android/) [https://github.com/1c3t3a/rust-socketio](https://github.com/1c3t3a/rust-socketio)


---

# Create a Project

- Initialize an empty Node.js project

```jsx
npm init -y
```

- Add tsconfig to it

```jsx
npm install typescript
npx tsc --init
```

- Update tsconfig

```jsx
"rootDir": "./src",
"outDir": "./dist",
```

- Install ws
```tsx
npm i ws @types/ws
```

### Approaches to start WebSocket server:-

***We use the WS library to make the server node  has inbuilt WebSocket but not to create its server like how we need to use express to create http server

### Code using http library

```jsx
import WebSocket, { WebSocketServer } from 'ws';
import http from 'http';

const server = http.createServer(function(request: any, response: any) {
    console.log((new Date()) + ' Received request for ' + request.url);
    response.end("hi there");
});

const wss = new WebSocketServer({ server });

wss.on('connection', function connection(ws) {
  ws.on('error', console.error);

  ws.on('message', function message(data, isBinary) {
    wss.clients.forEach(function each(client) {
      if (client.readyState === WebSocket.OPEN) {
        client.send(data, { binary: isBinary });
      }
    });
  });
  
    ws.send('Hello! Message From Server!!'); }); 
server.listen(8080, function() {      
       console.log((new Date()) + ' Server is listening on port 8080'); 
});
```
### Code using express

```jsx
npm install express @types/express
```

```jsx
import express from 'express'
import { WebSocketServer } from 'ws'

const app = express()
const httpServer = app.listen(8080)

const wss = new WebSocketServer({ server: httpServer });

wss.on('connection', function connection(ws) {
  ws.on('error', console.error);

  ws.on('message', function message(data, isBinary) {
    wss.clients.forEach(function each(client) {
      if (client.readyState === WebSocket.OPEN) {
        client.send(data, { binary: isBinary });
      }
    });
  });

  ws.send('Hello! Message From Server!!');
});
```

**Here we are doing 
`const wss = new WebSocketServer({ server: httpServer });` 
because:-**

**1. WebSockets start as HTTP requests**  
A WebSocket connection doesn’t magically appear—it begins as a normal HTTP request (called a handshake) and then upgrades to a persistent connection. Socket.IO needs access to the HTTP server to intercept and upgrade that request.

**2. Single server, multiple responsibilities**  
By passing `server` into Socket.IO:
- Express handles regular routes (`/api`, `/login`, etc.)
- Socket.IO handles real-time connections (`connect`, `message`, etc.)
Both run on the same port and server.

**3. Sharing the same port (clean architecture)**  
If you didn’t do this, you’d end up needing:
- One server for HTTP (Express)
- Another for WebSockets
That’s messy and unnecessary. This approach keeps everything unified.

**4. Access to low-level events**  
Socket.IO sometimes needs direct access to the underlying server (for upgrades, headers, etc.), which Express alone doesn’t expose.


### Code without HTTP Servers

```jsx
import WebSocket, { WebSocketServer } from 'ws';

const wss = new WebSocketServer({ port: 8080 });

wss.on('connection', function connection(ws) {
  ws.on('error', console.error);

  ws.on('message', function message(data, isBinary) {
    wss.clients.forEach(function each(client) {
      if (client.readyState === WebSocket.OPEN) {
        client.send(data, { binary: isBinary });
      }
    });
  });
});
```

---


# Scaling ws servers   18/04/2026

![[Drawing 2026-04-10 23.26.32.excalidraw#^frame=61OgFJVQgY9Wrso09YHFF|100%]]

*In the real world, you’d want more than one websocket servers (Especially as your website gets more traffic)

- The way to scale websocket servers usually happens by creating a `ws fleet`

- There is usually a central layer behind it that `orchestrates` messages

- ws servers are kept `stateless`

***So we form connection of each of the websocket with each other

![[Screenshot 2024-04-06 at 6.06.53 PM.png]]

