https://petal-estimate-4e9.notion.site/Databases-and-MongoDb-1017dfd107358065a996cda5ed89682e

 ![[Excalidraw/Drawing 2026-02-23 23.56.15.excalidraw.md#^frame=MgpeAJ6Iwi5MAwzJIMj4k|500]]

# Hashing Password:

- *Password hashing is a technique used to securelu store password in a way that makes them difficult to recover and misuse.*
- Instead of storing the actual password, you store a hashed version.
## Salt:-

- A popular approach to hashing passwords involves using a hashing algorithm that incorporates a salt - a random value added to password before hashing.
- This prevents attackers from using precomputed tables (rainbow tables) to crack password

## Bcrypt:-

- *Its a cryptographic hashing algo designed for securely hashing passwords.*
- Bcrypt incorporates  a salt and is designed to be computationally expensive, making brute-force attacks more difficult.
- 
![[Screenshot 2024-09-15 at 6.51.34 PM.png]]

```js
app.post("/signup", async function (req, res) {
  const email = req.body.email;
  const password = req.body.password;
  const name = req.body.name;

  

    const hashedPassword= await bcrypt.hash(password,5)
	    console.log(hashedPassword);
	    await UserModel.create({
	    email: email,
	    password: hashedPassword,
	    name: name,
	  });

  

  res.json({
    message: "You are signed up",
  });
});
```

```js
app.post("/signin", async function (req, res) {
  const email = req.body.email;
  const password = req.body.password;

  const response = await UserModel.findOne({
    email: email,
  });

  

  if(!response){
    res.status(403).json({
        message:"user doesnt exist"
    })
    return
  }

  const passwordMatch=bcrypt.compare(password,response.password)
  // here the compare is an function which matches the password automatically where Password is the text password and the res.password is hash password
  
  if (response) {
    const token = jwt.sign(
      {
        id: response._id.toString(),
      },
      JWT_SECRET,
    );

  

    res.json({
      token,
    });
  } else {
    res.status(403).json({
      message: "Incorrect creds",
    });
  }

});
```

