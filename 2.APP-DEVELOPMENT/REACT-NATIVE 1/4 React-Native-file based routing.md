## 1. What is File-Based Routing?

File-based routing is a routing system where **the files and folders inside a specific directory automatically define your app's screens and URLs**.

Instead of manually defining every screen and its route, you create files, and the router maps them to routes.

Expo Router is a file-based routing library built on top of React Navigation.

### Example

```text
app/
├── index.tsx
├── about.tsx
└── profile.tsx
```

These files automatically create the following routes:

|File|Route|Screen|
|---|---|---|
|`index.tsx`|`/`|Home screen|
|`about.tsx`|`/about`|About screen|
|`profile.tsx`|`/profile`|Profile screen|

**Remember:** The filename determines the route, and the component inside the file determines what the screen displays.

---

## 2. Why Do We Need Routing?

Most mobile applications contain multiple screens, such as:

- Home
- Login
- Profile
- Settings
- Product details

Routing allows users to navigate between these screens.

Without a routing system, you would need to manage which screen is visible and how users move between screens manually.

Expo Router provides:

- Automatic route registration.
- Navigation between screens.
- Dynamic routes using parameters.
- Nested layouts and navigation groups.
- Deep linking support.

---

## 3. Setting Up Expo Router

For a new Expo project, you can create an app with Expo Router already configured:

```bash
npx create-expo-app@latest my-app
cd my-app
npx expo start
```

