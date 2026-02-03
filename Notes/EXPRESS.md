
## Express Error handling:-

[[NODE ERROR HANDLING]] *Look here for full reference* 


```js
app.all("*",(req,res,next)=>{
	  next(.new ExpressError(404,"page not found"));
  });
```

- this will not work as in the newer version if express "* " and all is not used

```js
app.all("*", handler); // or 

app.use("*", handler); // or
 
router.get("*", handler);
```

In **newer versions of Express / router / path-to-regexp**,  
`"*"` is **no longer a valid route pattern**.

- In the newer version of express we use 

```js
  app.use((req,res)=>{
	  throw new ExpressError(404,"page not found");
  })
```

- for all path in the express

---

# Express Router:-

- Express router are a way to organize your Express application such that our primary app.js file done not become bloated.
- ***Use the `express.Router` class to create modular, mountable route handlers. A `Router` instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.***
- ` const router=express.Router()` // *creates new router object .
- All related routes will have same starting point.

```js
const express=require("express");
const router=express.Router();

//index users
router.get("/",(req,res)=>{
    res.send("get for users");
});

//show users
router.get("/:id",(req,res)=>{
    res.send("get for show users");
});

module.exports=router;
```

```js
const users=require("./routes/user.js");
const posts=require("./routes/post.js");

app.use("/users",users);
app.use("/post",posts);//for post
```

