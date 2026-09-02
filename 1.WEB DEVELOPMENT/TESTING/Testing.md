
>[!tip] Application testing
> ***why do we need testing?***
>- When new feature is added we  create a test for it if test  fails for the old test in the function.
>- ==Old feature breaks due to new feature==
>- To test  all the feature it takes a lot of time and may lead to human error


`To fix these issues we do,`

## Automated  testing:-

- we write code so it test the requirement.
- This automatically test the function based on test cases and functionalities.


![[Pasted image 20260903025929.png]]

![[Pasted image 20260903030528.png]]

`Here we are testing if the greet functions using assert  with the expected output`

***This is UNIT TESTING***

## Types of Testing:-

1. **Unit testing :** Test the smallest unit  like a function
2. **Integration Testing :**  we test the whole thing together as  an  integrated functionality. 
3. **End-To-End Testing :** This is the only testing not done in isolation. Tested like the user uses the app. `This is slowest`
![[Pasted image 20260903031512.png]]


## AAA

- This is an  method to write test in `UNIT TESTING` 
- This has three steps
- `Arrange, Act, Assert`
- Arrange: Setup everything needed for the test
- Act: Execute the code function you want to test
- Assert: Check whether the result is what you expected

   > **Arrange** = Prepare  
     **Act** = Execute  
     **Assert** = Verify
### Testing 

```js
import { greet } from "../app.js";
import { test, suite } from "node:test";
import assert from "node:assert";

// suite and describe are aliases, you can use either one
suite("greet function tests", () => {

  test("greet returns the correct greeting", () => {

    //AAA

    /*
        Arrange
        Act
        Assert
    */

    const expected = "Hello, World"; // this is arrange
    const actual = greet("World"); //this is act
    assert.strictEqual(actual, expected); //this is assert

  });

});
```


>[!tip] Test and Suite
> A **test** checks one specific piece of behavior in your code.
> A **suite** is a **group/container of related tests**.

