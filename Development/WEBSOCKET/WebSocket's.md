
*WebSocket provide a way to establish a persistent, full duplex communication channel over a single TCP connection between the client(Typically a web browser) and the server.*

![[_Excalidraw/Drawing 2026-04-10 23.26.32.excalidraw.md#^frame=fOb44aoFGomrCEjDIbe6K]]

This happens for every time the axios request is sent. So its very slow.

*In WebSocket a three-way handshake also happens but the connection is persistent.*

SO the connection only establishes only once. So client can send multiple messages so its full duplex.

 
**We create TCP server using WebSocket .It is used for Real Time Communication.**

![[_Excalidraw/Drawing 2026-04-10 23.26.32.excalidraw.md#^frame=tUufnvcl4pw4qQ9pM_6Vz]]

### **Use Cases for WebSocket's:**

- **Real-Time Applications**: Chat applications, live sports updates, real-time gaming, and any application requiring instant updates can benefit from WebSocket's.
- **Live Feeds**: Financial tickers, news feeds, and social media updates are examples where WebSocket's can be used to push live data to users.
- **Interactive Services**: Collaborative editing tools, live customer support chat, and interactive webinars can use WebSocket's to enhance user interaction.

### Why not use http server:-

1. Network Handshake happens for every request
2. No way to push server side events (You can use polling but not the best approach)

