[NOTES](https://projects.100xdevs.com/tracks/rendering/render-2#6c7eff2b15074ef9a3899f5f5eb9876b "Why SSR?")

When the `rendering` process (converting JS components to HTML) happens on the server, it’s called SSR.

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F43cdeaa8-c2e9-4421-815c-838160cb637f%2FScreenshot_2024-04-05_at_9.38.40_PM.png?table=block&id=116ccc50-2b20-4080-b9af-efb9596993a0&cache=v2)

#### Why SSR?

1. SEO Optimisations

2. Gets rid of the waterfalling problem

3. No white flash before you see content

Try creating a NextJS app and notice the HTML file you receive is populated

- Create next app `npx create-next-app`

- Build the project

```javascript
npm run build
```

- Start the NEXT Server

```javascript
npm run start
```

Notice the initial HTML page is populated

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fcb57fca4-8640-46c5-8f4c-ab1d023b3043%2FScreenshot_2024-04-05_at_9.41.59_PM.png?table=block&id=cf121f4a-8c4e-4789-916a-fab0b45c694e&cache=v2)

#### Downsides of SSR?

1. Expensive since every request needs to `render` on the server

2. Harder to scale, you can’t cache to CDNs