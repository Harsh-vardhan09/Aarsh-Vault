## 1. What is Pressable?

`Pressable` is a React Native component used to detect touch interactions, such as pressing, long pressing, and releasing.

- Used for buttons, clickable cards, icons, and custom interactive elements.
    
- Provides more control over touch interactions than `Button`.
    
- Supports styling based on the press state.
    

## 2. Basic Syntax

```
import { Pressable, Text } from 'react-native';

<Pressable onPress={() => console.log('Pressed!')}>
  <Text>Click Me</Text>
</Pressable>
```

## 3. Important Props

|Prop|Purpose|
|---|---|
|`onPress`|Called when the user presses and releases.|
|`onLongPress`|Called when the user holds the element.|
|`onPressIn`|Called when the press begins.|
|`onPressOut`|Called when the press ends.|
|`disabled`|Disables interaction when `true`.|
|`hitSlop`|Expands the touchable area.|
|`delayLongPress`|Sets the delay before triggering a long press.|
|`style`|Defines styles; can also receive a function based on press state.|

## 4. Styling Based on Press State

`Pressable` provides a `pressed` boolean through its style callback.

```
<Pressable
  onPress={() => console.log('Pressed')}
  style={({ pressed }) => ({
    backgroundColor: pressed ? '#999' : '#007AFF',
    padding: 12,
    borderRadius: 8,
  })}
>
  <Text style={{ color: 'white' }}>Submit</Text>
</Pressable>
```

- `pressed = true` → The element is being pressed.
    
- `pressed = false` → The element is not being pressed.
    

## 5. Handling Long Press

```
<Pressable
  onPress={() => console.log('Short press')}
  onLongPress={() => console.log('Long press')}
  delayLongPress={500}
>
  <Text>Press and Hold</Text>
</Pressable>
```

- `onPress` handles a normal press.
    
- `onLongPress` handles holding the element for the configured delay.
    

## 6. Expanding the Touch Area

Use `hitSlop` to make a small element easier to tap without changing its visual size.

```
<Pressable
  hitSlop={10}
  onPress={() => console.log('Icon pressed')}
>
  <Text>✕</Text>
</Pressable>
```

## 7. Pressable vs. Button vs. TouchableOpacity

|Feature|Pressable|Button|TouchableOpacity|
|---|---|---|---|
|Custom styling|High|Limited|High|
|Press-state styling|Yes|No|Opacity effect|
|Long press|Yes|No|Yes|
|Best use|Custom interactive UI|Simple standard buttons|Touchable UI with opacity feedback|

`TouchableOpacity` is still available, but `Pressable` is generally preferred for new custom interactions.

## Quick Revision

- `Pressable` handles touch interactions.
    
- `onPress` → Normal press.
    
- `onLongPress` → Long press.
    
- `onPressIn` → Press starts.
    
- `onPressOut` → Press ends.
    
- `disabled` → Prevents interaction.
    
- `hitSlop` → Enlarges the touch target.
    
- `style={({ pressed }) => ...}` → Changes styling based on press state.
    

**Remember:** Use `Pressable` when you need a customizable, interactive component in React Native.