- *But if the parent route `/birds` has path parameters, it will not be accessible by default from the sub-routes. To make it accessible, you will need to pass the `mergeParams` option to the Router constructor* [reference](https://expressjs.com/en/5x/api.html#app.use).

```js
const router = express.Router({ mergeParams: true })
```

---

# Cookies:-

- ### Web Cookies
- HTTP cookies are small block of data created by a web server while a user is browsing a website and placed on the user's computer or other device by the user's web browser.
- ***We use cookies:-***
	1. Session management : Cookies we originally introduced to provide a way for users to record item they want to purchase.	
	2. Personalization : The cookies a can be used to remember information about user in order to show relevant content to that user over time.
	3. Tracking : Tracking cookies are used to track user's web browsing habit.
- #### Authentication Cookies:-
	- these are commonly used by web server to authenticate that user is logged in, and with which account they are logged in.
	-  Without the cookie, users would need to authenticate themselves by logging in on each page containing sensitive information that they wish to access.
### Sending Express Cookies:-

- `res.cookie(name,value,[options]):-
	- Sets cookie `name` to `value`. The `value` parameter may be a string or object converted to JSON.
	- The `options` parameter is an object that can have the following properties.
	
```js
app.get("/getcookies",(req,res)=>{
    res.cookie("greet","hello");
    res.send("sent you some cookies");
});
```

![[Pasted image 20260127224323.png]]
## Cookie Parser:-

- ### req.cookies:-

	- When using cookie parser ,middleware, this property is an object that contains cookies sent by the request. If the request contains no cookies, it defaults to `{}`.

- ### Cookie-parser Package
	- we `npm install cookie-parser`
	- This allows accessing cookies and parsing it.
	- we do it using ***Middleware called cookie parser.
	- Parse `Cookie` header and populate `req.cookies` with an object keyed by the cookie names.
	- This allows cookies to be parsed and used by <mark style="background: #FFB8EBA6;">req.cookies</mark>.

```js
const cookieParser=require("cookie-parser");

app.use(cookieParser());

app.get("/greet",(req,res)=>{
    let {name="anonymous"}=req.cookies;
    res.send(`hello ${name}`);
})
```

## Signed Cookies:-

- ***SEND Signed Cookies
- This allows to protect from tampering.
- When using cookie-parser middleware, this property contains signed cookies sent   by the request, unsigned and ready for use.
- Signed cookies reside in a different object to show developer intent; otherwise, a malicious attack could be placed on `req.cookie` values.
- If no signed cookies are sent, the property defaults to `{}`
- But if its tampered it will send `False`.

```js
app.use(cookieParser("secretcode"));//this can be any string

app.get("/getsignedcookie",(req,res)=>{
    res.cookie("madeIN","INDIA",{signed:true});
    res.send("signed cookie sent")
});


app.get("/verify",(req,res)=>{
    console.log(req.signedCookies);
    res.send("verified");
});
```

-> *Here secret code doesn't mean encrypted it means that the cookies has not been tampered.*


---
# State Protocol :-

- *State essentially means all the information related to a request*
- Status on server side it means whether if payment worked or not  maybe state of that request.

<mark style="background: #FFB8EBA6;">SESSION:</mark> Every time client interacts with the server its one session.

## Stateful Protocol:-
- stateful protocol require server to save the status and session information.
- Eg- ftp (File Transfer Protocol).

## Stateless Protocol:-
- stateless protocol does not require the server to retain the server information or or session
- Eg - http

![[Pasted image 20260130220111.png]]

***

# Express Sessions:-

- An attempt to make our session stateful.
- User can add item in cart without logging in we can store it in session id.
- It will save the session of user in server.
- On client side, Express sends  only session id.
- This doesn't get saved in database since its not permanent.
- we store it in temporary storage.
  
 ```js 
//this is server side
  user1
  sessionid:101
  items:{
  item: laptop,
  item:charger
  }
 ```

```js
const session=require("express-session");

app.use(session({secret:"mysupersecretstring"}));
```

## Session Data:-
- In stateless there is no data being saved but in stateful it does.

```js
app.use(
  session({
    secret: "mysupersecretstring",
    resave: false,
    saveUninitialized: true,
  }),
);
```

***RESAVE:- Forces the session to be saved back to the session store, even if the session was never modified during the request.

***SaveUninitialized:-Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified.

```js
app.get("/reqcount", (req, res) => {
    if(req.session.count){
        req.session.count++;
    }else{
        req.session.count=1;
    }
  res.send(`you sent a request ${req.session.count}times`);
});
```

*This is being saved in session storage `MEMORY STORE` and updated everytime we refresh.*

*The default server-side session storage,`MemoryStore`, is _purposely_ not designed for a production environment. It will leak memory under most conditions, does not scale past a single process, and is meant for debugging and developing.*

## Storage and Using Info:-
- This stores useful information in SESSION so we can use this on other pages.
```js
app.get("/register",(req,res)=>{
    let {name="anonymous"}=req.query;
    req.session.name=name;
    console.log(req.session.name);
    res.redirect("/hello");
});

app.get("/hello",(req,res)=>{
    res.send(`hello ${req.session.name}`);
})
```

## Connect-Flash:-
- The flash is a special area of the session used for storing message.
- Message are written to flash and cleared after being displayed to the user.
- These appear when we do something like update,add,delete like alert.
- It only flashes once,and gets removed after refresh.

**Flash messages are stored in the session. First, setup sessions as usual by enabling `cookieParser` and `session` middleware.**

```js
const flash=require("connect-flash");

app.use(flash());

app.get("/register",(req,res)=>{
    let {name="anonymous"}=req.query;
    req.session.name=name;
    console.log(req.session.name);
    req.flash("success","user registered");
    res.redirect("/hello");
});

app.get("/hello",(req,res)=>{
    res.render("page.ejs",{name:req.session.name,msg:req.flash("success")});

})
```

### Res.Local:-
- Use this property to set variables accessible in templates rendered with `res.render` . 
- he `locals` object is used by view engines to render a response.


## Adding Cookie Options.

- Settings object for the session ID cookie. The default value is `{ path: '/', httpOnly: true, secure: false, maxAge: null }`.
- ***expires: 
	- specifies date the object will expire set-cookie attribute.
	- By default no expiration is set and its considered a "non-persistent cookie".
	- It gets deleted when we exit a site.

- ***maxAge:
	- Specifies the `number` (in milliseconds) to use when calculating the `Expires` `Set-Cookie` attribute.

- ***httpOnly:
	- Specifies the `boolean` value for the `HttpOnly` `Set-Cookie` attribute. When truthy, the `HttpOnly` attribute is set, otherwise it is not. By default, the `HttpOnly` attribute is set.
	- we use this for security against cross-scripting attacks.

```js
const sessionsOptions={
   secret:"mysupersecretcode",
   resave:false,
   saveUninitialized:true,
   cookie:{
    expires:Date.now()+ 7 * 24 * 60 * 60 * 100,
    maxAge:7 * 24 * 60 * 60 * 100,
    httpOnly:true
   }
} 

app.use(session(sessionsOptions));
```

