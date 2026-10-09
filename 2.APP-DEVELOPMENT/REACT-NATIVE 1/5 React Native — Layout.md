## 1. What is a Layout Page?

In Expo Router, `_layout.tsx` defines how screens are organized and displayed.

It is used to configure navigation shared by multiple screens.

**File structure:**

```
app/
├── _layout.tsx
├── index.tsx
├── about.tsx
└── profile.tsx
```

The `_layout.tsx` file controls the navigation structure for `index.tsx`, `about.tsx`, and `profile.tsx`.

**Important:** `_layout.tsx` is not a normal screen. It defines how screens are rendered within the navigation structure.

---

## 2. Stack

`Stack` is used for stack-based navigation, where screens are placed on top of one another.

For example:

- Home → Profile
- Profile → Settings
- Pressing Back returns to the previous screen.

### Example: `_layout.tsx`

```
import { Stack } from 'expo-router';

export default function RootLayout() {
  return <Stack />;
}
```

Expo Router automatically registers the routes inside the layout.

### Configure screen options

```
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <Stack>
      <Stack.Screen
        name="index"
        options={{ title: 'Home' }}
      />
      <Stack.Screen
        name="about"
        options={{ title: 'About' }}
      />
    </Stack>
  );
}
```

### Common Stack options

|Option|Purpose|
|---|---|
|`title`|Sets the screen title|
|`headerShown`|Shows or hides the header|
|`presentation`|Controls how a screen is presented, such as a modal|
|`animation`|Controls supported screen transition animations|

Example:

```
<Stack.Screen
  name="profile"
  options={{
    title: 'My Profile',
    headerShown: true,
  }}
/>
```

**Use Stack when:** You want navigation between screens with a navigation history and Back behavior.

---

## 3. Slot

`Slot` renders the current matching child route inside a layout.

Think of it as a **placeholder for the active screen**.

### Example: `_layout.tsx`

```
import { Slot } from 'expo-router';

export default function RootLayout() {
  return <Slot />;
}
```

If the current route is `index.tsx`, its screen appears in the slot. If the route changes to `about.tsx`, that screen appears instead.

### File structure

```
app/
├── _layout.tsx
├── index.tsx
└── about.tsx
```

The layout provides the placeholder, and Expo Router renders the matching route inside it.

### When to use Slot?

Use `Slot` when you want to render child routes without adding a Stack or Tabs navigator through that layout.

For example, you might use it when creating a simple shared wrapper around several screens.

```
import { View } from 'react-native';
import { Slot } from 'expo-router';

export default function RootLayout() {
  return (
    <View style={{ flex: 1, padding: 16 }}>
      <Slot />
    </View>
  );
}
```

Here, each matching child screen appears inside the padded `View`.

---

## 4. Stack vs Slot

|Feature|Stack|Slot|
|---|---|---|
|Purpose|Provides stack navigation|Renders the matching child route|
|Navigation history|Managed by the Stack navigator|Does not create a navigator itself|
|Header|Can provide a navigation header|Does not provide a header by itself|
|Screen transitions|Supports stack transitions|Does not provide stack transitions by itself|
|Best for|Typical screen-to-screen navigation|Custom layouts and route placeholders|

**Remember:** `Stack` manages navigation; `Slot` renders the active route.

---

## 5. Complete Example

### Project structure

```
app/
├── _layout.tsx
├── index.tsx
└── about.tsx
```

### `app/_layout.tsx`

```
import { Stack } from 'expo-router';

export default function RootLayout() {
  return <Stack />;
}
```

### `app/index.tsx`

```
import { View, Text } from 'react-native';
import { Link } from 'expo-router';

export default function Home() {
  return (
    <View>
      <Text>Home Screen</Text>
      <Link href="/about">Go to About</Link>
    </View>
  );
}
```

### `app/about.tsx`

```
import { View, Text } from 'react-native';

export default function About() {
  return (
    <View>
      <Text>About Screen</Text>
    </View>
  );
}
```

When the app starts, the Stack displays the Home screen. Clicking the link navigates to About, and the Stack manages the navigation history.

---

## 6. Quick Revision

- `_layout.tsx` → Defines the layout and navigation structure for child routes.
- `Stack` → Manages stack navigation, headers, and screen transitions.
- `Slot` → Renders the matching child route without creating a navigator.
- `Tabs` → Provides tab-based navigation.
- `Link` → Navigates to another route.

### Easy way to remember

- **Layout:** Where and how child screens are organized.
- **Stack:** How screens are pushed and popped.
- **Slot:** Where the current child screen appears.

## Official Documentation

- [Expo Router — Layouts](https://docs.expo.dev/router/basics/layout/)
- [Expo Router — Stack](https://docs.expo.dev/router/advanced/stack/)
- [Expo Router — Slot](https://docs.expo.dev/versions/latest/sdk/router/#slot)