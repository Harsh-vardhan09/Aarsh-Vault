# History of Turborepo

1. Created by `Jared Palmer`

2. In December 2021 Acquired/aqui-hired by `Vercel`

3. Mild speculation/came from a random source - Pretty hefty dealp

4. They’ve built a bunch of products, Turborepo is the most used one

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F9309b48d-a889-4930-81f5-b2d9982b130e%2FScreenshot_2024-03-16_at_2.48.17_AM.png?table=block&id=3ba7fd41-5739-4625-a740-4d4219004616&cache=v2)

---

# Turborepo as a build system orchestrator

Turborepo is a `build system orchestrator` .

The key feature of TurboRepo is its ability to manage and optimize the execution of these tasks across your monorepo. It does this through:

1. **Caching**: TurboRepo caches the outputs of tasks, so if you run a task and then run it again without changing any of the inputs (source files, dependencies, configuration), TurboRepo can skip the actual execution and provide the output from the cache. This can significantly speed up build times, especially in continuous integration environments.

2. **Parallelization**: It can run independent tasks in parallel, making efficient use of your machine's resources. This reduces the overall time needed to complete all tasks in your project.

3. **Dependency Graph Awareness**: TurboRepo understands the dependency graph of your monorepo. This means it knows which packages depend on each other and can ensure tasks are run in the correct order.