For an existing Expo project, follow the installation and configuration instructions in the [official Expo Router documentation](https://docs.expo.dev/router/installation/).

The examples below use the `app/` directory convention. Some newer Expo projects use `src/app/` instead. Use the route directory configured in your project.

---

## 4. Basic File Structure

```text
my-app/
├── app/
│   ├── _layout.tsx
│   ├── index.tsx
│   ├── about.tsx
│   └── profile.tsx
├── assets/
├── components/
└── package.json
```

### Purpose of each file

- `index.tsx`: Defines the home route `/`.
- `about.tsx`: Defines the `/about` route.
- `profile.tsx`: Defines the `/profile` route.
- `_layout.tsx`: Defines the navigation layout shared by routes in that directory.
- `components/`: Holds reusable UI components; it is not automatically a route directory.

---

## 5. Creating Screens

### Home screen — `app/index.tsx`

```tsx
import { View, Text } from 'react-native';

export default function HomeScreen() {
  return (
    <View>
      <Text>Home Screen</Text>
    </View>
  );
}
```

### About screen — `app/about.tsx`

```tsx
import { View, Text } from 'react-native';

export default function AboutScreen() {
  return (
    <View>
      <Text>About Screen</Text>
    </View>
  );
}
```

Each file exports a React component as its default export.

Expo Router discovers these files and registers their routes automatically.

---

## 6. Navigation Between Screens

Use `Link` to navigate declaratively or `router` for programmatic navigation.

### Method 1: Using Link

```tsx
import { View, Text } from 'react-native';
import { Link } from 'expo-router';

export default function HomeScreen() {
  return (
    <View>
      <Text>Home Screen</Text>

      <Link href="/about">Go to About</Link>
      <Link href="/profile">Go to Profile</Link>
    </View>
  );
}
```

Here, `href="/about"` navigates to the screen defined in `about.tsx`.

### Method 2: Using router.push()

```tsx
import { View, Button } from 'react-native';
import { router } from 'expo-router';

export default function HomeScreen() {
  return (
    <View>
      <Button
        title="Open About"
        onPress={() => router.push('/about')}
      />
    </View>
  );
}
```

### Difference

|API|Purpose|
|---|---|
|`Link`|Navigation through a link-like component|
|`router.push()`|Adds a screen to the navigation history|
|`router.replace()`|Replaces the current navigation entry|
|`router.back()`|Goes back to the previous screen when possible|

---

## 7. Understanding `_layout.tsx`

A layout defines how screens are presented and what navigation structure they share.

For example, use a Stack layout when screens should open one on top of another.

### `app/_layout.tsx`

```tsx
import { Stack } from 'expo-router';

export default function RootLayout() {
  return <Stack />;
}
```

This enables Stack navigation for the routes managed by this layout.

You can configure screen titles as well:

```tsx
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
        options={{ title: 'About Us' }}
      />
    </Stack>
  );
}
```

### Important

- `_layout.tsx` is a special file, not a normal screen route.
- Layouts can be nested.
- A layout can provide Stack, Tabs, or other supported navigation structures.

---

## 8. Nested Routes

Folders help organize related screens and create nested paths.

### File structure

```text
app/
├── index.tsx
└── settings/
    ├── _layout.tsx
    ├── index.tsx
    └── privacy.tsx
```

### Resulting routes

|File|Route|
|---|---|
|`app/index.tsx`|`/`|
|`app/settings/index.tsx`|`/settings`|
|`app/settings/privacy.tsx`|`/settings/privacy`|

The `settings/_layout.tsx` file defines the navigation structure shared by screens in the `settings` directory.

**Rule:** A folder groups routes, while `index.tsx` represents the folder's default route.

---

## 9. Dynamic Routes

Dynamic routes allow one screen file to handle multiple values, such as different user IDs or product IDs.

### File structure

```text
app/
├── index.tsx
└── users/
    └── [id].tsx
```

The file `[id].tsx` defines a dynamic route.

Examples:

- `/users/101`
- `/users/202`
- `/users/aarsh`

All these paths can use the same screen component.

### Example — `app/users/[id].tsx`

```tsx
import { View, Text } from 'react-native';
import { useLocalSearchParams } from 'expo-router';

export default function UserScreen() {
  const { id } = useLocalSearchParams<{ id: string }>();

  return (
    <View>
      <Text>User ID: {id}</Text>
    </View>
  );
}
```

When the route is `/users/101`, the screen displays `User ID: 101`.

### Navigating to a dynamic route

```tsx
import { Link } from 'expo-router';

<Link href="/users/101">View User 101</Link>
```

The ID can also be supplied as a parameter:

```tsx
<Link href={{ pathname: '/users/[id]', params: { id: '101' } }}>
  View User
</Link>
```

---

## 10. Route Groups

Parentheses create route groups that organize screens without adding that folder name to the URL.

### File structure

```text
app/
├── (auth)/
│   ├── login.tsx
│   └── signup.tsx
└── (tabs)/
    ├── _layout.tsx
    ├── index.tsx
    └── profile.tsx
```

The routes include:

- `app/(auth)/login.tsx` → `/login`
- `app/(auth)/signup.tsx` → `/signup`
- `app/(tabs)/index.tsx` → `/`
- `app/(tabs)/profile.tsx` → `/profile`

The group names `(auth)` and `(tabs)` are not included in the URLs.

Route groups are useful for separating authentication screens from the main application and organizing tab-based navigation.

---

## 11. Tab Navigation

Use `Tabs` when your app has a bottom navigation bar.

### `app/(tabs)/_layout.tsx`

```tsx
import { Tabs } from 'expo-router';

export default function TabLayout() {
  return (
    <Tabs>
      <Tabs.Screen
        name="index"
        options={{ title: 'Home' }}
      />
      <Tabs.Screen
        name="profile"
        options={{ title: 'Profile' }}
      />
    </Tabs>
  );
}
```

With `index.tsx` and `profile.tsx` in the same `(tabs)` directory, Expo Router displays them as tabs.

Use Stack navigation for hierarchical screen flows and Tabs for top-level destinations.

---

## 12. Query Parameters

Query parameters pass optional information through the URL without making each value a separate route.

For example:

```text
/products?category=books
```

Read the parameter using `useLocalSearchParams()`:

```tsx
import { Text, View } from 'react-native';
import { useLocalSearchParams } from 'expo-router';

export default function ProductsScreen() {
  const { category } = useLocalSearchParams<{
    category?: string;
  }>();

  return (
    <View>
      <Text>Category: {category ?? 'All'}</Text>
    </View>
  );
}
```

### Dynamic route vs query parameter

- Dynamic route: `/products/101` — identifies a specific product.
- Query parameter: `/products?category=books` — filters or modifies a listing.

---

## 13. Common Mistakes

1. **Missing default export:** Each screen file should export a screen component as its default export.
2. **Incorrect file placement:** Screen files must be inside the configured route directory.
3. **Incorrect navigation path:** Ensure the `href` matches the route structure.
4. **Using** `**_layout.tsx**` **as a screen:** It defines navigation layout, not a regular route.
5. **Placing reusable components in the route directory:** Put general UI components outside the route directory to avoid accidentally creating routes.
6. **Confusing route groups with URL segments:** Parenthesized group names do not appear in the URL.
7. **Assuming routes are access-controlled:** Hiding a screen or redirecting users is not a substitute for authorization checks in your backend.

---

## 14. Quick Revision

- **File-based routing:** Files and folders define routes automatically.
- **Expo Router:** File-based routing solution for Expo and React Native apps.
- `index.tsx`: Default route for a directory.
- `_layout.tsx`: Shared navigation structure.
- `Link`: Declarative navigation.
- `router.push()`: Navigate and add to history.
- `[id].tsx`: Dynamic route.
- `(auth)`: Route group without a URL segment.
- `Tabs`: Bottom tab navigation.
- `useLocalSearchParams()`: Reads route and query parameters.

## 15. Official Documentation

- [Expo Router Introduction](https://docs.expo.dev/router/introduction/)
- [Installation](https://docs.expo.dev/router/installation/)
- [Navigation](https://docs.expo.dev/router/basics/navigation/)
- [Layouts](https://docs.expo.dev/router/basics/layout/)
- [Dynamic Routes](https://docs.expo.dev/router/reference/url-parameters/)
