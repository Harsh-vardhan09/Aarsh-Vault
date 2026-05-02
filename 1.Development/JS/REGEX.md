- *It stands for Regular expression*.
- Used for pattern matching and string matching.

```
[abc]         a,b or c
[^abc]        any character except a,b,c
[a-z]         a to z
[A-Z]         A to Z
[a-z A-Z]     a to z,A to Z
[0-9]         0 to 9
```

### Quantifiers:

```
[ ]?       occurs 0 or 1 time
[ ]+       occurs  1 time or more
[ ]*       occurs  0 time or more
[ ]{n}     occurs  n time or more
[ ]{x,y}   occurs  min x and max y time 
```

### Regex Meta characters:

- \ tells computer to treat following characters as search characters 
- \ is called escape character

```
\d        [0-9]
\D        [^0-9]
\w        [a-z A-Z 0-9]
\W        [^a-z A-Z 0-9]
```


#### Example of Email address REGEX

```
[a-zA-Z0-9_\-\.]+[@][a-z]+[\.][a-z]{2,3}
```

Here the first can be any letter a-z capital number `- .` all *we use `\` to escape the internal working and make the letter next a literal.

Then @ should be there as well as `.` 
after there can be **yahoo, Gmail**  anything and at end either in or com so only 2 to 3 letter
