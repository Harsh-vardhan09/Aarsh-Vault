# Your first CI pipeline

Create:

```
.github/workflows/ci.yml
```

Example:

```
name: CI

on:
  push:
  pull_request:

jobs:
  test:

    runs-on: ubuntu-latest

    steps:

      - name: Get code
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build
```

Don't worry about memorizing this.

Understand what it means.

---

# 7. Understanding the workflow

## `name`

```
name: CI
```

Just gives the workflow a name.

GitHub will show:

```
CI
```

---

## `on`

```
on:
  push:
  pull_request:
```

This means:

> Start this workflow when a push or pull request happens.

For example:

```
git push
   ↓
CI starts
```

---

# 8. Jobs

```
jobs:
  test:
```

A workflow contains one or more **jobs**.

Think:

```
Workflow
│
├── Test
├── Build
└── Deploy
```

Each job performs a particular task.

Our example has:

```
jobs:
  test:
```

So we have one job called `test`.

---

# 9. Runner

```
runs-on: ubuntu-latest
```

GitHub needs a computer to execute your commands.

GitHub provides a temporary computer called a **runner**.

We're saying:

> Give me an Ubuntu Linux machine.

Conceptually:

```
GitHub
   │
   └── creates temporary Ubuntu computer
                │
                ├── downloads project
                ├── installs dependencies
                ├── runs tests
                └── builds project
```

---

# 10. Steps

```
steps:
```

A job consists of steps.

For example:

```
steps:

  - name: Get code
    ...

  - name: Setup Node
    ...

  - name: Install dependencies
    ...

  - name: Run tests
    ...

  - name: Build
    ...
```

They execute approximately:

```
1. Get code
       ↓
2. Setup Node
       ↓
3. npm ci
       ↓
4. npm test
       ↓
5. npm run build
```

---

# 11. `uses` vs `run`

This is important.

### `run`

```
- name: Install dependencies
  run: npm ci
```

Means:

> Run this command in the terminal.

Basically:

```
npm ci
```

---

### `uses`

```
- name: Get code
  uses: actions/checkout@v4
```

Means:

> Use an existing GitHub Action.

`actions/checkout` is an action that downloads your repository into the runner.

So:

```
uses:
```

generally means:

> Use somebody's predefined action.

While:

```
run:
```

means:

> Execute my shell command.

---

# 12. `npm ci`

You'll often see:

```
npm ci
```

instead of:

```
npm install
```

For CI environments, `npm ci` is preferred because it installs dependencies based on the lockfile.

You should have:

```
package.json
package-lock.json
```

Then:

```
npm ci
```

installs exactly what's specified in the lockfile.

---

# 13. What happens when you push?

Imagine you change:

```
src/auth.js
```

Then:

```
git add .
git commit -m "Fix authentication"
git push
```

GitHub receives your code.

Then:

```
                  GitHub
                     │
                     ↓
              GitHub Actions
                     │
                     ↓
              Checkout code
                     │
                     ↓
                Setup Node
                     │
                     ↓
                  npm ci
                     │
                     ↓
                npm test
                     │
                     ↓
               npm run build
                     │
              ┌──────┴──────┐
              ↓             ↓
             ❌             ✅
           Failed          Passed
```

That's CI.