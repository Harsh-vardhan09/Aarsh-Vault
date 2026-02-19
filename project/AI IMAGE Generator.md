- *This is a image generation app that uses an API for generating an image



# CLIENT SIDE:-

- using `reactjs` for the front end.
- using tailwind for UI changes
- Get react router dom using `npm i react-router-dom`
- #### FileSaver: This is solution to save files on the client-side, and is perfect for web apps that generates files on the client.
## Tailwind css:-
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