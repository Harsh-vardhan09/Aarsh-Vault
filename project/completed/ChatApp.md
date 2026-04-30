what user can send

###### JOIN A ROOM
```
{
	"type:join"
	"payload":{
		'roomId':"123"
	}
}
```



###### SEND A MESSAGE
```
{
	"type":"chat",
	"payload":{
		"message":"hi there"
	}
}
```


###### What the server can send/user recives
##### MESSAGE
```
{ 
	"type": "chat", 
	"payload": {
		 "message": "hi there"
	  } 
  }
```