## 1. What is Safe Area View?

A safe area prevents UI elements from overlapping with system interface areas, such as the status bar, notch, and home indicator.

- Keeps content visible within the usable screen area.
    
- Especially important on devices with notches or rounded corners.
    
- In modern Expo projects, prefer `SafeAreaView` from `react-native-safe-area-context`.
    

## 2. Installation

For Expo projects:

```
npx expo install react-native-safe-area-context
```

## 3. Basic Usage

```
import { SafeAreaView } from 'react-native-safe-area-context';
import { Text } from 'react-native';

export default function HomeScreen() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Text>Hello World!</Text>
    </SafeAreaView>
  );
}
```

- `flex: 1` makes the view fill the available space.
    
- The safe area automatically accounts for applicable system insets.
    

## 4. Using SafeAreaProvider

Wrap your app with `SafeAreaProvider` to provide safe area inset information.

```
import { SafeAreaProvider } from 'react-native-safe-area-context';

export default function RootLayout() {
  return (
    <SafeAreaProvider>
      {/* App content */}
    </SafeAreaProvider>
  );
}
```

In Expo Router, this can be placed in the root `app/_layout.tsx`.

## 5. SafeAreaView vs. useSafeAreaInsets

### SafeAreaView

Automatically applies safe area padding.

```
<SafeAreaView style={{ flex: 1 }}>
  {/* Content */}
</SafeAreaView>
```

### useSafeAreaInsets

Provides individual inset values for custom layouts.

```
import { useSafeAreaInsets } from 'react-native-safe-area-context';

const insets = useSafeAreaInsets();
```

The `insets` object contains:

- `top` — top safe area.
    
- `bottom` — bottom safe area.
    
- `left` — left safe area.
    
- `right` — right safe area.
    

Example:

```
const insets = useSafeAreaInsets();

<View style={{ paddingTop: insets.top }}>
  {/* Content */}
</View>
```

Use this hook when you need precise control over padding or positioning.

## 6. Important Props

|Prop|Purpose|
|---|---|
|`edges`|Specifies which edges receive safe area handling.|
|`style`|Applies styling to the container.|
|`initialWindowMetrics`|Provides initial inset measurements when configuring the provider, when appropriate.|

Example:

```
<SafeAreaView
  edges={['top', 'left', 'right']}
  style={{ flex: 1 }}
>
  {/* Content */}
</SafeAreaView>
```

Here, safe area padding is applied to the top, left, and right edges, but not the bottom.

## 7. Important Notes

- React Native's built-in `SafeAreaView` is deprecated; prefer `react-native-safe-area-context`.
    
- Use `SafeAreaProvider` near the root of the application.
    
- Avoid applying safe area padding twice, which can create excessive spacing.
    
- Scrollable screens may require `SafeAreaView` or inset handling depending on the layout.
    

## Quick Revision

- Safe areas prevent content from overlapping system UI.
    
- `SafeAreaView` handles safe area padding automatically.
    
- `SafeAreaProvider` supplies inset information.
    
- `useSafeAreaInsets()` provides individual inset values.
    
- `edges` controls which sides receive safe area padding.
    

**Remember:** Use `SafeAreaView` for standard layouts and `useSafeAreaInsets()` when you need more control.