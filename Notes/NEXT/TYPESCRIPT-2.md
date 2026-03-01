
## Pick
- pick allows you to create a new type by selecting a set of properties(`keys`) from an existing type(`types`)

```ts
interface User{
    id:string,
    name:string,
    age:number,
    email:string,
    password:string

}

// const user:User=User.find({})//database call

//we can do it by creating a new prop where we can call it as an prop
//this will cause if we make a change in the User we need to change in the updated/new prop as well

//so we use pick

type UpdateProps=Pick<User,'name'|'age'|'email'>
//for the updatedProps we pick name age email

function UpdateUser(updateProps:UpdateProps){

    //hit the database to update the user

}
```

## Partial
- ==Partial== *makes all the properties of a type optional, creating a type with the same properties*.
- But marked as optional.

```ts
type UpdateProps=Pick<User,'name'|'age'|'email'>
type UpdatePropsOptional=Partial<UpdateProps>;

function UpdateUser(updateProps:UpdatePropsOptional){

    //hit the database to update the user

}
```

## Read Only:
- When you have a configuration object that should not be altered after initialization.
- Making it read only ensure its properties cannot be changed.

```ts
  
const user={
    name:"harsh",
    age:32
}

user.name='adsfsd';

```

*we can do this even though the user is const because we are not changing its reference of the user not for the value in the object or array.*
```ts
type ReadUser={
    readonly name:string,
    readonly age:number
}

const obj:ReadUser={
    name:"harsh",
    age:32,
}

const obj2:Readonly<ReadObj>={
    name:"harsh",
    age:32,
}

obj.age=12;//here ts is complaining since this is readonly now
```

## Records and map:
- ### Records
	- it lets you cleaner type to the object
	- its an typescript concept
	
```ts
type UsersRecord=Record<string,{name:string,age:number}>

const user2:UsersRecord={
    "ras@qd1":{
        name:'aarsh',
        age:21
    },
    "ras@qd2":{
        name:"harsh",
        age:32
    }

}
```


- ### Maps:
	- This is a JS concept where we can use to generate key value pair.

```ts
type UsersMap={
    name:string,
    age:number
}

  
const UserMap=new Map<string,UsersMap>();

UserMap.set("ras@qd1",{
        name:'aarsh',
        age:21
    })

UserMap.set("ras@qd2",{
        name:"harsh",
        age:32
    })

  
const newUser=UserMap.get("ras@qd2")

console.log(newUser);
```

## Exclude
- Its a function that can accept several types of inputs but you want to exclude specific types from being passed to it.

```ts
// exclude
//it lets you exclude a bunch of literals

type EventType='click'|'scroll'|'mousemove'

type ExcludeEvent=Exclude<EventType,'scroll'>;//click and ,mouse move
  
const handleEvent=(event:ExcludeEvent)=>{
    console.log(`handle event:${event}`);
}

  
handleEvent('click');
```


## Type interface in Zod:

- When using ZOD, we're done runtime validation.
- It makes sure the users is sending right input to update their profile information

```ts
import { z } from "zod"
import express  from "express"
const app=express();

  
const userProfileSchema=z.object({
    name:z.string().min(1,{message:'name cannot be empty'}),
    email:z.string().email({message:'email cannot be empty'}),

    age:z.number().min(18,{message:"you must at least 18 year old"}).optional()

})
 

type FinalUserSchema=z.infer<typeof userProfileSchema>;//infer to the same type of the profileSchema

app.put("/user",(req,res)=>{
    const {success} =userProfileSchema.safeParse(req.body)

    const updateBody:FinalUserSchema=req.body;//how to assign a type to updateBody?

    if(!success){
        res.status(411).json({});
        return
    }
    
    //update the db
    res.json({
        message:"user updated"
    })
})
```