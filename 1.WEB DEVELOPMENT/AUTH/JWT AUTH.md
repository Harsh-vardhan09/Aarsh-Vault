*by harkirat
### What is authentication?
- The process of letting users `sign in`/`sign out` of your website.
- making sure your route are protected and users can only get back their own data and not from different user.

#### What we have
1. auth basics
2. JWT ( JSON Web Token)
3. Authorization header
4. creating own auth middleware
5. local storage

### Auth workflow:

![[Drawing 2026-02-23 23.56.15.excalidraw#^frame=c_xIdIfX-pQ96pampNeNU|500]]
*these token gives us the persistent session*

- The users comes to your website
- the users sends the req to sign in with there username and password
- the user gets back token
- In every subsequent req, the user sends the token to identify it self to the backend

### Creating an express app:-
- creating an empty folder
- `npm init -y` to install a `package.json`
- `npm i express` for the express package.
- create get, post, put, Delete, Patch path.
- Use `express.json()` as middleware

```bash
npm init -y
npm i express
//in index.js
app.use(express.json())
```

### Express. json() 

- This lets you parse any post body 
- return a middleware that only parses JSON and only look at request where content type header matches the type option.


## Tokens VS JWT:

- There is a problem with using `stateful` token.

#### Stateful:
 By stateful here we mean that we need to store these tokens in a variable right now

#### Problem:-
*The problem is that we need to send req to database every time the user want to hit an `Authenticated endpoint`*

![[Drawing 2026-02-23 23.56.15.excalidraw#^frame=tBsNwecDxJmqWVHlzIUVm|500]]


## JWT's:-

*JWTs or JSON web tokens are a compact and self-contained way to represent information between two parties*
*They are commonly used for authentication and information exchange in web application.
***JWT are stateless** JWT contains all information needed to authenticate a req, so the server doesn't need to store session data.

- All the data are stored in the token itself
- Its encoded not encrypted


![[Drawing 2026-02-23 23.56.15.excalidraw#^frame=Z4QaKyW2s9dIm6rYg4Ym8|600]]

```js
npm i jsonwebtoken

const jwt=require("jsonwebtoken");
const JWT_SECRET="helloworld";


const token = jwt.sign({
        username:username
    },JWT_SECRET);//convert username over to a jwt using the secret
    
```
***It converts the username to the token using jwt_secret which we give it
We dont need to store it in db it since its stateless 


---

## 
*JSON web tokens are like bank chequebooks
They are issued by backend when you sign in.
Anyone can create something similar but backend will reject it
If you lose the original JWT, then it is a problem*

![[Drawing 2026-02-23 23.56.15.excalidraw#^frame=5HYDJsTaMmKW8rOI7ecck]]

## Creating a JWT

- generating JWT `jwt.sign(value,secret)`
- decoding `jwt.decode`
- verifying `jwt.verify(token,secret)`


*Any one can decode our JWT token and get the value.
But to verify they need JWT secret since token is created using token


