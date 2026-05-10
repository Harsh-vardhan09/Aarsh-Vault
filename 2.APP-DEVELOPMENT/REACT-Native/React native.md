## what is react native?

- A framework for building native mobile applications using JavaScript and React.

### Single codebase

Traditionally, Android and IOS would be two different project.

With React Native, you can create a single project that will run on both Android and iOS.

- Cost-effective development
- code reusability and faster development
- Easy Collaboration
- Native-like performance
- Less maintenance & complexity.

---
## How react native works?

![[_Excalidraw/App development.md#^frame=b3_Ke3lEMuw_f959i7San]]

---

### Under the hood


| Javascript side                                            | native side                                                                          |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| your code(component state, logic) runs in theHermes engine | The actual UI elements ae real native platform components written in other languages |

*you do not write or manage any native code*

---
### *The new architecture*

![[_Excalidraw/App development.md#^frame=jJTWtWnR7yRx3bZsGnRVs]]

- React runs in js runtime 
- React Native process and prepares for native UI
- JSI is used  to cross the boundary
- Fabric handles  the ui 
- Turbo Modules  handles the APIs
---

### EXPO FRAMEWORK

- expo is a framwork built on top of react native that provides a set of tools and  services  to  make it easier to build and deploy react native apps.
- easier to use than react native 


- Development environment  and expo `CLI`
- Rich set of api
- expo router which is a file bases routing
- expo go
- EAS(expo application services)

---

