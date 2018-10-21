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

If you still see the error `Unrecognized font family` restart your react packager or add the following to the `AppDelegate.m` in order to print all available fonts: `NSLog (@"Font families: %@", [UIFont familyNames]);`

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

# Make your images load faster
You can use the `fadeDuration` prop in React Native images to set the time it takes to load on the screen, you can set it to 0 to make it load asap.

```Javascript
<Image fadeDuration={0} source={{uri: 'http://placehold.it/300x300'}} />
```

# Text transform uppercase with React Native styles
I wasn't able to find react native style that does CSS `text-transform: uppercase;`. To make my `<Text>` component uppercased I've used javascript fallback.

```Javascript
<Text style={styles.published}>{this.props.myText.toUpperCase()}</Text>
```

# React Native Text component `numberOfLines` default value
Number of lines expects integer to be passed in as specified in [docs](https://facebook.github.io/react-native/docs/text.html#numberoflines).

I've tried using `null` and it works but it logs warning in console: `numberOfLines` expects `number` not `null`.
Searching react repo for `numberOfLines` didn't help and I just tried out setting it to `0` and it worked out without any wornings.

Example component that is showing just first 10 lines of text and on tap shows the rest.

```javascript
class Article extends Component {
  constructor() {
    super();

    this.state = {
      // initial number of lines
      numberOfLines: 10
    }
  }

  render() {
    var showMore = this.state.numberOfLines ? <Text style={{color: '#f00'}}>{'SHOW MORE \u25BC'}</Text> : null;

    return (
      <ScrollView>
        <TouchableHighlight onPress={() => this.setState({numberOfLines:0})} >
          <View>
            <Text numberOfLines={this.state.numberOfLines}>{this.props.whateverLongText.youHave}</Text>
            {showMore}
          </View>
        </TouchableHighlight>
      </ScrollView>
    );
  }
}
```

# Get device width and height
There are quite a few ways to do this, but an easy to use module is [react-native-device](https://www.npmjs.com/package/react-native-device)

``` npm install react-native-device --save```

Then, to use in your project:

```Javascript
var Device = require('react-native-device');

if (Device.isIpad()) {
  // return iPad layout
  } else {
  // return iPhone layout
}

var deviceWidth = Device.width() // return device width
var deviceHeight = Device.height() // return device width

var deviceKind = Device.kind() // returns kind of device (iPad, iPhone4, iPhone6, etc...)

```

# Extending and overriding styles with inline styles and how to use dynamic styles
If you want to override one/more styles defined within `React.StyleSheet.create` you can do it inline.
Instead of using `style={styles.myStyle}` you can pass in array of style properties:

```Javascript

<View style={[ styles.myStyle, {color:'red'}, {something: 'else'} ]} />

<Text style={[ styles.myText, {width: Device.width} ]} />
```

# Preventing Image Flash after ActionSheetIOS is used
Sometimes after a user has shared something on your app via the `ActionSheetIOS` component, images that were previsouly displayed in your app will disappear for a moment before getting re-rendered, causing a flicker. To prevent this you can set the Image's `defaultSource` to the main source.

```Javascript
<Image accessibilityLabel='x-icon' style={styles.xIcon} source={requireImage('x-icon')} defaultSource={requireImage('x-icon')} />
```
