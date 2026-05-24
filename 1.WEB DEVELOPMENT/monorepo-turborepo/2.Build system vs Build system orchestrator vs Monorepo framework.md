[](https://projects.100xdevs.com/tracks/monorepo/monorepo-5#aa5f4bd7e2984b1a87aedf989696aeaf "Build System Orchestrator")

  
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F61af291c-5f82-4c11-a22c-f1d1110abcb1%2FScreenshot_2024-03-16_at_2.58.02_AM.png?table=block&id=c58a54e2-0660-4e54-b5eb-a7855b0c2502&cache=v2)
#### **Build System**

A build system automates the process of transforming source code written by developers into binary code that can be executed by a computer. For JavaScript and TypeScript projects, this process can include transpilation (converting TS to JS), bundling (combining multiple files into fewer files), minification (reducing file size), and more. A build system might also handle running tests, linting, and deploying applications.

#### **Build System Orchestrator**

TurboRepo acts more like a build system orchestrator rather than a direct build system itself. It doesn't directly perform tasks like transpilation, bundling, minification, or running tests. Instead, TurboRepo allows you to define tasks in your monorepo that call other tools (which are the actual build systems) to perform these actions.

These tools can include anything from tsc, vite etc

#### **Monorepo Framework**

A monorepo framework provides tools and conventions for managing projects that contain multiple packages or applications within a single repository (monorepo). This includes dependency management between packages, workspace configuration.