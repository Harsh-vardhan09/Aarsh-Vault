
-  It is intermediary 
- *In express middleware are functions that come into play after server recieves a request and before response is sent to client.*
- common middle ware are:-
	1. method-override
	2. body parser
	3. express.Static 
	4. express.Urlencoded
	
### Middleware function perform :-

 1. execute any code.
 2. make changes to the request and the response object.
 3. end req-res cycle.
 4. call the next middle ware function in the stack.

---
## First Middleware

```js
app.use((req,res)=>{
    res.send("this is a middleware");
});

```

==this will work for all path as there is no path specified in it and middleware sends response itself.==


## Types of middleware:-

An Express application can use the following types of middleware:

- [Application-level middleware](https://expressjs.com/en/guide/using-middleware.html#middleware.application)
- [Router-level middleware](https://expressjs.com/en/guide/using-middleware.html#middleware.router)
- [Error-handling middleware](https://expressjs.com/en/guide/using-middleware.html#middleware.error-handling)
- [Built-in middleware](https://expressjs.com/en/guide/using-middleware.html#middleware.built-in)
- [Third-party middleware](https://expressjs.com/en/guide/using-middleware.html#middleware.third-party)

[MUST KNOW MIDDLEWARE](https://blog.bitsrc.io/5-express-middleware-libraries-every-developer-should-know-94e2728f7503)

---
## Using Next():-

- The middleware function is commonly denoted by variable next.
- If the current middleware function does not end request-response cycle. It must call next() to pass control to the next middleware.
- this next denotes what next middleware or path it will work on.

```js
app.use((req,res,next)=>{

    console.log("this is a 1st middleware");
    next();
    console.log("this is after next()");//here this will also work after the middleware.if we use return next code written below in the function   will work after it
});

app.use((req,res,next)=>{

    console.log("this is a 2nd middleware");
    next();
});
```

---

# App.Use callback

- App.use runs for all paths in a server. It creates middlewares.

*it can have an argument as 

![[Pasted image 20260117145946.png]]

==when we leave path empty it works for all as it takes ' / ' as path which all paths have.

---
## Error Handling by Express:-

- Express has default error handling.
- Express itself sends error as res in case if it comes with <mark style="background: #FFB8EBA6;">stack trace</mark>.
- <mark style="background: #FFB8EBA6;">Stack traces where the error started stacking.</mark>
- It sends status code and stack trace.

```js
const checkToken=("/api",(req,res,next)=>{
    let {token}=req.query;
    if(token==="giveaccess"){
        next();
    }
    throw new Error("acess denied")
});

```
