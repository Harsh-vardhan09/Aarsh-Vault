## 1. Route Groups

Route groups organize related routes without changing the URL path. In Expo Router, they are created using parentheses `()`.

**Example structure:**

```
app/
├── (auth)/
│   ├── login.tsx
│   └── signup.tsx
├── (tabs)/
│   ├── index.tsx
│   └── profile.tsx
└── _layout.tsx
```

**Key points:**

- Parentheses `()` define a route group.
    
- Group names do not appear in the URL.
    
- Useful for organizing authentication, tabs, and other sections.
    
- Groups can have their own `_layout.tsx` for separate navigation layouts.
    

**Example:**

- `app/(auth)/login.tsx` → `/login`
    
- `app/(tabs)/profile.tsx` → `/profile`
    

## 2. Nested Routes

Nested routes represent pages organized inside folders. The folder hierarchy determines the URL path.

**Example structure:**

```
app/
├── products/
│   ├── _layout.tsx
│   ├── index.tsx
│   └── details.tsx
└── _layout.tsx
```

**Routes:**

- `products/index.tsx` → `/products`
    
- `products/details.tsx` → `/products/details`
    

**Key points:**

- Folders represent URL segments.
    
- `index.tsx` represents the folder's default route.
    
- `_layout.tsx` defines shared navigation for child routes.
    
- Nested routes help organize complex applications.
    

## 3. Nested Dynamic Routes

Dynamic routes use square brackets `[]` to match variable URL segments.

```
app/
└── products/
    └── [id].tsx
```

Example URL: `/products/123`

Access the parameter using Expo Router:

```
import { useLocalSearchParams } from 'expo-router';

export default function ProductDetails() {
  const { id } = useLocalSearchParams<{ id: string }>();

  return <Text>Product ID: {id}</Text>;
}
```

## 4. Route Groups vs. Nested Routes

|Feature|Route Groups|Nested Routes|
|---|---|---|
|Syntax|`(group)`|`folder/file`|
|URL impact|Group name omitted|Folder names included|
|Main purpose|Organize routes and layouts|Create URL hierarchy|
|Example|`(auth)/login.tsx`|`products/details.tsx`|

## 5. Quick Revision

- `(group)` → Organizes routes without adding a URL segment.
    
- `folder/` → Creates a nested URL segment.
    
- `index.tsx` → Default route for a directory.
    
- `_layout.tsx` → Shared navigation or layout configuration.
    
- `[id].tsx` → Dynamic route parameter.
    
- Route groups and nested routes can be combined.
    

**Remember:** Route groups organize navigation; nested routes define the URL hierarchy.