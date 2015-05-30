Since I just started playing around with [React Native](https://facebook.github.io/react-native/) and there are just few resources available online I've decided to created this repo to write down things that I find interesting or I've spent some time googling/figuring out. 
Hope it helps and makes your time with React Native even more fun!

And of course, feel free to contribute

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

# How to make circle image with React Native
Since `borderRadius` style expects `number` as a value you can't use `borderRadius: 50%`.
To make circle all you have to do is use your image width/height and devide it with 2.

Example for 100x100 image:

```Javacript
// component
<Image style={styles.image} source={{uri: 'http://placehold.it/100x100'}}/>
```

``` Javascript
// styles
var styles = StyleSheet.create({
  image: {
    height: 100,
    borderRadius: 50,
    width: 100
  }
});
```

# React Native Image component can have child components
Because of `HTML` and `<img />` tag it's easy to ignore that React `<Image/>` component can have child components, but it can.

This is how you can do it. If it doesn't look pretty, change image `source` and add styles to make it nicer!

```Javascript
<Image source={{uri: 'http://placehold.it/300x300'}}>
  <Image source={{uri: 'http://placehold.it/100x100'}}/>
  <Text>This is my text</Text>
</Image>
```
