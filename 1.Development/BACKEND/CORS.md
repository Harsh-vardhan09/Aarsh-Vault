- ***Cross Origin Resource Sharing*
- Its a mechanism which uses a http header which tells an web app that it can share resources with another web app.
- Both should have different origin.

```
localhost:8080-> requesting -> localhost:5173
```

![[CORS-Draw#^frame=OfuXRGt6rLGFoFX1x5g6W|800]]
*CORS relies on a mechanism by which browsers make a "preflight" request to the server hosting the cross-origin resource, in order to check that the server will permit the actual request.*

*In the preflight,  the browser sends the headers that indicate the http method and header that will be used in the actual request*

![[Pasted image 20260222212404.png]]