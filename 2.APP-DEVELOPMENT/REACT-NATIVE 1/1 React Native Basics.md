*It is cross platform framework that lets us create app and web application for the IOS, android, web application.*

React native transforms normal tags to native language.

we use expo to create the app since expo allows us to scaffold project far cleaner and better

to create the app 

```
npx create-expo-app@latest
```

This creates an predefined app with all the dependency 

*WE can even use templates for the type of project we want*

### `--template`[](https://docs.expo.dev/more/create-expo/#--template)

Running `create-expo-app` with a [Node package manager](https://docs.expo.dev/more/create-expo/?comet_token_override=4238676782498815563268797729731874384914187879362#node-package-managers-support) initializes and sets up a new Expo project using the default template.

You can use the `--template` option to select one of the following templates or pass it as an argument to the option. For example, `--template default`.

> Looking for more templates? Check out the [`--example`](https://docs.expo.dev/more/create-expo/?comet_token_override=4238676782498815563268797729731874384914187879362#--example) option to initialize your project with one of the example apps that demonstrate specific features and integrations.

|Template|Description|
|---|---|
|[`default`](https://github.com/expo/expo/tree/main/templates/expo-template-default)|Default template. Designed to build multi-screen apps. Includes recommended tools such as Expo CLI, Expo Router library and TypeScript configuration enabled. Suitable for most apps.|
|[`blank`](https://github.com/expo/expo/tree/main/templates/expo-template-blank)|Installs minimum required npm dependencies without configuring navigation.|
|[`blank-typescript`](https://github.com/expo/expo/tree/main/templates/expo-template-blank-typescript)|A Blank template with TypeScript enabled.|
|[`tabs`](https://github.com/expo/expo/tree/main/templates/expo-template-tabs)|Installs and configures file-based routing with Expo Router and TypeScript enabled.|
|[`bare-minimum`](https://github.com/expo/expo/tree/main/templates/expo-template-bare-minimum)|A Blank template with native directories (android and ios) generated. Runs [`npx expo prebuild`](https://docs.expo.dev/workflow/continuous-native-generation/?comet_token_override=4238676782498815563268797729731874384914187879362) during the setup.|

### `--example`[](https://docs.expo.dev/more/create-expo/#--example)

Use this option to initialize a project using an example from [expo/examples](https://github.com/expo/examples).

For example:

- Running `npx create-expo-app --example` shows an interactive list of available examples to choose from
- Running `npx create-expo-app --example with-router` sets up a project with the Expo Router library
- Running `npx create-expo-app --example with-react-navigation` sets up a project similar to the default template, but configured with plain React Navigation library