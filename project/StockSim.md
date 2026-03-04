*This is an Stock Trading which is based on mern stack*
*It uses CRUD operation to get stock remove them and show portfolio for this*


---
## 🛠️ Tech Stack
#### Root
- Concurrently
- git ignore
- Readme.md
#### Client 
- react with vite
- tailwind
- Lucide-reacts
#### Server
- express
- nodemon
- mongoose/mongodb
- Passport/zod

---
## ⚙️ Installation & Setup

1. ***Clone the repo:-
```bash
git clone <Repolink>
//open StockSim
```

2. ***Setup Client:-
```bash
cd client
npm install 
```

3. ***Setup server:-
```bash
cd server
npm i
```

4. Root server:-
```bash
//enter root from either by doing cd..
npm i
npm run start
```


---
## 📂Project structure

### Client

```
client/
│
├── public/
│
├── src/
│   │
│   ├── components/
│   │     Navbar.js
│   │     StockCard.js
│   │     TradeForm.js
│   │
│   ├── pages/
│   │     Home.js
│   │     Market.js
│   │     Portfolio.js
│   │     Login.js
│   │     Register.js
│   │
│   ├── services/
│   │     api.js
│   │
│   ├── context/
│   │     AuthContext.js
│   │
│   ├── App.js
│   └── index.js
```

### Server

```
server/
│
├── config/
│     db.js
│
├── models/
│     User.js
│     Stock.js
│     Transaction.js
│     Portfolio.js
│
├── controllers/
│     authController.js
│     stockController.js
│     tradeController.js
│     portfolioController.js
│
├── routes/
│     authRoutes.js
│     stockRoutes.js
│     tradeRoutes.js
│     portfolioRoutes.js
│
├── middleware/
│     authMiddleware.js
│
├── utils/
│     updatePortfolio.js
│
├── .env
├── server.js
└── package.json
```

---
