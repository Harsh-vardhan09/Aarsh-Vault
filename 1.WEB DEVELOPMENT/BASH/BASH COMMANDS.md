*Terminal is nothing more than another interface for  doing things*

### 1. PWD
- print working directory
- path to current folder

### 2. CD
- Change directory
- To go to the directories

### 3. ls
- to list all the files inside the contents

### 4. mkdir
- make directory
- To create an folders

### 5. Touch
- create an empty file inside a folder

### 6. Cat
- Prints content of a file

```bash
Dell@Aarsh-laptop MINGW64 ~/Downloads/test
$ cat index.txt
afdfdsfasdfafdasd
```

### 7. vi
- lets you edit file using teminal
- vim command
- works same as rebase 
- 
>    i for insert 
    esc for exit 
    :wq! for out for exit with save
     :q! for exit without saving

### 8. mv
- move files from one folder to another

```
Dell@Aarsh-laptop MINGW64 ~/Downloads
$ mv test/a.txt new-folder

Dell@Aarsh-laptop MINGW64 ~/Downloads
$ cd new-folder

```

### 9. cp
- copy files and folder and paste it to a location
```
Dell@Aarsh-laptop MINGW64 ~/Downloads
$ cp test/index.txt new-folder
```

### 10. nvm
- node version manager 
- lets you install node into device

### 11. NPM 
- node package manager
- allows node packages 
- packages which are code that is pre written to use 

### 13. node
- used to run node directly in the cli 

```js
Dell@Aarsh-laptop MINGW64 ~/Downloads
$ node
Welcome to Node.js v24.15.0.
Type ".help" for more information.
> let a =2
undefined
> console.log(a)
2
undefined

```


```node
Dell@Aarsh-laptop MINGW64 ~/Downloads
$ cd test

Dell@Aarsh-laptop MINGW64 ~/Downloads/test
$ touch a.js

Dell@Aarsh-laptop MINGW64 ~/Downloads/test
$ vi a.js

Dell@Aarsh-laptop MINGW64 ~/Downloads/test
$ cat a.js
let a=2;
let b=3;

console.log(a+b);

Dell@Aarsh-laptop MINGW64 ~/Downloads/test
$ node a.js
5

```

### 14. git
- allow to run git commands 
- allows to connect to the git server remote or non remote 