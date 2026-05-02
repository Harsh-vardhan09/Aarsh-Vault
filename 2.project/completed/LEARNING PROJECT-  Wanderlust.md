
[[#phase 1 Structure]]
[[#Phase 2 User And Review]]
[[#USER Model -]]
[[#Phase -3 Deployment]]
[[#Deployment(End) -]]

---

- frontend : *HTML,CSS,EJS*
- backend : *Node ,express*
- Database : *MongoDB* 
- middleware

---

# phase 1 : Structure

- creating Airbnb type site frontend for the listings.
- creating listing for the  website
- create new listing 
- CRUD for listing

*database set up*
*REST API set up* 

### model-listing
- title-string
- description-string
- image- URL
- price -number
- location-string
- country-string

#### index route
- `GET /listing` ->all listing
- `GET/listing/:id(show)`->specific listing data(view)
  
```js
  toLocaleString("we pass the region we want");
  //this makes number into string with commas according to the region given 
```

### Create: New & Create route

- `GET /listing/new` ->form to create new route->
- `POST /listings`

### Update: Edit and Update route

- `GET /listing/:id/edit` -> edit the listing
- `PUT /listing/:id`

### Delete

- `DELELETE /listing/:id`

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
# Phase 2: User And Review

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
//we need session for user authentication so we use passport after session.

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

## Connecting Login Route:-
- How to check user is logged in
- Connecting so that only authenticated user can add listings and review.
- `req.isAuthticated()` //passport Method
## Logout User:-
- `GET /Logout`

## Add Styling:-
- Passport save user data in `req.user` if user is undefined its not logged in.
- if there is object user is logged in.
- if not logged in undefined-> signup, login
- if logged in object-> log out in req.user

```html
<% if(!currUser){
          <a class="nav-link" href="/signup">Sign Up</a>
         <a class="nav-link" href="/Login">Log In</a>
        <% } %>
        <% if(currUser){
          <a class="nav-link" href="/logout">Log Out</a>
        <% } %>
```

*Here the currUser is using `req.user` in `res.locals` in app.js*


## Login after signup:-
- Passport login method automatically establishes a login session .
- we can invoke login to automatically login a user.
```js
req.login(user, function(err) { 
if (err) {
 return next(err); 
 }
return res.redirect('/users/' + req.user.username); });
```

When the login operation completes, `user` will be assigned to `req.user`.

*Note: `passport.authenticate()` middleware invokes `req.login()` automatically. This function is primarily used when users sign up, during which `req.login()` can be invoked to automatically log in the newly registered user.*


## Post-login Page:-
- The moment user logins
- After login-> redirects to original path

```js
console.log(req.path,"..",req.originalUrl);
```
- *This will print the path and original url of the path we were trying to access*
- *req object stores alot of data about all these things*
- - ***but passport deletes sessions the moment it logins.

```js
module.exports.saveRedirectUrl=(req,res,next)=>{
    if(req.session.redirectUrl){   
		res.locals.redirectUrl=req.session.redirectUrl;
    }
    next();
}
```


## Listing Owner:-

- Authorization for listing owner
- So only allow create, delete, edit for owner

## Starting with authorization:-

- Hide Delete and edit button if its not the user who created the listing.
- To do this we will check
	- listing owner
	- user who want to edit/delete
```js
if(currUser && currUser._id.equals(listing.owner._id)){
	//putting the code we want to hide here
}
```
### Setting authorization:-
##### For listings:-
- we use authorization for the listing
```js
module.exports.isOwner = async (req, res, next) => {

  let {id}=req.params;
  let listing = await Listing.findById(id);
  if (!listing.owner._id.equals(res.locals.currUser._id)) {
    req.flash("error", "you are not the owner");
    return res.redirect(`/listings/${id}`);
  }
  next();
  
};
```

#### For Reviews:-
- Review ->author who created it
- only that author has access to delete it.


---
# Phase -3 : Deployment

## MVC:-
- Model, View, Controller is design pattern.
- Its way of writing code.
- We divide the project into Model, Views, Controllers.
- Model stores the Database component.
- View stores what we render.
- Controller stores the backend core functionality.

## Implement design pattern for
###  Listings:-
- extending listings in the controller folder.

```js
module.exports.index=async (req, res) => {
    const allListing = await Listing.find({});    res.render("listing/index.ejs", { allListing });
}
```

### Reviews & Users:-
- Using this in controller framework.

### Router. route:-
- Using this to reformat the path with same output to make it cleaner for use.
- Like putting all the route with same `/signup` or `/login` with get, post, put, delete.

```js
router
  .route("/signup")
  
 .get(userController.renderSignUpForm)
.post(wrapAsync(userController.signup));

  
router
  .route("/login")
  
.get(userController.renderLoginForm)
.post(
    saveRedirectUrl,
    passport.authenticate("local", {
      failureRedirect: "/login",
      failureFlash: true,
    }),   wrapAsync(userController.login),

  );
```

### Re-Style Ratings:-
- We use stars to show the value of the rating of the listings.
- For that we use a GitHub library called  [Starability](https://github.com/LunarLogic/starability)
- To present the input we use this inside the review.
```html
<p 
class="starability-result card-text" 
data-rating="<%= reviews.rating %>"
>
</p>
```

## Image Upload:-

- Right now we can't upload image:-
	1. as our database can't save files.
	2. We cant send files through backend.
- To make our form capable of sending files.
- we will save the images on cloud not on database.
- We will use the free version of some third party service. 
- This will give a URL to access the image.
- Then we will save the image link in the DB.
### Manipulating form:-
- We can't send the file as form sends URL encoded data.
- To send files we add a new type called `enctype="multipart/form-data`
- We add the type to file in the form.

### Multer:-
- Multer is a node.js middleware for handling `multipart/form-data`, which is primarily used for uploading files. It is written on top of busboy for maximum efficiency.

- **NOTE**: Multer will not process any form which is not multipart (`multipart/form-data`).

```js
const multer  = require('multer')
const upload = multer({ dest: 'uploads/' })

app.post('/profile', upload.single('avatar'), function (req, res, next) {
  // req.file is the `avatar` file
  // req.body will hold the text fields, if there were any
})
```

## Cloud setup:-
- cloudinary & .env file
- We use this to save the files using its credentials
- we create `.env` to store environment variable which we don't share on github
- In .env we save in key value pair.

***NOTE: To access env file we use a npm package called dotenv is a zero-dependency module that loads environment variables from a `.env` file into `process.env`.


```js
if(process.env.NODE_env!="production"){
  require('dotenv').config();
}
```
*We don't want to use `dotenv` in production/deployment as we will use different method then.*

### Store Files:-
- Multer store cloudinary
- `npm i cloudinary multer-storage-cloudinary`
- here these are two packages called cloudinary and multer-storage-cloudinary
- To do this we need some credentials and some logic.
```js
const cloudinary=require('cloudinary').v2;
const {CloudinaryStorage}=require('multer-storage-cloudinary');

cloudinary.config({
    cloud_name:process.env.CLOUD_NAME,
api_key:process.env.CLOUD_API_KEY,api_secret:process.env.CLOUD_API_SECRET
});

const storage = new CloudinaryStorage({
  cloudinary: cloudinary,
  params: {
    folder: 'wanderlust_DEV',
    AllowedFormat:["png","jpg","jpeg"], // supports promises as well
  },

});

  
module.exports={
    cloudinary,
    storage,
};
```

### Save the link in mongo:-
- We update the schema and change where we get the data for url to save the data from.
- We save the url that comes from file when we save to DB which sends to cloudinary.

```js
module.exports.createListing=async (req, res) => {

  let url=req.file.path;
  let filename=req.file.filename;
  // console.log(url,"  ",filename);
    const newListing = new Listing(req.body.listing);
    newListing.owner = req.user._id;
    newListing.image={url,filename};
    await newListing.save();
    req.flash("success", "New Listing Created");
    res.redirect("/");
}
```


## Edit Listing Image:-

- We need to change edit form if we want to edit image which exists.
- So we change the input type to the file and do the same as the new route.
- Here we find the listing first and update which gets data by `req.body`.
- To change the image we put the value of image to new URL and new filename.

```js
module.exports.updateListing=async (req, res) => {

 let { id } = req.params;

 let listing=await Listing.findByIdAndUpdate(id, { ...req.body.listing });
    
    if(typeof req.file!=="undefined"){
      let url=req.file.path;
      let filename=req.file.filename;
      listing.image={url,filename};
      await listing.save();
    }

    req.flash("success", "Listing Updated!");
    res.redirect(`/listings/${id}`);

}
```
*Here we check if the value of the `req.file` is undefined using `typeof` to only when it comes as false we update the file.*

### Image preview before edit:-

- We can do this by giving making the image preview.
- But if we do this it will upload the original pic which may be of high quality but we don't need it.
```html
<div class="mb-3">
          Original Listing Image <br>
          <img src="<%=listing.image.url %>" alt="prevImage" style="height:30%;width:30%;border-radius: 10px;" />
        </div>
```

- ***So we can use cloudinary image transformation to make it change.
- We do this using the url change.

```js
let originalImage=listing.image.url;
originalImage.replace("/upload","/upload/h_300,2_250");
```

## Getting Started with maps:-

- We will put the maps of the place where it exist.
- We can use any API.
- We will use `MapBox` API free tier
#### Using MapBox:-
- *****Mapbox GL JS is a client-side JavaScript library for building web maps and web applications with Mapbox's modern mapping technology. You can use Mapbox GL JS to display Mapbox maps in a web browser or client, add user interactivity, and customize the map experience in your application.

### Geocoding:-
- Its the process of converting address(like street address) into geographic coordinates (like latitude and longitude)
- which you can use to place markers on a map, or position the map.
- There is geocoding api of mapbox.
- GET `https://api.mapbox.com/search/geocode/v6/forward?q={search_text}`
- Or we can use SDK(software development kit) to use from git.

```js
const mbxGeocoding = require("@mapbox/mapbox-sdk/services/geocoding");
const mapToken = process.env.MAP_token;
const geocodingClient = mbxGeocoding({ accessToken: mapToken }); 

 let response=await  geocodingClient
    .forwardGeocode({
      query: req.body.listing.location,
      limit: 1,
    })
    .send()
    console.log(response.body.features[0].geometry);
    return res.send("done")
```

#### Storing Coordinates:-
- We store the coordinates using geoJSON.
- [GeoJSON](http://geojson.org/) is a format for storing geographic points and polygons. 
- MongoDB has excellent support for geospatial queries on GeoJSON objects. Let's take a look at how you can use Mongoose to store and query.
- This is standary format for geographic data.

```js
{
  "type" : "Point",
  "coordinates" : [
    -122.5,
    37.7
  ]
}
```

#### Map Marker:-
-  we use it to mark it 

#### Marker Popup:-
- It popup when we go to the marker.


## Home Page UI:-
- doing UI changes on front page adding icons and filter
### Adding Filters:-
- Adding category to the model of listings so where they can be classified.
- We can use enum for those categories and give categories where it will filters according to it.
### Adding Tax Switch:-
- Adding switch to using js to allow it to make show or hide taxes.
- Adding search bar in nav to allow search function.

---
# Deployment(End):-


- we can use platform for deployment like:- 
	- render
	- netlify
	- vercel
	- hiroku
	- cyclic ,etc