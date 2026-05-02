
### 1. What is REST API? ITS characteristics and role?

*rest api is a way for different systems(Frontend->backend) to communicate over HTTP using standard methods  like GET,POST,PUT,DELETE.*


Key Characteristics 
- stateless
- client server archi
- uniform interface
- cacheable

### 2. Difference between PUT and POST?

###### *POST* : Used to create a new resource
###### *PUT* : Used to update or replace  an existing resource.

###### Based on idempotency?
- IT means same request same result

- PUT = idempotent ✅
- POST = not idempotent ❌

### 3.Request Body vs Request Parameters

***Request Body***
- *Data sent inside the HTTP request* 
- *used in POST,PUT,PATCH*

***Request Parameters***
- *Data sent inside parameters*
- Part of url

### 4. What is document in mongoDB?

*A document  is a record stored in a collection*
- It is stored in BSON format
- Similar to json

### 5. Difference between Embedding vs Reference in MONGODB

- Embedding is nested documents.
- Store related data inside the same document

**While referencing stores in seperate collection and link using IDs**

### 6. What is objectId  in mongoDB?

An object id is a special data type used as the default value for the _id field in every document.

- it acts as unique identifier of each document
- automatically generated if not provided


### 7. What is Tag in HTML?

- In HTML its the basic building block used to define elements on a web page.
- Tags tell how to display content
- Tags are written inside angular brackets `<>`


### 8. What is a HYPERLINK

- A hyperlink is a clickable element that takes you to another page, file or location.
- its created using anchor tags

Types of hyperlink
- internal 
- external
- open in  new tab
