CI- `Continous Integration`
CD- `Continous Depoloyement`

## Github workflow:-
- These are workflow that auto run when user creates or pushes code to github
- these are written in `yaml/yml` in `.github/workflows`
- Yaml stands for `YAMl Aint Markup Language`


## Yaml
- these are written in `yaml/yml` in `.github/workflows`
- Yaml stands for `YAMl Aint Markup Language`
- it is white space sensitive
  
  
### CI = Continuous Integration

> **Whenever I push code, automatically test it.**

Without CI:

```
Write code
   ↓
git push
   ↓
Hope everything works 😅
```

With CI:

```
Write code
   ↓
git push
   ↓
CI starts
   ↓
Install dependencies
   ↓
Run tests
   ↓
Build project
   ↓
✅ Pass / ❌ Fail
```

### CD = Continuous Delivery / Deployment

```
Code
 ↓
Test
 ↓
Build
 ↓
Deploy
 ↓
🚀 Application updated
```

There are two commonly used meanings:

**Continuous Delivery**

> Code is automatically prepared and ready to deploy, but deployment may require approval.

**Continuous Deployment**

> If everything passes, deployment happens automatically.

```
CI → Check my code
CD → Deliver my code
```



  
![[Pasted image 20260826232758.png]]

![[Pasted image 20260826232819.png]]

![[Pasted image 20260826232841.png]]

![[Pasted image 20260826232903.png]]


## Yaml commands
![[Pasted image 20260826233046.png]]

![[Pasted image 20260826233120.png]]

![[Pasted image 20260826233152.png]]

![[Pasted image 20260826233211.png]]

![[Pasted image 20260826233228.png]]