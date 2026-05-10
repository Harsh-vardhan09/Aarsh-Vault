## Authentication:-
- Authentication is the process of verifying who someone is.
- The sign up/ Log in part of an app.

## Authorization:-
- Authorization is the process of verifying what specific application, files, and data a user has access to.

---
# Storing Password:-

- We store data of user like email, username, password in database.
- We ==never== store the password as it is.
- We store their *HASHED* form.
- Hashing function turn our string to an unreadable string form.
- When we enter same string to hashing func it will give same output and we match it in database to allow login.
 ![[Pasted image 20260202010848.png]]
  ***why we don't store password as it is:-
  1. If data leak happens users password gets in hand of hacker who can login your id.
  2. If user have same password multiple places this can lead to major loss.

## Hashing Function:-

- For every input, there is a fixed output.
- They are one-way functions, we can't get input from output.
- For a different input, there is a different output but of same length.
- Small changes in input should bring large changes in output.
- some famous hashing function are:
	 1. SHA256 : its weak because its fast 
	 2. MD5
	 3. CRC
	 4. BCrypt

- Blockchain made SHA256 algo famous. It gives 64 length hashed values. 
- We want slower hashing functions to protect against  brute force attacks.

### Avalanche Effect:-
- Its a desirable property of `Cryptography` in block ciphers and hash functions where a minor input change causes a drastic, unpredictable change in output. 
![[Pasted image 20260203152317.png]]
## Salting:-

-  Password salting is a technique to protect password stored in databases by adding a string of 32 or more characters and then hashing them.
- We append a string to password to hash form and store it.
- Hacker can't brute force it using common form.
- like adding helloworld -> add salt `%?a` to helloworld%?a.
- Hackers use reverse lookup table.

---
## Passport [](https://www.passportjs.org/):-

- It is library with with authentication in node.js .
- Passport is Express-compatible authentication middleware for node.js .
- Passport sole purpose is to authenticate requests, which it does through an extensible set of plugins knows as strategies. 
- A comprehensive set of strategies support authentication using a *username and password*, Facebook, Twitter, and [more](https://www.passportjs.org/packages/).
- **Passport-local mongoose:** Passport-Local Mongoose is a Mongoose that simplifies building username and password login with Passport.
	`npm install passport-local-mongoose`




## Passport configuration:-

- ***Passport.initialize() : A middleware that initialize passport.

- ***Passport.session(): A web application that needs that ability to identify users as they browse from page to page. This series of request and response, each associated with the same user, is known as session.

- ***Passport.use(new LocalStategy(User.authenticate())) : This uses User model.
- This means inside passport whatever local strategy we have created must be authenticated through this.
- To authenticate user we use `authenticate()` which is a static method added by default  mongoose.

```js
//we need session for user authentication so we use passport after session

app.use(passport.initialize());
app.use(passport.session());
passport.use(new LocalStrategy(User.authenticate()));
 

// use static serialize and deserialize of model for passport session support
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```
 *To store data of user in session is serialization and to unstore it is deserialization.*

```js
router.post( "/login",
passport.authenticate("local", { //this is a middleware
    failureRedirect: "/login",
    failureFlash: true,
  }),

  wrapAsync(async (req, res) => {
    // res.send("welcome to wanderLust,you are logged in")
    req.flash("success","welcome to wanderLust");
    res.redirect("/listings");
  }),
);
```

***Here authentication is being done by Passport and in case of failure its redirecting and sending a flash. Which is working as a middleware and has multiple function like
failureRedirect
failureFlash

---
### Mongoose passport Error:-
- First param to `schema.plugin()` must be a function, got "object"/"undefined"
### ✅ Meaning

Mongoose expects a **function**, but the imported plugin is not a function because the package export shape is different.

- when we `console.log(passportLocalMongoose)`
	this is output `{ default: [Function] }`
- *This tells us that the function is in default what we were sending was whole object so we use*. 

```js
const mongoose=require("mongoose");
const Schema=mongoose.Schema;
const passportLocalMongoose=require("passport-local-mongoose");  

// console.log("plugin test" , passportLocalMongoose);

const userSchema=new Schema({
    email:{
        type:String,
        required:true
    },
})
userSchema.plugin(passportLocalMongoose.default);
//here the function comes inside the dafault object we will get error for object when it comes we need fuction so we call .default 

module.exports=mongoose.model('User',userSchema);
```

