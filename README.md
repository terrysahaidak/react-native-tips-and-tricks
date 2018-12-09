# React Native tips and tricks <!-- omit in toc -->

> A curated list of helpful tips and trick for react native developers.

## Table of contents <!-- omit in toc -->

- [Android specific](#android-specific)
  - [Title for android example](#title-for-android-example)
- [iOS specific](#ios-specific)
  - [Title for ios example](#title-for-ios-example)
- [Crossplatform](#crossplatform)
  - [Title for crossplatform](#title-for-crossplatform)
  - [FlatList last item margin bottom](#flatlist-last-item-margin-bottom)
- [Bundling](#bundling)
  - [How to use symlinks](#how-to-use-symlinks)
- [License](#license)

## Android specific

### Title for android example

## iOS specific

### Title for ios example

## Crossplatform

### Title for crossplatform

### FlatList last item margin bottom

[Related issue](https://github.com/facebook/react-native/issues/15707)

Sometimes you might want to add an extra whitespace in the bottom of your FlatList. You will see that neither adding `paddingBottom` to the `style` property of the FlatList nor adding extra margin to your list items makes any effect.

__Problem:__

<img src="/images/flatlist-last-item-margin-bottom-problem.jpg" alt="Problem Screenshot" width="400">


The solution to this problem is to add `paddingBottom` style to the `contentContainerStyle`. i.e.:

```
<FlatList
  contentContainerStyle={{paddingBottom: 20}}
  data={data}
  renderItem={renderItem}
  // ...
/>
```

__Result:__

<img src="/images/flatlist-last-item-margin-bottom-result.jpg" alt="Result Screenshot" width="400">

## Bundling

### How to use symlinks

[Symlinks](https://docs.npmjs.com/cli/link.html) are really common and helpful thing when you're developing in monorepo using [lerna](https://github.com/lerna/lerna) or just an example for your library. They will allow you to link your local modules between each other so the module inside your `node_module` folder will be pointing to your local folder and will be up to date with all changes you've made.
Currently metro (the react native packager) doesn't follow symlinks and won't build your bundle.

To fix that you'll have to:

1. Create a symlink of your package ([here](https://medium.com/dailyjs/how-to-use-npm-link-7375b6219557)) is example).

2. Run inside your react native project [metro-with-symlinks](https://www.npmjs.com/package/metro-with-symlinks) tool.

It will generate you a `rn-cli.config.js` file with all the instructions for metro how to resolve symlinks and build the bundle.

## License

[MIT](LICENSE)
