There are various libraries that let you create a ws server (similar to how `express` lets you create an HTTP server)

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