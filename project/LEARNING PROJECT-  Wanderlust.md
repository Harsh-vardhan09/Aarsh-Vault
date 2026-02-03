
[[#phase 1]]
[[#Phase 2 -]]
[[#USER Model -]]

---

- frontend : *HTML,CSS,EJS*
- backend : *Node ,express*
- Database : *MongoDB* 
- middleware

---

# phase 1

- creating Airbnb type site frontend for the listings.
- creating listing for the  website
- create new listing 
- CRUD for listing

*database set up*
*REST API set up* 

### model-listing
- title-sting
- description-string
- image- URL
- price -number
- location-string
- country-string

#### index route
- GET /listing ->all listing
- GET/listing/:id(show)->specific listing data(view)
  
```js
  toLocaleString("we pass the region we want");
  //this makes number into string with commas according to the region given 
```

### Create: New & Create route

- GET /listing/new ->form to create new route->
- POST /listings

### Update: Edit and Update route

- GET /listing/:id/edit -> edit the listing
- PUT /listing/:id

### Delete

- DELELETE /listing/:id

## styling

EJS Mate:- this gives boiler plate template where we can share the template between  the pages of similar page.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>It's <%= who %></title>
  </head>
  <body>
    <section>
      <%- body -%>
    </section>
  </body>
</html>
```

```html
<% layout('boilerplate') -%>
<h1>I am the <%= what %> template</h1>
```


---
# ADRESSING in Code Routes:-
 
`./` means:

> “Start from **where I am right now in the URL**”

So the browser does **not** care about your folder structure on disk.  
It only cares about the **URL in the address bar**.

### Example URLs
	
### Page that worked
	
	`http://localhost:3000/`
	
	Browser sees:
	
	`<link href="./css/style.css">`
	
	➡️ It requests:
	
	`http://localhost:3000/css/style.css ✅`



#### Page that broke

	`http://localhost:3000/posts`
	
	Same HTML:
	
	`<link href="./css/style.css">`
	
	➡️ Browser requests:
	
	`http://localhost:3000/posts/css/style.css ❌`
	
	That folder **does not exist**, so CSS fails silently.



## What `/css/style.css` means

	The leading `/` is the key.
	
	`<link href="/css/style.css">`
	
	This means:
	
> 	“Start from the **root of the website**, no matter where I am”
	
	So **every page** requests:
	
	`http://localhost:3000/css/style.css ✅`
	
	Root = where `express.static('public')` points.



## Visual analogy 🧠

	Think of URLs like folders:
	
### `./css/style.css`
	
> 	“Go to my current folder → then css → style.css”
	
## `/css/style.css`
	
> 	“Go to the front door of the house → css → style.css”
	
	That’s why `/css/...` is the standard for CSS, JS, images, fonts, etc.

## ==Always use absolute path if the path has been used with path.join in directory else use the normal adressing.

---
# Form validation:-

### Client validation:-

- when we enter data in the form, the browser and/or the web server will check to see that the data is in the correct format and within the constraints set by application.
	1. Client 
	2. Server - database rules are failed according to schema( error handling);

 - We want the validation as standardized using the bootstrap CSS form validation in the project.
### Success or Failure validation:-

- Use of the valid invalid to show success or failure in the page to confirm validation and giving error message if invalid.
`
     ` <div class="invalid-feedback">Invalid Price</div>`

### Error Handling for Server:-

- In case of server a person can send api request directly using postman or other with wrong credential. So we have to deal with this.

# Schema validation:-

- In case of api request like postman we can send data that is incomplete and it will work.
- For that we need schema validation.

```js
if(!req.body.listing){
      throw new ExpressError(400,"Send Valid Data");
    }
```
*this is written for all conditions and can be used for those cases.*


- but this is very long and bad way to do this.

==For that we use tool that called 

![[NODE ERROR HANDLING#JOI Schema Validation -]]



***********
# Phase 2:-

## Reviews Model:-

#### Review Data:-
- comments
- Rating(1 to 5)
- createdAt

#### Create Reviews:-
1. setting up review form

2. Submitting the form    
	`POST /listing/:id/reviews`

#### Validation for review:-
1. Client side(form)
2. Server side(JOI): JOI schema-> schema validate->middleware

#### Render Reviews:-
- Display on the show listing page
- ##### Adding Styling:-
	- show in card format

#### Deleting Review:-
- mongo $pull operator.
- Delete Button: `Listing/:id/reviews/:review id`

	`$pull`[](https://www.mongodb.com/docs/manual/reference/operator/update/pull/#mongodb-update-up.-pull "Permalink to this definition")
	- The [`$pull`](https://www.mongodb.com/docs/manual/reference/operator/update/pull/#mongodb-update-up.-pull) operator removes from an existing array all instances of a value or values that match a specified condition.

#### Deleting Listing:-
- delete Middleware for reviews

## Restructuring Express Router:-
- We use `express.Router()` to make different files for pages .
- We remove common part from routes of each request.
-  making a folder called `routes`.

### Restructuring Listing:-
- making `listing.js` .
- pushing all /listing routes in it. 

```js
const listing=require("./routes/listing.js")

app.use("/listings",listing);
```

*All paths for listing will go to this route making main app.js less cluttered.*

### Restructuring Reviews:-
- Made `revies.js` .
- Pushing all/reviews routes in it.

```js
const reviews=require("./routes/review.js");
app.use("/listings/:id/reviews",reviews);//here the /listing/:id stays in this so we cant acess it in review route file to do that we need external
```

## Sessions:-
- using sessions on this to allow a certain cookie to have a expiry date.
- And use that for login or other uses.

```js
const session=require("express-session");

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


### Using Flash:-
- We use bootstrap to alert to make a success .
- Here success is an empty array it exist even if the internal message is not sent.
### Success Partial:-
- In case we do some change sucessfully like deletion,update,addition of listing or review.
- `req.flash("success","Listing Deleted!");` *This in each of the success with different message*

 ```js
 app.use((req,res,next)=>{
  res.locals.success=req.flash("success");
  res.locals.error=req.flash("error");
  next();
}) 
 ```

```html
<% if(success && success.length){ %>
    <div class="alert alert-success" role="alert">
        <%= s:-uccess %>
    </div>
<% }%>
```
### Failure Partial:-
- For case when we try to access any id which does not exist we show this.

```js
 router.get(
  "/:id",
  wrapAsync(async (req, res) => {
    let { id } = req.params;
    const listing = await Listing.findById(id).populate("reviews");
    if(!listing){
      req.flash("error","Listing you requested does not exist");
      return res.redirect("/listings");
    }
    res.render("listing/show.ejs", { listing });
  }),
);
```

*we need to use return in `res.redirect` since because if we don't node will get both redirect to and render request below and crash due to double request.*

```html
<% if(error && error.length){ %>
    <div class="alert alert-danger alert-dismissible fade show col-6 offset-3 mt-3" role="alert">
        <%= error %>
        <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
    </div>
<% }%>
```

---
# USER Model:-

-> **username, email, password
- You're free to define your user how you like.
- Passport local Mongoose will add a username, hash and salt field to store username and hashed password and salt value.
## Configuring Strategy:-

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


## Signup User:-

- `GET /signup` -> signup form
- `POST /signup` -> to register new user

### Login User:-
- `GET /login` -> form to login
- `POST /login` -> to authenticate user if exist using `passport.authenticate()`

- *`passport.authenticate()` is middleware which will authenticate the request. By default, when authentication succeeds, the `req.user` property is set to the authenticated user, a login session is established, and the next function in the stack is called.*
