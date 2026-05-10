- *This is an image generation AI project which uses OpenAI for generating images and rendering them for the user based on their prompt.
- *This uses MERN stack for the development and uses OpenAI API to generate image*

---

## 🎯 Purpose:-

- Learn API fetching and storing the image.
- Practice MERN Stack
- Learning Cloud and its functionalities.

---

## 🛠️ Tech Stack

### Client side
- React js(vite)
- Tailwind css
- File saver
### Server side
- Node js
- express js
- MongoDB
- Cloudinary
- cors

---

## 📂 Project Structure

```
Imaginaryx/
│
├──Vscode
|   ├──setting.json
|
├── client/        # Frontend (React + Vite)
│   ├── src/
│   │   ├── assets/       # Images, icons
│   │   ├── components/   
|   |   |    ├── Cards.jsx
|   |   |    └── FormField.jsx
|   |   |     
│   │   ├── pages/       
|   |   |   ├── Home.jsx
|   |   |   └── CreatePost.jsx
|   |   |
│   │   ├── utils/        # using functions
|   |   |   └── Index.js
|   |   |
│   │   ├── constants/    
|   |   |   └──index.js # Static values
|   |   |   
│   │   ├── App.jsx
│   │   └── main.jsx
│
├── server/
|   |
|   ├── mongodb
|   |    |
|   |    ├── models
|   |    |   ├── init.js  # dummy data
|   |    |   └── post.js  #  mongoose Schema
|   |    └── connect.js   # mongoDb connect
|   |  
|   ├── routes
|   |    ├── dalleRoutes
|   |    └── postRoutes
|   |
|   ├── index.js   # main server
|   |
│   └── package.json
│
├──.GITIGNORE
└── README.md
```

---
## ⚙️ Installation & Setup

1. ***Clone the repo:-
```bash
git clone <Repolink>
//open Imaginaryx
```

2. ***Setup Client:-
```
cd client
npm install 
npm run dev
```

3. ***Setup server:-
```
cd server
npm i
nodemon index
```

---

### Learnings:-

- ##### FileSaver: 
	- This is solution to save files on the client-side, and is perfect for web apps that generates files on the client.
	- This allows us to download images on with their links to our device

- ##### CORS:
	- Cross Origin Resources Sharing is a http header based mechanism that allows the browser to accept the request from other origin and load resources from it

---
## Challenges faced:-

- #### Tailwind css:-
	- If tailwind css doesn't intellisense doesn't work 
	- check the output terminal if its showing can't find the ts.config it means we are in parent directory.
	- To fix that we can
	
```json
.vscode/settings.json
//create folder and put this code in it
{
  "tailwindCSS.experimental.configFile": "client/tailwind.config.js"
}

```

```json
//put this in package.json
{
  "workspaces": ["client"]
}

```

---
### Author : 
- ***Aarsh-HV
