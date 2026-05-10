
## Binary search
- Here we are not doing start=mid as we know by dry run that it runs into infinite loop and if we don't do start=mid+1 search space doesn't decrease from that point 
- In binary search search space should always decrease  
- We also learn that since mid+ 1> mid so we don't need to keep mid if we know there is greater element to the right.

```java
❌ while(start<end){  
    int mid=start+(end-start)/2;  
    if(arr[mid]<arr[mid+1]){  
        start=mid;  
    }else{  
        end=mid;  
    }  
}

✅ while(start<end){  
    int mid=start+(end-start)/2;  
    if(arr[mid]<arr[mid+1]){  
        start=mid+1;  
    }else{  
        end=mid;  
    }  
}
```

---
## Complexity:

- **Time complexity: its the function that gives us the relationship about the time that grows as the input grows.
	`O(1)<O(logn)<O(nlogn)<O(n)<O(n^2)`
	- we only care about worst case complexity since it its good for the worst case it will perform good for best case.
- **Space Complexity: this is the main and auxiliary space taken by an algo to solve a problem. 

### Recursion complexity:-

- This is the space taken there are two types of recursion:-
	- linear recursion - fibonacci
	- Divide and conquer - binary search 
- *For the divide and conquer there are many method to find time complexity *
	1. plug and chuck
	2. tree 
	3. masters method
	4. Akra bazzi

***
### NP Problems:-
- 