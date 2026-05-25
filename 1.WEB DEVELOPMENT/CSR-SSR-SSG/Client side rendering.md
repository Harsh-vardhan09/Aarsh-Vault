[notes](https://projects.100xdevs.com/tracks/rendering/render-1#23d15c3e6b4b4a54bb9a48e6ed231bd9 "Downsides?")

Client-side rendering (CSR) is a modern technique used in web development where the rendering of a webpage is performed in the browser using JavaScript. Instead of the server sending a fully rendered HTML page to the client

Good example of CSR - React

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Ff7813ee0-6e5a-415a-a8a4-e9b9b8aceca1%2FScreenshot_2024-04-05_at_9.14.58_PM.png?table=block&id=a5fda757-8cae-4a51-9733-a6de02e4c1cd&cache=v2)

Let’s see a react project in action

- Initialise a react project

```javascript
npm create vite@latest
```

- Add dependencies

```javascript
npm i
```

- Start the project

```javascript
npm run build
```

- Serve the project

```javascript
cd dist/
serve
```

Open the network tab and notice how the inital HTML file deosn’t have any content

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F4611fa82-f337-4a35-901c-8a2eb1ab7cee%2FScreenshot_2024-04-05_at_9.22.57_PM.png?table=block&id=71bb352f-ddaf-47dd-8f08-8f9a68ff5a31&cache=v2)

This means that the JS runs and actually `populates` / `renders` the contents on the page

React (or CSR) makes your life as a developer easy. You write components, JS renders them to the DOM.

#### Downsides?

1. Not SEO optimised

2. User sees a `flash` before the page renders

3. Waterfalling problem

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F14e94b35-2af0-4d4d-a498-651cdc4b0ae1%2FScreenshot_2024-04-05_at_9.26.44_PM.png?table=block&id=8ba94d9a-2843-4aa1-875d-56aca7e746b4&cache=v2)