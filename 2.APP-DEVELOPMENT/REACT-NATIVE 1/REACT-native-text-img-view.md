Components we create in react native are just functions

- we create components
- Use in built components

`EX- view, img, text`

### What is View?

- `View` is similar to div in html
- `View` is the fundamental building block of a React Native UI.
- It works similarly to a `<div>` in HTML.
- It is used to group components and create layouts.
- It supports styling, touch handling, and nesting.
- It default uses flexbox

### Syntax

```
import { View } from 'react-native';

export default function App() {
  return (
    <View style={{ padding: 20, backgroundColor: 'lightblue' }}>
      <View style={{ height: 100, backgroundColor: 'tomato' }} />
    </View>
  );
}
```

### Common View Styles

|Property|Purpose|
|---|---|
|`width`|Sets the width|
|`height`|Sets the height|
|`backgroundColor`|Sets the background color|
|`padding`|Adds space inside the component|
|`margin`|Adds space outside the component|
|`borderWidth`|Sets border thickness|
|`borderRadius`|Rounds the corners|
|`flex`|Controls how available space is distributed|
|`alignItems`|Aligns children along the cross axis|
|`justifyContent`|Aligns children along the main axis|

### Example: Card Layout

```
import { View } from 'react-native';

export default function App() {
  return (
    <View style={{
      margin: 20,
      padding: 20,
      backgroundColor: '#eee',
      borderRadius: 12,
      alignItems: 'center',
    }}>
      <View style={{
        width: 100,
        height: 100,
        backgroundColor: 'skyblue',
        borderRadius: 10,
      }} />
    </View>
  );
}
```

### Important Points

- `View` is not an HTML element; it is a React Native component.
- Use nested `View` components to create complex layouts.
- React Native uses Flexbox by default, with `flexDirection: 'column'`.
- Use `StyleSheet.create()` to organize reusable styles.

---

## 2. Text Component

### What is Text?

- `Text` is used to display textual content in React Native.
- It works similarly to text elements such as `<p>` and `<span>` in HTML.
- Text must be placed inside a `Text` component rather than directly inside a `View`.
- Nested `Text` components support different styles within the same paragraph.
    

### Syntax

```
import { Text, View } from 'react-native';

export default function App() {
  return (
    <View>
      <Text>Hello, React Native!</Text>
    </View>
  );
}
```

### Common Text Styles

|Property|Purpose|
|---|---|
|`color`|Sets the text color|
|`fontSize`|Sets the font size|
|`fontWeight`|Controls font thickness|
|`fontStyle`|Sets normal or italic text|
|`textAlign`|Aligns text: left, center, right|
|`textDecorationLine`|Adds underline or line-through|
|`lineHeight`|Sets the height of each line|
|`letterSpacing`|Controls spacing between characters|
|`textTransform`|Changes capitalization|

### Example: Styled Text

```
import { Text, View } from 'react-native';

export default function App() {
  return (
    <View style={{ padding: 20 }}>
      <Text style={{
        fontSize: 24,
        fontWeight: 'bold',
        color: 'blue',
      }}>
        Welcome!
      </Text>

      <Text style={{
        fontSize: 16,
        color: 'gray',
        marginTop: 10,
      }}>
        Learn React Native step by step.
      </Text>
    </View>
  );
}
```

### Nested Text

```
import { Text } from 'react-native';

export default function App() {
  return (
    <Text>
      Hello{' '}
      <Text style={{ fontWeight: 'bold', color: 'blue' }}>
        Developer
      </Text>
      !
    </Text>
  );
}
```

### Important Points

- Use `Text` for all displayed text.
- Nested `Text` components allow inline styling.
- Text styling is applied through the `style` prop
- `numberOfLines` limits the number of displayed lines.

Example:

```
<Text numberOfLines={2} ellipsizeMode="tail">
  This is a long paragraph that will be limited to two lines.
</Text>
```

---

## 3. Image Component

### What is Image?

- `Image` displays images in React Native.
    
- It can display local images, remote images, and other supported image sources.
    
- Unlike HTML's `<img>` tag, React Native uses the `Image` component.
    
- The `source` prop specifies where the image comes from.
    

### A. Local Image

