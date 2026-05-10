AXIOS is an external library which is cleaner and better

```js
const axios=require("axios");

  

//fetch
function main(){
    fetch("https://sum-server.100xdevs.com/todos").then(async res=>{
        const json=await res.json();
        console.log(json);
    })
}
  
main();

//axios

async function main(){
    const res=await axios.get("https://sum-server.100xdevs.com/todos")
    // const json=await res.json();
    console.log(res.data.todos.length);  
}

```

***AXIOS is smarter so it knows that the data is JSON so we don't have to `res.json()` it.***


### For POST:

- In axios its easier to just name the axios  
- const res=await axios.post("https://sum-server.100xdevs.com/todos")

**But in fetch** 

```js
function main(){
    fetch("https://sum-server.100xdevs.com/todos",{
        method:"POST",{
        headers:"bearer"
        }
    }).then(async res=>{
        const json=await res.json();
        console.log(json);
    })
}
```