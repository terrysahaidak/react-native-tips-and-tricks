# React Native tips and tricks <!-- omit in toc -->

> A curated list of helpful tips and trick for react native developers.

## Table of contents <!-- omit in toc -->

- [Android specific](#android-specific)
  - [*Ripple goes outside of the TouchableNativeFeedback with border radius*](#ripple-goes-outside-of-the-touchablenativefeedback-with-border-radius)
- [iOS specific](#ios-specific)
  - [*Title for ios example*](#title-for-ios-example)
- [Cross-platform](#cross-platform)
  - [*FlatList last item margin bottom*](#flatlist-last-item-margin-bottom)
- [Bundling](#bundling)
  - [*How to use symlinks*](#how-to-use-symlinks)
  - [*Absolute imports*](#absolute-imports)
- [License](#license)

## Android specific

### *Ripple goes outside of the TouchableNativeFeedback with border radius*

__Problem:__

Sometimes you need to implement rounded buttons with ripple animation. You will go ahead and use a `TouchableNativeFeedback` with some `borderRadius`. But you'll notice that ripple animation doesn't follow your border radius and goes outside of rounded container.

<img src="/images/touchable-native-feedback-ripple-border-radius-problem.jpg" alt="Touchable native feedback ripple with border radius problem screenshot" width="400">

__Solution:__

To fix that wrap your `TouchableNativeFeedback` with another `View` with some styles:

```js
<View style={{
  borderRadius: 0,
  backgroundColor: 'transparent',
}}>
  <TouchableNativeFeedback>
    {/* all your content goes here... */}
  </TouchableNativeFeedback>
</View>
```

<img src="/images/touchable-native-feedback-ripple-border-radius-solution.jpg" alt="Touchable native feedback ripple with border radius solution screenshot" width="400">

__Demo:__

[Run snack with the demo](https://snack.expo.io/@terrysahaidak/supportive-celery).

## iOS specific

### *Title for ios example*

## Cross-platform

### *FlatList last item margin bottom*

__Problem:__

Sometimes you might want to add an extra whitespace in the bottom of your FlatList. You will see that neither adding `paddingBottom` to the `style` property of the FlatList nor adding extra margin to your list items makes any effect.

<img src="/images/flatlist-last-item-margin-bottom-problem.jpg" alt="Flatlist last item margin bottom problem screenshot" width="400">

__Solution:__

The solution to this problem is to add `paddingBottom` style to the `contentContainerStyle`. i.e.:

```js
<FlatList
  contentContainerStyle={{paddingBottom: 20}}
  data={data}
  renderItem={renderItem}
  // ...
/>
```

<img src="/images/flatlist-last-item-margin-bottom-result.jpg" alt="Flatlist last item margin bottom result screenshot" width="400">

__Related issues:__

- [react-native#15707](https://github.com/facebook/react-native/issues/15707)

## Bundling

### *How to use symlinks*

__Problem:__

[Symlinks](https://docs.npmjs.com/cli/link.html) are really common and helpful thing when you're developing in monorepo using [lerna](https://github.com/lerna/lerna) or just an example for your library. They will allow you to link your local modules between each other so the module inside your `node_module` folder will be pointing to your local folder and will be up to date with all changes you've made.
Currently metro (the react native packager) doesn't follow symlinks and won't build your bundle.

__Solution:__

To fix that you'll have to:

1. Create a symlink of your package ([here](https://medium.com/dailyjs/how-to-use-npm-link-7375b6219557)) is example).

2. Run inside your react native project [metro-with-symlinks](https://www.npmjs.com/package/metro-with-symlinks) tool.

It will generate you a `rn-cli.config.js` file with all the instructions for metro how to resolve symlinks and build the bundle.

### *Absolute imports*

__Problem:__

Module imports in your project are usually separated in two types:

- import npm modules which are resolved relatively to the `node_modules` folder
- import your own modules

Doing the second, you will always find something like this `import { SomeComponent } from '../SomeComponent';` and it is ok for smaller projects or when you have a relatively independent module. But as your project start to grow, your imports may become a hell: `import { doSomething } from '../../../../../../../some-module';` and it is extremally hard to read or follow this path in your IDE to find the right file. Furthermore, your import will fail if you move dependent file somewhere else during a refactoring.

__Solution:__

Absolute imports in react native.
So, what if we could import our component located in `src/components` from the `src/screens` with something like this:

```js
import { SomeComponent } from 'components/SomeComponent';
```

Just create a `package.json` file under your `src/components` folder with the next content:

```json
{
  "name": "components"
}
```

It does make sense to create such `package.json` for each root module. So imagine you have a file structure like this:

```plain
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

```plain
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
