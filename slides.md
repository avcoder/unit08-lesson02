---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# React Native
Mobile Development: Unit 08 - Lesson 02

- [ ] Expo Router
- [ ] Various Navigations
- [ ] Input, Scrolling, Lists

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---
transition: slide-left
---

# Recap

- Where we last left off 
- `git clone https://github.com/avcoder/mobile-react-native-01`
- `cd mobile-react-native-01`
- `npm i`
- `npm start`
- scan QR code with phone via Camera/Expo Go, or press appropriate key to emulate on phone or web

---
transition: slide-left
---

# Expo Router

- Since React Native (RN) does not come with a navigation library, we will install [Expo Router](https://docs.expo.dev/router/introduction/)
- Expo Router is a file based nav library (just like Express)
- Expo Router is built on top of [React Navigation library](https://reactnavigation.org/) (good to know in case you need to look up docs)
- We have a few different types of navigation which we'll explore:
   - Stack
   - Modal
   - Tabs
- The `_layout` file instructs how the other .tsx files (i.e. screen files) are laid out
   - you can have one `_layout` file per folder

---
transition: slide-left
---

# Example Folder Structure for Expo Router
main entry point has to be index.tsx

```md
├── app/                    # All routes go here (like pages/)
│   ├── index.tsx           # Route: "/"
│   ├── about.tsx           # Route: "/about"
│   ├── settings/           # Nested route group
│   │   ├── index.tsx       # Route: "/settings"
│   │   └── profile.tsx     # Route: "/settings/profile"
│   ├── (auth)/             # Route group for auth flows (not part of URL)
│   │   ├── login.tsx       # Route: "/login"
│   │   └── register.tsx    # Route: "/register"
│   ├── _layout.tsx         # Shared layout for all routes
│
├── components/             # Reusable UI components
│   └── Header.tsx
│
├── constants/              # Constants like colors, layout, sizes
│   └── Colors.ts
│
├── assets/                 # Images, fonts, icons, etc.
│   └── images/
```

---
transition: slide-left
---

# Exercise: Installing Expo Router (pg.1)

- `npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar`
   - `expo-router`: Handles navigation using a file-based routing system, similar to Express.js
   - `react-native-safe-area-context`: Helps manage safe areas on iPhones (like notches, status bars, etc)
   - `react-native-screens`: Optimizes memory usage and performance of navigation
   - `expo-linking`: Handles deep linking and universal linking (e.g., opening your app to a specific screen from a link))
   - `expo-constants`: Provides system-level constants like the app’s version, device info, and environment variables.
   - `expo-status-bar`: Provides a unified way to control the status bar appearance across iOS, Android, and Web.

---
transition: slide-left
---

# Exercise: Using Expo Router (pg.2)

- in `package.json`, change to `"main": "expo-router/entry"`
- in `app.json`, under "expo", insert new key/value of  `"scheme": noteapp"`
   - required for deep linking;  Other apps on phone will only respond if url request matches their name of scheme
- create new folder `./app` and move `App.tsx` into it (let VS Code update imports)
- rename `App.tsx` to `index.tsx`
- may need to `npm start` again 
- create file `./app/_layout.tsx` where we'll define each of the screens that matches our filename
   ```tsx
   import { Stack } from "expo-router";
   export default function Layout() {
      return (
         <Stack>
            <Stack.Screen name="index" options={ title: "Note App List"}> 
         </Stack>
      )
   }
   ```

---
transition: slide-left
---

# Stack Navigation

- by default,a stack data structure is used; new screens are rendered on top of current screen
- create a couple of screens: add `./app/counter.tsx` and `./app/idea.tsx`
   ```tsx
   import { Text, View, StyleSheet } from "react-native";

   export default function CounterScreen() { /* add function IdeaScreen() and change text below to Idea */
      return (
         <View style={styles.container}>
            <Text style={styles.text}>Counter</Text> 
         </View>
      )
   }

   const styles = StyleSheet.create({
      container: {
         flex: 1,
         justifyContent: "center",
         alignItems: "center",
         backgroundColor: "#ffffff"
      },
      text: { fontSize: 24, }
   });
   ```

---
transition: slide-left
---

# 1. Navigate Screens in Expo Router using `<Link>`

```tsx
import { Link } from "expo-router";
...
<View...>
   <Link href="/counter">
      Goto /counter
   </Link>
```

- Does tapping the corresponding link navigate appropriately?
- Does swiping between screens work too?

---
transition: slide-left
---

# 2. Navigate Screens in Expo Router programmatically 

- in counter.tsx, add TouchableOpacity
   ```tsx
   import { useRouter } from "expo-router";
   export default function CounterScreen() {
      const router = useRouter();
      return (
         <View>
            <TouchableOpacity onPress={() => router.navigate("/idea")}>
               <Text>Go to /idea</Text>
            </TouchableOpacity>
   ```
- note that we had a choice between `router.push`, `router.navigate`, `router.replace` -- wt diff?

---
transition: slide-left
---

# 3. Modal Navigation

- must define Modals above/adjacent to other screens in order to work
- note in our `_layout.tsx` we still only defined one screen
   - Can define more screens if needed (ex: if we have different properties)
- let's add more screens, in _layout.tsx
   ```tsx
   <Stack>
      <Stack.Screen name="index" options={{ title: "List" }} />
      <Stack.Screen name="counter" options={{ title: "Counter", presentation: "modal", animation: "fade" }} />
      <Stack.Screen name="idea" options={{ title: "Idea"}} />
   </Stack>
   ```
- try adding `presentation: "modal"` and animations to other screens

---
transition: slide-left
---

# 4. Tabbed Navigation (pg.1)

- to use Tabbed Navigation, make the following changes in _layout.tsx:
   ```tsx
   import { Tabs } from "expo-router";
   ...
   <Tabs>
      <Tabs.Screen name="index" options={{ title: "List" }} />
      <Tabs.Screen name="counter" options={{ title: "Counter" }} />
      <Tabs.Screen name="idea" options={{ title: "Idea"}} />
   </Tabs>
   ```
   - Try it out.  Do our previous navigation method still work too?
   - Let's clean our previous navigation methods: Remove our `<TouchableOpacity>` our `<Link>`

---
transition: slide-left
---

# Use Icons for Tabs (pg.2)

- Use icons for tabs: [@expo/vector-icons](https://icons.expo.fyi/) 
   - add to new options prop to Tabs.Screen:
   ```tsx
   options={
      ...
      tabBarIcon: () => {
         return <Feather name="list" size={24} color="black" />
      }
   }
   ```

---
layout: image-right
transition: slide-left
image: /assets/rn.png
backgroundSize: 444px 380px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 🎨 [SVG tool](https://www.fffuel.co/sssvg/)
- 🖍️ [Design Resources](https://www.toools.design/)
- 🏂 [client-side db](https://www.instantdb.com/)
- ⚛️ [Numberflow Component](https://number-flow.barvian.me/)
- 🦹‍♀️ [Better Auth](https://www.better-auth.com/)


<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)


---
transition: slide-left
---

# Homework

- Think about your Capstone Planning Project
   - can submit before due date if you wish