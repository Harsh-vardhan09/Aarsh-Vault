[NOTES](https://projects.100xdevs.com/tracks/rendering/render-3#35af1a6c67364731ab2d9a11789e3b6d "Why?")

Ref [https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating)

If a page uses **Static Generation**, the page HTML is generated at **build time**. That means in production, the page HTML is generated when you run `next build`. This HTML will then be reused on each request. It can be cached by a CDN.

### Why?

If you use static site generation, you can defer the `expensive` operation of rendering a page to the `build time` so it only happens once.

#### How?

Let’s say you have an endpoint that gives you all the `global` todos of an app.

By `global todos` we mean that they are the same for all users, and hence this page can be statically generated.

[https://sum-server.100xdevs.com/todos](https://sum-server.100xdevs.com/todos)

- Create a fresh next project

- Create `todos/page.tsx`

```javascript
export default async function Blog() {
    const res = await fetch('https://sum-server.100xdevs.com/todos')

    const data = await res.json();
    const todos = data.todos;

    console.log("todos", );
    return <div>
        {todos.map((todo: any) => <div key={todo.id}>
            {todo.title}
            {todo.description}
        </div>)}
    </div>
    
}
```

- Try updating the `fetch` requests

#### Clear cache every 10 seconds

```javascript
const res = await fetch('https://sum-server.100xdevs.com/todos', {
    next: { revalidate: 10 }
});
```

#### Clear cache in a next action

```javascript
import { revalidateTag } from 'next/cache'

const res = await fetch('https://sum-server.100xdevs.com/todos', { next: { tags: ['todos'] } })
```

```javascript
'use server'
 
import { revalidateTag } from 'next/cache'
 
export default async function revalidate() {
  revalidateTag('todos')
}
```