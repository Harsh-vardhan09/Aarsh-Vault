## 1. What is Tab Navigation?

Tab navigation allows users to switch between top-level screens using a tab bar, usually displayed at the bottom of the screen.

- Commonly used for Home, Search, Profile, and Settings.
- Maintains a separate navigation history for each tab, depending on the navigator.
- In Expo Router, tabs are created using file-based routing and `Tabs`.
    

## 2. Basic Folder Structure

```
app/
├── _layout.tsx
└── (tabs)/
    ├── _layout.tsx
    ├── index.tsx
    ├── search.tsx
    └── profile.tsx
```

- `(tabs)` is a route group and does not appear in the URL.
- `(tabs)/_layout.tsx` defines the tab navigator.
- Each screen file becomes a tab route.

## 3. Creating Tab Navigation

In `app/(tabs)/_layout.tsx`:

```
import { Tabs } from 'expo-router';

export default function TabLayout() {
  return (
    <Tabs>
      <Tabs.Screen
        name="index"
        options={{ title: 'Home' }}
      />
      <Tabs.Screen
        name="search"
        options={{ title: 'Search' }}
      />
      <Tabs.Screen
        name="profile"
        options={{ title: 'Profile' }}
      />
    </Tabs>
  );
}
```

**How it works:**

- `Tabs` creates the tab navigator.
- `Tabs.Screen` configures each tab.
- `name` must match the corresponding route filename.
- `options` customizes the tab's title, icon, and appearance.

## 4. Adding Tab Icons

Use icons from `@expo/vector-icons`.

```
import { Tabs } from 'expo-router';
import { Ionicons } from '@expo/vector-icons';

export default function TabLayout() {
  return (
    <Tabs>
      <Tabs.Screen
        name="index"
        options={{
          title: 'Home',
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="home" color={color} size={size} />
          ),
        }}
      />
      <Tabs.Screen
        name="profile"
        options={{
          title: 'Profile',
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="person" color={color} size={size} />
          ),
        }}
      />
    </Tabs>
  );
}
```

The icon callback receives:

- `color` — the current tab icon color.
- `size` — the configured icon size.
- `focused` — whether the tab is currently selected.

## 5. Important Tab Options

|Option|Purpose|
|---|---|
|`title`|Sets the screen and tab title.|
|`tabBarIcon`|Displays an icon in the tab bar.|
|`tabBarLabel`|Customizes the tab label.|
|`tabBarActiveTintColor`|Sets the active tab's color.|
|`tabBarInactiveTintColor`|Sets inactive tabs' color.|
|`headerShown`|Shows or hides the screen header.|
|`tabBarStyle`|Customizes the tab bar's appearance.|

Example:

```
<Tabs
  screenOptions={{
    headerShown: false,
    tabBarActiveTintColor: '#007AFF',
    tabBarInactiveTintColor: 'gray',
  }}
>
  {/* Tab screens */}
</Tabs>
```

## 6. Hiding a Tab

To keep a screen in the route structure without displaying it in the tab bar, configure it as a hidden tab when appropriate.

```
<Tabs.Screen
  name="settings"
  options={{
    href: null,
  }}
/>
```

This is useful when a route belongs to the tab navigator but should not have a visible tab button.

## 7. Tab Navigation vs. Stack Navigation

|Feature|Tab Navigation|Stack Navigation|
|---|---|---|
|Navigation style|Switch between tabs|Push and pop screens|
|Typical use|Home, Search, Profile|Details, Forms, Subpages|
|Common component|`Tabs`|`Stack`|
|Navigation history|Can maintain history per tab|Follows a screen stack|

Both can be combined. For example, each tab can contain its own stack of nested screens.

## Quick Revision

- `Tabs` creates tab navigation in Expo Router.
- `Tabs.Screen` configures individual tabs.
- File names determine route names.
- `(tabs)` groups related routes without adding a URL segment.
- `tabBarIcon` adds icons.
- `screenOptions` configures shared navigation settings.
- Tabs are best for switching between the main sections of an app.

**Remember:** Use tab navigation for top-level sections and stack navigation for moving deeper into a section.