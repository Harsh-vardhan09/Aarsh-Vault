
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

