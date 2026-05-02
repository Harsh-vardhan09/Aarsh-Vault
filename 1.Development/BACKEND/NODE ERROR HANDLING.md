
1. [[#Normal Error handling -]]
2. [[#Async Error Handling -]]
3. [[#Try-Catch Error Handling -]]
4. [[#wrapAsync Error handling -]]
5. [[#JOI Schema Validation -]]

---

**Error Handling** refers to how Express catches and processes errors that occur both **synchronously and asynchronously**. Express comes with a default error handler so you don’t need to write your own to get started.

HTTP response status codes indicate whether a specific [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) request has been successfully completed. Responses are grouped in five classes:

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#informational_responses) (`100` – `199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#successful_responses) (`200` – `299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#redirection_messages) (`300` – `399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#client_error_responses) (`400` – `499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#server_error_responses) (`500` – `599`)


```js
const checkToken=("/api",(req,res,next)=>{
    let {token}=req.query;
    if(token==="giveaccess"){
        next();
    }
    throw new Error("acess denied")
});
```

---
# Normal Error handling :-

- Create an oops style ExpressError.js

```js
class ExpressError extends Error{
    constructor(status,message){
        super();
        this.status=status;
        this.message=message;
    }
}

module.exports=ExpressError;
```

using it in main page to handle error

```js

app.get("/admin",(req,res)=>{
    throw new ExpressError(403,"ACCESS TO ADMIN IS FORBIDDEN")
})

  

app.use((err,req,res,next)=>{
    let {status=500,message="some error"}=err;
    res.status(status).send(message);
});
```


---
# Async Error Handling :-

- Async error handling is different from normal error handling .
- In async it doesn't call next next automatically ==but this was in version 4 in version 5 express it automatically calls next in case of Rejected promise.

```js

app.get("/chats/:id",async(req,res,next)=>{

    let {id}=req.params;
    let chat=await Chat.findById(id);
     if(!chat){
      console.log("not found");
      throw new ExpressError(404,"chat not found");
     }
     
    res.render("edit.ejs",{chat});
  }catch(err){
    next(err);
  }
});

```

*This will work in the express version 5 but will crash in version 4 as it doesn't need to explicitly call next. ==but we still handles error using Try-catch or asyncWrap.*

## Example:-

## version 4:-

![[Pasted image 20260117190403.png]]

<mark style="background: #FFB8EBA6;">In express 4 the nodemon crashes when we throw error in the async function while when we use next in it it gives correct output .
</mark>

<mark style="background: #FFB8EBA6;">As in version 4 this</mark> 

`throw` in `async` ⇒ **rejected Promise**
What happens:

1. Route returns a **rejected Promise**
2. Express 4 **ignores it**
3. Node sees an **unhandled promise rejection**
4. Node terminates the process 💥
![[Pasted image 20260117190935.png]]
 **This gives correct output like in video according to the version 4**

![[Pasted image 20260117191041.png]]

## version 5:-

<mark style="background: #FFB8EBA6;">In version 5 it works even with throw in the code</mark>

### Express 5 **AUTOMATICALLY catches async errors**

Express 5 internally does this:

`Promise.resolve(routeHandler(req, res, next))   .catch(next);`

So your code:

```js
throw new ExpressError(404, "Chat not found");
```
becomes:

`next(err);`

Which means:

- Error goes to error middleware
- Server does NOT crash
- Proper HTTP 404 response sent

➡️ **This is the biggest Express 5 upgrade**

---

# Try-Catch Error Handling:-

 **There are many type of error in the databases like:-
 1. Validation error 
 2. Cast error 

- when we send the mongoose validation with "required:True" as empty it gives validation error.
- **we use TRY AND CATCH block to handle these errors.
- Since **TRY AND CATCH** handles all types of error instead of using *if with next and throw to handles all kind of error.*

```js
app.get("/chats/:id",async(req,res,next)=>{

  try{
    let {id}=req.params;
    let chat=await Chat.findById(id);
     if(!chat){
      console.log("not found");
      next(new ExpressError(404,"chat not found"));
     }
     
    res.render("edit.ejs",{chat});
    
  }catch(err){
    next(err);
  }
});

```


---
# wrapAsync Error handling:-

- *TRY-CATCH can be really bulky using it on every function.*
- *we will create a function called **wrapAsync**  which returns a Function . And has a Function as an argument.*
- It returns a function which handles the error.
- Passing function inside the asyncWrap.

```js
function asyncWrap(fn){

  return function(req,res,next){
    fn(req,res,next).catch(err=>next(err));
  };
};
```

**Using it to wrap other function to make the code look more appealing. Instead of using the try and catch in each block.**

```js
app.get("/chats",asyncWrap(async (req,res)=>{

    let chats= await Chat.find();
    res.render("index.ejs",{chats})
}));
```

---
# Mongoose Error:-

- **We can have error with each different name and we can extract those names to do some special Response for those errors.**


```js
//handling special error

  

const handleValidationError=(err)=>{

  console.log("this is a validation error please follow rules");
  console.dir(err);
  return err;
};  
  

//name of error for each differnt error we waant differnt output

app.use((err,req,res,next)=>{

   console.log(err.name);
   if(err.name==="ValidationError"){
      err=handleValidationError(err);
   }
   next(err);
});

```

---
# Schema validation:-

- In case of api request like postman we can send data that is incomplete and it will work.
- For that we need schema validation.

```js
if(!req.body.listing){
      throw new ExpressError(400,"Send Valid Data");
      
      //this needs to be before the save since if it tries saving it will not give same error.
      
    }
```
*this is written for all conditions and can be used for those cases. *

- but this is very long and bad way to do this.

<mark style="background: #ADCCFFA6;">For that we use tool that called</mark> 

# JOI Schema Validation:-


- #### The most powerful schema description language and data validator for JavaScript.
- **JOI lets you describe your data using a simple, intuitive, and readable language.
- We use this to define a schema for server side validation.

*we use JOI schema validation to create validation for server side so we don't have to write alot of conditions.*

### *There is two step process for the schema validation:-*

1. First create a schema using the rules.

```js
const schema = Joi.object({
    a: Joi.string()
});
```

2. Second , Step is to use the schema to validate the *ERROR*.

```js
const { error, value } = schema.validate({ a: 'a string' });
```

[*JOI DOCUMENTATION*](https://joi.dev/api/?v=17.13.3)  *read for further idea*


