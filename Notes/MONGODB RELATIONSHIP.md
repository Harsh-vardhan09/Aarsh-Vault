
[[#One to many(Approach 1) -]]
[[#One to many(Approach 2) -]]
[[#One to many(Approach 3) -]]
[[#Populate(Mongoose) -]]
[[#Mongoose Middlewares -]]
[[#6 Rules of Thumb for MongoDB Schema Design -]]

 - __In mongo relationship there can be can be a few type the one to many relationship.__
	 1. __one to few__
	 2. __one to many__
	 3. __one to squillion__

---
## One to many(Approach 1):-
### One to few:-
- stores the child element in the parent.
- Like addresses saved in the amazon or any ecommerce.
- There can only be a few addresses per user like less than 1000.
- We understand this by *Will we need the information individually* without the parent data.
- If we don't we don't create a seperate collection.

```js
const mongoose=require("mongoose");
const {Schema}=mongoose;

main().then(()=>console.log("connection succesful")).catch(err=>console.log(err));

async function main() {
    await mongoose.connect('mongodb://127.0.0.1:27017/relationDemo')

}

  

const userSchema=new Schema({

    username:String,
    addresses:[
        {
            _id:false,
            location:String,
            city:String
        },
    ],
});

const User=mongoose.model("user",userSchema);

const addUser=async()=>{

    let user1=new User({
    
        username:"sherlockholmes",
        addresses:[{
            location:"22 baker streew",
            city:"london"
        }]
    });
    
    user1.addresses.push({location:"p32 wallstreet",city:"london"});

    let res=await user1.save();
    console.log(res);
};

addUser();
```
*since id is false it will not give id to the address if we don't it will give id to each address like below saved.*


```js
{
  username: 'sherlockholmes',
  addresses: [
    {
      location: '22 baker streew',
      city: 'london',
      _id: new ObjectId('697272bce409aa9430415d1a')    
    },
    {
      location: 'p32 wallstreet',
      city: 'london',
      _id: new ObjectId('697272bce409aa9430415d1b')    
    }
  ],
  _id: new ObjectId('697272bce409aa9430415d19'),       
  __v: 0
}
```

## One to many(Approach 2):-

- ### one to thousand

- In case of thousands of data points to form connections.
- *Store reference of the child document inside parent.*
- Orders placed by customer on a shop or site.

```js
//mongoose schema connection

const orderSchema = new Schema({
  item: String,
  price: Number,
});


const customerSchema=new Schema({
    name:String,
    orders:[{
        type:Schema.Types.ObjectId,
        ref:'Order'
    }]
});

/*
if we want to only add the object id as ref we type Schema.Types.ObjectId
this adds only the id ref since populate uses to save storage
To add whole data we need to write [schemaName] in types.
*/

  
const Order = mongoose.model("order", orderSchema);
const Customer = mongoose.model("customer", customerSchema);

const addCustomer=async()=>{
    let cust1=new Customer({
        name:"rahul",
    });

    let order1=await Order.findOne({item:"chips"});
    let order2=await Order.findOne({item:"chocolate"});
    cust1.orders.push(order1);
    cust1.orders.push(order2);
  
    let res=await cust1.save();
    console.log(res);
}

addCustomer();

  

const addOrders = async () => {
 let res= await Order.insertMany([
    { item: "samosa", price: 12 },
    { item: "chips", price: 10 },
    { item: "chocolate", price: 40 },
  ]);

  console.log(res);
};

addOrders();
```

**This is what it saves in db.**

```js
{
  name: 'rahul',
  orders: [
    new ObjectId('69727d1deadd203f00000ccd'),
    new ObjectId('69727d1deadd203f00000cce')
  ],
  _id: new ObjectId('69727e72534bd9a098384d90'),
  __v: 0
}
```


## One to many(Approach 3):-

- ### One to Squillion:-

- Store reference to the parent inside the child.
- *Like on Instagram each post have reference to the user saved inside it . since each user can create alot of post.*

```js
//mongoose schema connection
  
const userSchema = new Schema({
  username: String,
  email:String,
});

  
const postSchema=new Schema({

    content:String,
    likes:Number,
    user:[{
	    type:Schema.Types.ObjectId,
        ref:'User'
    }]
});

  
const User=mongoose.model("User",userSchema);
const Post=mongoose.model("Post",postSchema);


const addData=async()=>{  

    let user=await User.find({username:"rajatshardul69"});

    let user1=new User({
        username:'rajatshardul69',
        email:"rajat@gamil.com"
    });

    let post1=new Post({
        content:"hello",
        likes:700,
    });

    let post2=new Post({
        content:"bye bye",
        likes:79
    });

  

    post2.user=user;//we are using this to connect the post to the user which is rajatshardul here it will give that a objectId of that user;

    // await user1.save();
    await post2.save();
}
  

addData();

  

const getData=async()=>{
    let res=await Post.findOne({}).populate('user');
    console.log(res);
}

getData();
```

---
# Populate(Mongoose):-

- **Population is the process of automatically replacing the specified paths in the document with documents from other Collection(s).**
- Populated path are no longer set to their original ==\_Id== , their value is replaced with mongoose document returned from database as result.
- You can call `Populated()` function to check whether a field is populated.

```js
const getData=async()=>{
    let res=await Post.findOne({}).populate('user');
    console.log(res);
}

getData();
```


---

# Mongoose Middlewares:-

__[Mongoose middleware documentation](https://mongoosejs.com/docs/middleware.html#pre)__

- *Middleware (also called pre and post _hooks_) are functions which are passed control during execution of asynchronous functions.*
- __Mongoose has 4 types of middleware:__ 
	1. document middleware 
	2. model middleware 
	3. aggregate middleware
	4. query middleware.

##### we use query middleware to write the query we want to run for these cases. 

```js
customerSchema.pre("findOneAndDelete",async()=>{
  console.log("pre middleware");
});
```

- _Mongoose has two type of middleware Calls_ :-
	1. PRE-run before the query is executed
	2. post-run after the query is executed.

## PRE

- Pre middleware functions are executed one after another.
- *this will work before the MongoDB function we make the pre for .In this case `findOneAndDelete` .

```js
customerSchema.pre("findOneAndDelete",async()=>{
  console.log("pre middleware");
});
```
#### [Documentation](https://mongoosejs.com/docs/middleware.html#pre)


## POST

- post middleware are executed _after_ the hooked method and all of its `pre` middleware have completed.
- *this will work after the MongoDB function we make the pre for .In this case `findOneAndDelete` .

```js
customerSchema.post("findOneAndDelete",async(customer)=>{
  if(customer.orders.length){
    let res=await Order.deleteMany({_id:{$in:customer.orders} });
    console.log(res);
  }
});
```
#### [Documentation](https://mongoosejs.com/docs/middleware.html#post)


## Handling Deletion:-

- Deletion of an two connected collections.
- user and post,*Either user is deleted and so are all the posts* or *user is deleted but post exist like reddit.*
- We need to do it so when we delete user its post also gets deleted like in Instagram.
-  __We do this using mongoose middleware to do this.__

- __HERE we are calling `findByIdAndDelete` and this call ` findOneAndDelete` as middleware. Thats why these middleware for  ` findOneAndDelete` are working for these.__
```js
// Mongoose middleware
customerSchema.post("findOneAndDelete",Aynnc(customer)=>{
	if(customer.orders.length){
		let res=await order.deleteMany({_id:{$in:customer.orders} });
		console.log(res);
	}
});

//deletion function
const delCust=async()=>{
  let data=await Customer.findByIdAndDelete('6973a48593466c6cafacfa74');
  console.log(data);
}

delCust();
```



---
# 6 Rules of Thumb for MongoDB Schema Design:-

## [Documentation for the rules](https://www.mongodb.com/company/blog/mongodb/6-rules-of-thumb-for-mongodb-schema-design)

__Here are some “rules of thumb” to guide you through these innumerable (but not infinite) choices:

1. **One:** Favor embedding unless there is a compelling reason not to.
    
2. **Two:** Needing to access an object on its own is a compelling reason not to embed it.
    
3. **Three:** Arrays should not grow without bound. If there are more than a couple of hundred documents on the “many” side, don’t embed them; if there are more than a few thousand documents on the “many” side, don’t use an array of ObjectID references. High-cardinality arrays are a compelling reason not to embed.
    
4. **Four:** Don’t be afraid of application-level joins: If you index correctly and use the projection specifier, then application-level joins are barely more expensive than server-side joins in a relational database.
    
5. **Five:** Consider the read-to-write ratio with denormalization. A field that will mostly be read and only seldom updated is a good candidate for denormalization. If you denormalize a field that is updated frequently then the extra work of finding and updating all the instances of redundant data is likely to overwhelm the savings that you get from denormalization.
    
6. **Six:** As always with MongoDB, how you model your data depends entirely on your particular application’s data access patterns. You want to structure your data to match the ways that your application queries and updates it.