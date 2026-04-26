
**We know that JWT token are stateless in case we want to to logout or make it more secure.**

***We would need to re login each time to overcome this limit came this method**

### ACCESS VS REFRESH TOKEN

- *Here we make two tokens*
##### *ACCESS TOKEN
This is a short lived token which is used to access everthing in the system

##### *REFRESH TOKEN
This is long term use token which is used for the token to refresh access token.

***if the refresh token is present and it matches with the internal system if not it means logged out or token `refresh token` expired***

---

### Authentication:-

- To implement authentication we have two ways
  
  - ***Session based***
  - **Token based**

- Token based is better than session based since its stateless so no storage and checking each token 
- More secure

#### Then the question how to implement it??
- I chose a mix of two things Passport which i have used for session based
- But not for token based after learning about it through youtube and documentation

I mostly implemented it but now the next big question...
### how to implement logOut functionality??

- In The session based you can directly delete session and simple logout but for state less there is no such way so the other ways

|Method|Complexity|Security|Impact|
|---|---|---|---|
|**Client-side only**|Low|Low|Token still works if stolen.|
|**Blacklisting**|Medium|High|Reintroduces state to the server.|
|**Short-lived + Refresh**|Medium|High|Best balance for performance and security.|
|**User Salt/Timestamp**|Medium|High|Effective for "logout from all devices".|


***For me the first is not good enough and second is not right way to do it.
But then i remembered how the clerk did the token based logout it was the third way***


# Core Idea 

> Use **two tokens**:

- **Access Token (short-lived)** → used in API requests
- **Refresh Token (long-lived)** → used to get new access tokens

```
Login →
  Access Token (15 min)
  Refresh Token (7 days, stored securely)

When access expires →
  React calls /refresh →
  Server verifies refresh token →
  New access token issued
```


```
import jwt from "jsonwebtoken";  
  
export const generateAccessToken = (user) => {  
return jwt.sign(  
{ id: user._id },  
process.env.ACCESS_SECRET,  
{ expiresIn: "15m" } // short-lived  
);  
};
```


```
const main_token=jwt.sign({username:user.email}, process.env.JWT_SECRET, {expiresIn:'7d'})
    const accessToken=generateAccessToken(user)


    res.cookie('refreshToken',main_token,{
      httpOnly:true, 
      secure:true,
      sameSite:'strict'
    })
    res.json({
      success:true,
      message: "Login Successfull",
      accessToken
      })
```

```
 httpOnly:true, 
 secure:true,
 sameSite:'strict'
````

this allows the client not to update it themselves