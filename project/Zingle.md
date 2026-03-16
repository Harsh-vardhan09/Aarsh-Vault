- ***This is a social media app that allows functionalities like***
	- creating post 
	- deleting 
	- posting stories 
	- following 
	- sending message


---
## 🛠️ Tech Stack

### Client side
- React JS (VITE)
- Tailwind CSS
- Clerk
- moment
- React-hot-toast
### Server side
- Node JS
- express JS
- MongoDB
- CLOUDINARY
- CORS



---
## 📂 Project Structure

#### *Client*
```bash
Zingle/
│
├── client/        # Frontend (React + Vite + tailwind)
│   ├── src/
│   │   ├── assets/       # Images, icons, Constatants
|	|   |    └── assets.js
|   |   |            
│   │   ├── components/   
|   |   |    ├── Loading.jsx
|	|   |    ├── MenuItem.jsx
|   |   |    ├── SideBar.jsx
|   |   |    ├── StoriesBar.jsx
|   |   |    ├── StoryModal.jsx
|   |   |    ├── StoryViewer.jsx
|   |   |    ├── PostCard.jsx
|   |   |    ├──
|   |   |    └── MenuItems.jsx
|   |   |     
│   │   ├── pages/       
|   |   |   ├── index.js
|   |   |   ├── ChatBox.jsx
|   |   |   ├── Connections.jsx
|   |   |   ├── Discover.jsx
|   |   |   ├── Feed.jsx
|   |   |   ├── Layout.jsx
|   |   |   ├── Login.jsx
|   |   |   ├── Messages.jsx
|   |   |   ├── Profile.jsx
|   |   |   └── CreatePost.jsx
|   |   |
│   │   ├── constants/    
|   |   |   └──index.js # Static values
|   |   |   
│   │   ├── App.jsx
|   |   ├── .env      # env variable for clerck
│   │   └── main.jsx
│
```
#### *Server*
```bash
|
├── server/
|   |
|   ├── mongodb
|   |    |
|   |    ├── models
|   |    |   ├──   # dummy data
|   |    |   └──   #  mongoose Schema
|   |    └──    # mongoDb connect
|   |  
|   ├── routes
|   |    ├── 
|   |    └── 
|   |
|   ├── index.js   # main server
|   |
│   └── package.json
│
├──.gitignore
└── README.md

```

---
## Notes:-

#### Clerk :
-  Its an most comprehensive user authentication and user management platform
- It provides ready to use component like register signup login
#### moment:
- Its an npm package that lets us show time in the format from now
	` moment(story.createdAt).fromNow()`

#### React-hot-toast:
- Add beautiful notifications to your React app with [react-hot-toast](https://github.com/timolins/react-hot-toast).
- `npm i react-hot-toast`
- put `<Toaster/>` in app.jsx
- then we can use toast with promise
```jsx
onClick={()=>toast.promise(handleCreateStory(),{
            loading:'Saving...',
            success:<p>story Added</p>,
            error:e=><p>{e.message}</p>
        })}
```

---

### Important:-

***Client***
- *Here the `media.type` is the MIME type it can be anything image video null. `?` allows if it null it doesn't crash.*
- *If the mime type starts with image like `image/png` , `image/jpeg` it gives true.*
```jsx
(media?.type.startswith("image") ? (
              <img
                src={previewUrl}
                alt=""
                className="object-contain max-h-full"
              />
            ) : (
              <video src={previewUrl} className="object-contain max-h-full" />
            ))
```

- we use regex to transform the input to the html format we want
	`post.content.replace(/(#\w+)/g,'<span class="text-indigo-600">$1</span>')`
- We use `dangerouslySetHTML` to use it. Its called dangerous because it can cause XSS(Cross scripting attack).

- In JavaScript, the parameters of `.map()` **must be in the correct order**.
- So the **first parameter is the element (your `message`)**, and the **second is the index**.
```jsx
messages.map((message, index) => (
  <Link key={index} className='flex items-start gap-2 py-2 hover:bg-slate-100'>
    <img src={message.from_user_id.profile_picture} alt="" className='w-8 h-8 rounded-full'/>
  </Link>
))
```
- Now the variables are **swapped**:
	- `index` = actually the **message object**
	- `message` = actually the **number index**
*so this gives error since the messages have (1,2,3,...)*
*Here the index is assigned by JS itself.*


---
#### Author
*Aarsh-HV*


