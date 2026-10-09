## 1. What is Theming?

Theming allows an app to change its colors and appearance based on the user's preference.

- **Light theme:** Light background with dark text.
- **Dark theme:** Dark background with light text.
- **System theme:** Automatically follows the device's appearance settings.

Example:

- Light mode → White background, black text.
- Dark mode → Black background, white text.

## 2. useColorScheme()

React Native provides the `useColorScheme()` hook to detect the device's current color scheme.

```
import { View, Text, useColorScheme } from 'react-native';

export default function App() {
  const theme = useColorScheme();

  return (
    <View style={{
      flex: 1,
      backgroundColor: theme === 'dark' ? '#121212' : '#FFFFFF',
      justifyContent: 'center',
      alignItems: 'center',
    }}>
      <Text style={{
        color: theme === 'dark' ? '#FFFFFF' : '#000000',
      }}>
        Hello World
      </Text>
    </View>
  );
}
```

### How it works

1. `useColorScheme()` detects the system theme.
    
2. It returns `'light'`, `'dark'`, or potentially `null`.
    
3. We check the returned value.
    
4. Colors change based on the current theme.
    

**Note:** This follows the system preference. It does not independently save a user's manual theme choice.

---

## 3. Creating Light and Dark Color Themes

Instead of writing conditional colors everywhere, store them in objects.

```
const lightTheme = {
  background: '#FFFFFF',
  text: '#000000',
  card: '#F2F2F2',
};

const darkTheme = {
  background: '#121212',
  text: '#FFFFFF',
  card: '#1E1E1E',
};
```

Select the theme:

```
const theme = colorScheme === 'dark'
  ? darkTheme
  : lightTheme;
```

Now use `theme.background`, `theme.text`, and `theme.card` throughout your UI.

---

## 4. Let the User Choose the Theme

Sometimes users want to select Light or Dark mode manually instead of following the device.

React's `useState()` can store the selected theme while the app is running.

```
import { useState } from 'react';
import { View, Text, Button } from 'react-native';

const lightTheme = {
  background: '#FFFFFF',
  text: '#000000',
};

const darkTheme = {
  background: '#121212',
  text: '#FFFFFF',
};

export default function App() {
  const [isDark, setIsDark] = useState(false);
  const theme = isDark ? darkTheme : lightTheme;

  return (
    <View style={{
      flex: 1,
      backgroundColor: theme.background,
      justifyContent: 'center',
      alignItems: 'center',
    }}>
      <Text style={{ color: theme.text }}>
        Current Theme: {isDark ? 'Dark' : 'Light'}
      </Text>

      <Button
        title="Toggle Theme"
        onPress={() => setIsDark(!isDark)}
      />
    </View>
  );
}
```

**Limitation:** This example resets to Light mode when the app restarts because the preference is not saved.

---

## 5. Save the User's Preference

To remember the selected theme after the app restarts, store it using persistent storage, such as `AsyncStorage`.

Install it in an Expo project:

```
npx expo install @react-native-async-storage/async-storage
```

Basic example of saving and reading the preference:

```
import AsyncStorage from '@react-native-async-storage/async-storage';

// Save the preference
await AsyncStorage.setItem('theme', 'dark');

// Read the preference
const savedTheme = await AsyncStorage.getItem('theme');
```

You would load this value when the app starts and update the theme state accordingly.

**Important:** The code above illustrates storage only. In a real app, load the saved value before rendering the final theme to avoid a flash of the wrong colors.

---

## 6. Best Approach: System + Manual Preference

A common theme system supports three choices:

|Preference|Behavior|
|---|---|
|`system`|Follows device appearance|
|`light`|Always uses the light theme|
|`dark`|Always uses the dark theme|

Example logic:

```
const systemScheme = useColorScheme();
const [preference, setPreference] = useState('system');

const activeTheme =
  preference === 'system'
    ? systemScheme
    : preference;
```

Use `activeTheme === 'dark'` to select dark colors; otherwise use light colors.

For a complete implementation, combine this logic with persistent storage and a shared theme context so every screen uses the same preference.

---

## 7. Theme in Expo Router

In an Expo Router project, place shared theme logic in a context or custom hook and make it available to all screens through the root layout.

Example structure:

```
app/
├── _layout.tsx
├── index.tsx
└── settings.tsx

context/
└── ThemeContext.tsx

constants/
└── Colors.ts
```

- `ThemeContext.tsx` → Stores the preference and exposes theme colors.
    
- `Colors.ts` → Contains light and dark color definitions.
    
- `_layout.tsx` → Wraps the app in the theme provider.
    
- `settings.tsx` → Lets users change their preference.
    

This prevents each screen from managing its own separate theme.


---


## 8.  Creating themed view 

```
import { useColorScheme, View } from "react-native";

import { Colors } from "../constants/Colors";

const ThemedView = ({ style, ...props }) => {
  const colorScheme = useColorScheme();
  const theme = Colors[colorScheme] ?? Colors.light;

  return (
    <View style={[{ backgroundColor: theme.background }, style]} {...props} />
  );

};

  

export default ThemedView;
```

*Auto uses children when we don't use closing view tag in react, So we don't have to use `{children}`*

---

## 9. Quick Revision

- `useColorScheme()` → Detects the device's appearance.
- `useState()` → Stores the current selection in memory.
- `AsyncStorage` → Persists the selection across app restarts.
- `Context API` → Shares theme state across screens.
- Theme objects → Keep colors organized and reusable.
- `system`, `light`, `dark` → Common preference options.

**Remember:** Detecting the system theme and remembering a manual user preference are two different things. A good theme system supports both.