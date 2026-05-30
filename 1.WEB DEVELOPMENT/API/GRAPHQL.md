## Problems with REST API:-

#### Problem 1
- In rest api server send the whole data.
- even though we need only some data.
- like fetching `title,id,status` in todos.
- but we only need title and status 
- we can clear it in frontend but we  are still getting it from backend
- wastage of bandwidth

#### Problem 2
- In case we need the user with the details
- we get user details from todo so we need two calls

***`GraphQL` send only the needed to do and create an nested query for the user which send  the data which i want***

