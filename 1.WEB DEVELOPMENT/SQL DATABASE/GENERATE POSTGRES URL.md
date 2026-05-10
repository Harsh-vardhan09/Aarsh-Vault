## 🔗 Standard PostgreSQL URL format

For PostgreSQL, the URL looks like:

```
postgresql://<username>:<password>@<host>:<port>/<database>
```

---

## ✅ For your case (based on your setup)

You’re using:

- user: `postgres`
- host: `localhost`
- port: `5432` (default)
- database: `postgres` (default unless you created one)

👉 So your local URL is:

```
postgresql://postgres:YOUR_PASSWORD@localhost:5432/postgres
```

---

## 🔥 Example

If password = `1234`:

```
postgresql://postgres:1234@localhost:5432/postgres
```

---

## 🧪 Test it in `psql`

Equivalent command:

```
psql -U postgres -h localhost -p 5432 -d postgres
```

---

## 📦 Using in your project

### Node.js (e.g. with Prisma / Sequelize / pg)

Put in `.env`:

```
DATABASE_URL=postgresql://postgres:1234@localhost:5432/postgres
```

---

## 🛠️ If you created your own DB

If you made a DB like:

```
CREATE DATABASE myapp;
```

Then URL becomes:

```
postgresql://postgres:1234@localhost:5432/myapp
```

---

## ⚠️ Common mistakes

- ❌ Using wrong user (`HARSH` instead of `postgres`)
- ❌ Forgetting password
- ❌ Wrong DB name
- ❌ PostgreSQL service not running

---