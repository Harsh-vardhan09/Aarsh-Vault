# MIME
- **MIME stand for Multipurpose Internet Mail Extension**
- This tells browser what kind of file this is that is being sent.
- When a browser request a file from a server, the server responds with the content type ==header==. *That header is MIME type*.
### Why MIME type matter:-
- The browser needs to know
	- Should I render this?
	- Should I download this?
	- Should I execute this?    
	- How should I interpret it?

### Does every req has MIME type:-
- No , only those which sends a **BODY** has mime type.
- IF the req has no body like `GET /index.html
- No content type needed.
## Types of MIME TYPE:-
| File Type     | MIME Type                |
| ------------- | ------------------------ |
| HTML          | `text/html`              |
| CSS           | `text/css`               |
| JavaScript    | `application/javascript` |
| JSON          | `application/json`       |
| PNG Image     | `image/png`              |
| JPG Image     | `image/jpeg`             |
| ICO (favicon) | `image/x-icon`           |
| SVG           | `image/svg+xml`          |
### URL ENCODED:-
- URL encoded is a way to send form data as a string like 
	` username=John&password=1234`
- If special character exist they are also encoded.
- why is it called `x-www-form-urlencoded`
- because it was first made for the ***HTML FORMS***.

```js
POST /login
Content-Type: application/x-www-form-urlencoded

username=John&password=1234
```
## JSON:-

```js
POST /login
Content-Type: application/json

{"username":"John","password":"1234"}

```

---
