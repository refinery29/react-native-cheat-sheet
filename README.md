# How to add custom fonts to a React Native app in Xcode

1. Create a new group `Fonts` within your Xcode project
2. Drag and drop fonts from `Finder` to the `Fonts` group you just created, and check your project name in `Add to targets` list
3. Expand your project name folder within the main directory in your project and open `Info.plist`
4. Add `Fonts provided by application` as a new key
5. Add a new item named after the full font name with extension under `Fonts provided by application` for each font you've added to the fonts folder
6. Run `Shift + Command + K` to clean last build
7. Then `Command + B` to start a new build
8. And finally `Command + R` to run the application

If you still see the error `Unrecognized font family` restart your react packager.

### How to use it

```Javascript
var styles = React.StyleSheet.create({
  title: {
    color: '#000',
    fontFamily: 'Font-Name-Without-Extension'
  }
});
```
