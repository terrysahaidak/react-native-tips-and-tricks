# React Native tips and tricks <!-- omit in toc -->

> A curated list of helpful tips and trick for react native developers.

## Table of contents <!-- omit in toc -->

- [Android specific](#android-specific)
  - [Title for android example](#title-for-android-example)
- [iOS specific](#ios-specific)
  - [Title for ios example](#title-for-ios-example)
- [Crossplatform](#crossplatform)
  - [Title for crossplatform](#title-for-crossplatform)
- [Bundling](#bundling)
  - [How to use symlinks](#how-to-use-symlinks)
  - [Absolute imports](#absolute-imports)
- [License](#license)

## Android specific

### Title for android example

## iOS specific

### Title for ios example

## Crossplatform

### Title for crossplatform

## Bundling

### How to use symlinks

[Symlinks](https://docs.npmjs.com/cli/link.html) are really common and helpful thing when you're developing in monorepo using [lerna](https://github.com/lerna/lerna) or just an example for your library. They will allow you to link your local modules between each other so the module inside your `node_module` folder will be pointing to your local folder and will be up to date with all changes you've made.
Currently metro (the react native packager) doesn't follow symlinks and won't build your bundle.

To fix that you'll have to:

1. Create a symlink of your package ([here](https://medium.com/dailyjs/how-to-use-npm-link-7375b6219557)) is example).

2. Run inside your react native project [metro-with-symlinks](https://www.npmjs.com/package/metro-with-symlinks) tool.

It will generate you a `rn-cli.config.js` file with all the instructions for metro how to resolve symlinks and build the bundle.

### Absolute imports

__Problem:__

Module imports in your project are usually separated in two types:

* import npm modules which are resolved relatively to the `node_modules` folder
* import your own modules

Doing the second, you will always find something like this `import { SomeComponent } from '../SomeComponent';` and it is ok for smaller projects or when you have a relatively independent module. But as your project start to grow, your imports may become a hell: `import { doSomething } from '../../../../../../../some-module';` and it is extremally hard to read or follow this path in your IDE to find the right file. Furthermore, your import will fail if you move dependent file somewhere else during a refactoring.

__Solution:__

Absolute imports in react native.
So, what if we could import our component located in `src/components` from the `src/screens` with something like this:

```
import { SomeComponent } from 'components/SomeComponent';
```

Just create a `package.json` file under your `src/components` folder with the next content:

```
{
  "name": "components"
}
```

It does make sense to create such `package.json` for each root module. So imagine you have a file structure like this:

```
  src/
    components/
    screens/
    config/
    constants/
    i18n/
    utils/
  index.js
  package.json - your app's main package.json
```

Just create a similar `package.json` for every folder to become resolvable absolutely. And you will have something like this:

```
  src/
    components/
      package.json - "components"
    screens/
      package.json - "screens"
    config/
      package.json - "config"
    constants/
      package.json - "constants"
    i18n/
      package.json - "i18n"
    package.json - "app"
  index.js
  package.json  - your app's main package.json
```


## License

[MIT](LICENSE)