Store the image inside your project, for example:

```
my-app/
├── assets/
│   └── logo.png
├── App.js
```

Example:

```
import { Image } from 'react-native';

export default function App() {
  return (
    <Image
      source={require('./assets/logo.png')}
      style={{ width: 150, height: 150 }}
    />
  );
}
```

**Note:** The path inside `require()` must be a static path known at build time.

### B. Remote Image

```
import { Image } from 'react-native';

export default function App() {
  return (
    <Image
      source={{
        uri: 'https://reactnative.dev/img/tiny_logo.png',
      }}
      style={{ width: 100, height: 100 }}
    />
  );
}
```

**Important:** Remote images generally need explicit dimensions through `width` and `height`.

### Common Image Props

|Prop|Purpose|
|---|---|
|`source`|Specifies the image source|
|`style`|Sets image dimensions and appearance|
|`resizeMode`|Controls how the image fits its container|
|`onLoad`|Runs when the image loads successfully|
|`onError`|Runs if the image fails to load|
|`onLoadEnd`|Runs after loading completes, successfully or unsuccessfully|
|`accessibilityLabel`|Describes the image for accessibility|

### Resize Modes

|Mode|Behavior|
|---|---|
|`cover`|Fills the area; may crop the image|
|`contain`|Shows the entire image; may leave empty space|
|`stretch`|Stretches the image to fit|
|`center`|Centers the image without scaling it|
|`repeat`|Repeats the image to fill the area where supported|

Example:

```
<Image
  source={{ uri: 'https://reactnative.dev/img/tiny_logo.png' }}
  style={{ width: 200, height: 120 }}
  resizeMode="contain"
/>
```

### Image with Rounded Corners

```
<Image
  source={require('./assets/logo.png')}
  style={{
    width: 150,
    height: 150,
    borderRadius: 75,
  }}
/>
```

For a circular image, use equal width and height and set `borderRadius` to half the dimensions.

---

## 4. Combined Example — View, Text & Image

```
import { View, Text, Image, StyleSheet } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Image
        source={{ uri: 'https://reactnative.dev/img/tiny_logo.png' }}
        style={styles.image}
        resizeMode="contain"
      />

      <Text style={styles.title}>React Native</Text>

      <Text style={styles.subtitle}>
        Build mobile applications using JavaScript and React.
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  image: {
    width: 100,
    height: 100,
    borderRadius: 50,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginTop: 16,
  },
  subtitle: {
    fontSize: 16,
    color: 'gray',
    textAlign: 'center',
    marginTop: 8,
  },
});
```

### How It Works

1. `View` acts as the main container.
2. `Image` displays the remote image.
3. `Text` displays the title and description.
4. `StyleSheet.create()` keeps styling organized and reusable.
5. Flexbox centers the content vertically and horizontally.

---

## 5. HTML vs React Native

|HTML|React Native|Purpose|
|---|---|---|
|`<div>`|`<View>`|Container and layout|
|`<p>`|`<Text>`|Display text|
|`<span>`|Nested `<Text>`|Inline text styling|
|`<img>`|`<Image>`|Display images|
|CSS|JavaScript style objects|Styling|

### Key Differences

- React Native components render native UI elements rather than HTML elements.
- Styles are JavaScript objects, not regular CSS files
- Style property names use camelCase, such as `backgroundColor`.
- Most numeric dimensions represent density-independent units.
- React Native does not support every CSS property.

---

## 6. Quick Revision

- **View:** Used for layout, containers, grouping, and styling.
- **Text:** Used to display text and style inline text.
- **Image:** Used to display local and remote images.
- **source:** Defines the image location.
- **resizeMode:** Defines how an image fits its available space.
- **style:** Applies styles to React Native components.
- **StyleSheet.create():** Organizes reusable styles.
- **Flexbox:** The default layout system in React Native.

## 7. Official Documentation

- [View](https://reactnative.dev/docs/view)
    
- [Text](https://reactnative.dev/docs/text)
    
- [Image](https://reactnative.dev/docs/image)
    
- [StyleSheet](https://reactnative.dev/docs/stylesheet)
    
- [Flexbox](https://reactnative.dev/docs/flexbox)