- [[#What we're Building -]]
- [[#Backend]]
- [[#Schema]]
- BootStrapping

----

## What we're Building:-

- This is an app where we can save the important links 
- We can share the content in the brain to others.
- We can build a query to search on the all the links.

----
## Backend

##### Sign Up
- `POST /api/v1/signup`
- constraints
	1. username should be 3-10 letters
	2. password must be 8 to 20 letters should have an uppercase one lowercase one special character
- Resonse :
	- 400 is succesful 
	- 500 if fails

##### Sign In
- `POST /api/v1/signin`
- returns 
	- 200 
	`'token:'jwt_token'`
	-  403 -wrong email password
	- 500 wrong email password
	- 
#####  add new content
- `/api/v1/content`
```ts
'type':'document'|'tweet'|'youtube'
"link":"url"
"title":"title of docs/video"
"tags":["productivity","Politics"]
```

##### fetching all existing documentation
- `GET /api/v1/content`
- return all the content of the person
- so we send the jwt to fetch
##### Delete a document 
- `DELETE /api/v1/content`
- Return
	1. 200 : Delete succuess
	2. 403 trying to delete a doc you don't own
##### Create a shareable link for your second brain
- `POST /api/v1/brain/share`

```ts
{
"share":true
}
```



---
## Schema 

![[Excalidraw/Projects.md#^frame=uXRox2jDABxDLH2yjdwIe|80%]]

![[Pasted image 20260301235154.png]]
1. Users table
2. Content table
3. tags
4. link

---
## Architecture


---

## NOTES:

1. we should install express and dependencies in dev dependencies using
	`npm i -D express`
	
2. 
	- if we use import express
	- we get this error,
```
	Could not find a declaration file for module 'express'. 'd:/Typescript-project/server/node_modules/express/index.js' implicitly has an 'any' type.
```
	
	*its because there is no `.d.ts` file in express which is its declaration file*
	
	so we use  `npm i -d @types/express`
	
	- //@ts-ignore  //if we add these above any error it goes away
	
