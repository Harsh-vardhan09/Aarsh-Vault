 [DOCS](https://projects.100xdevs.com/tracks/monorepo/monorepo-1#c2acea94b2bf4e6da5f1ce46142ede03 "Do you need to know them very well as a full stack engineer")
 
As the name suggests, a single repository (on github lets say) that holds all your frontend, backend, devops code.

Few repos that use monorepos are -

1. [https://github.com/code100x/daily-code](https://github.com/code100x/daily-code)

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F237a5ba6-3f60-4475-a190-025184fbf100%2FScreenshot_2024-03-16_at_2.09.31_AM.png?table=block&id=70d81aad-784f-4ab2-adfe-530157b5ea9f&cache=v2)

1. [https://github.com/calcom/cal.com](https://github.com/calcom/cal.com)

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F7ec98fc5-117e-402b-bf97-537cbd3b44ee%2FScreenshot_2024-03-16_at_2.17.24_AM.png?table=block&id=0e82a885-b8da-438e-95c8-7c211f7fcfb9&cache=v2)

#### Do you need to know them very well as a full stack engineer

Not exactly. Most of the times they are setup in the project already by the `dev tools` guy and you just need to follow the right practises

Good to know how to set one up from scratch though

---
# Why Monorepos?

#### Why not Simple folders?

Why cant I just store services (backend, frontend etc) in various top level folders?

You can, and you should if your

1. Services are highly decoupled (dont share any code)

2. Services don’t depend on each other.

For eg - A codebase which has a Golang service and a JS service

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F457b1c41-8f8c-47c7-881a-814ae54898b2%2FScreenshot_2024-03-16_at_2.54.08_AM.png?table=block&id=46bc3f63-54f9-4749-87ff-42722c34de81&cache=v2)

#### Why monorepos?

1. **Shared Code Reuse**

2. **Enhanced Collaboration**

3. **Optimized Builds and CI/CD**: Tools like TurboRepo offer smart caching and task execution strategies that can significantly reduce build and testing times.

4. **Centralized Tooling and Configuration**: Managing build tools, linters, formatters, and other configurations is simpler in a monorepo because you can have a single set of tools for the entire project.

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F778d65fc-118f-48f3-afc6-0c60af28ffb0%2FScreenshot_2024-03-16_at_2.55.59_AM.png?table=block&id=22ab6b78-b5f8-4852-b887-aec1778e8bbf&cache=v2)

---

# Common monorepo framework in Node.js

1. Lerna - [https://lerna.js.org/](https://lerna.js.org/)

2. nx - [https://github.com/nrwl/nx](https://github.com/nrwl/nx)

3. Turborepo - [https://turbo.build/](https://turbo.build/) — Not exactly a monorepo framework

4. Yarn/npm workspaces - [https://classic.yarnpkg.com/lang/en/docs/workspaces/](https://classic.yarnpkg.com/lang/en/docs/workspaces/)

We’ll be going through turborepo since it’s the most relevant one today and provides more things (like build optimisations) that others don’t

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F07d9c1dc-c198-4bd3-9d1d-e825272fd4a5%2FScreenshot_2024-03-16_at_2.41.18_AM.png?table=block&id=cef56b67-f226-4e4a-8412-e71ee85c3e12&cache=v2)

