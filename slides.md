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

## Exercise
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
      tabBarIcon: () => (
         return <Feather name="list" size={24} color="black" />
      )
   }
   ```
- What do you notice about the active tab colours now? 
- Hover over tabBarIcon to see what other props it can take.  Change accordingly:
   ```tsx
   tabBarIcon: ({ color, size }) => ...?
   ```

---
transition: slide-left
---

# Nested navigation (pg.1)

- You can mix/match navigations (ex: stack within a tab, stack within a modal etc.)
- If we want to refactor `counter.tsx` into a stack:
   - create new folder `./app/counter`
   - move `counter.tsx` in `/counter`
   - rename it to index.tsx
   - add `/counter/_layout.tsx` and in it:
   ```tsx
   import { Stack } from "expo-router";
   export default function Layout() {
      return (
         <Stack>
            <Stack.Screen name="index" options={{ title: "Counter" }}/>
         </Stack>
      )
   }
   ```
   - How many headers of "Counter" do you see now?  Let's hide one of them.  In `./app/_layout.tsx`
   ```tsx
   <Tabs.Screen ...  options={{...  headerShown: false, ... }}
   ```

## Exercise:
- Add an icon for the Counter tab, and the Idea tab
- Change default tab color via `<Tabs screenOptions={{ tabBarActiveTintColor: ? }}>`

---
transition: slide-left
---

# Nested navigation (pg.2)

- Create new page `/counter/history.tsx`; just copy/paste from idea.tsx but change to `<Text...>History</Text>`
- To begin navigating to our Counter page, our `/counter/_layout.tsx`
   ```tsx
   <Stack.Screen ... headerRight: () => <Text>Hello</Text>;
   ```
- Find an appropriate "history" [icon](https://icons.expo.fyi/) 
- Replace our Text component with:
   ```tsx
   <Link href="/counter/history" asChild>
      <Pressable hitSlop={20}>
         <MaterialIcons name="history" size={32} color={theme.colorGrey}>
      </Pressable>
   </Link>
   ``` 
- try clicking your history icon, does it navigate appropriately to the history page?
- What do you think `hitSlop` does?  fyi - `asChild` works with `hitSlop`

---
transition: slide-left
---

# Input (pg.1)

- fyi - There is no `<form>` component in React Native; must handle all inputs individually
- in `/app/index.tsx` add `<TextInput>`:
   ```tsx
   <View style={stlyes.container}>
      <TextInput placeholder="apples" />
   ``` 
   - do you see anything? then try adding borderColor, borderWidth, padding, marginHorizontal, marginBottom, fontSize, borderRadius etc.
   - remove `justifyContent` within `container` styles to move everything to top of screen
   - add `paddingTop: 12`
- on your Expo Go, if you tap on the input now, does the keyboard widget open? (if using simulator, may have to toggle keyboard to open via I/O > Keyboard > Toggle Software Keyboard;  Now try.)

---
transition: slide-left
---

# Input (pg.2)

- Let's add some state to keep track of what is typed in our `/counter/index.tsx`
   ```tsx
   import { useState } from "react";
   ...
   const [value, setValue] = useState("");
   ...
   <TextInput ... value={value} onChangeText={setValue}>
   ```
- Try typing in the input now, does it type accordingly?
- What's the difference between onChange and onChangeText?

## Exercise 
- see [TextInput docs](https://reactnative.dev/docs/textinput#keyboardtype)
- add `<TextInput ... keyboardType="phone-pad">` (see keyboard now)
- see [TextInput docs](https://reactnative.dev/docs/textinput) - get to know some of the interesting props on the right side (ex: autoComplete etc)
- try adding prop `<TextInput ... returnKeyType="done">` (see keyboard return key text)

---
transition: slide-left
---

# Input (pg.3)

- to submit, use `onSubmitEditing` callback
   ```tsx
   <TextInput ... onSubmitEditing={() => console.log("submit")}
   ```
   - click "Done" button -- do you see the log in the terminal?
- Let's add some data in our `/counter/index.tsx`
   ```tsx
   type ShoppingListItemType = {
      id: string;
      name: string;
   };
   const initialList: ShoppingListItemType[] = [
      {id: "1", name: "apples"},
      {id: "1", name: "bananas"},
      {id: "1", name: "carrots"},
   ]
   ...
   const [shoppingList, setShoppingList] = useState<ShoppingListItemType>(initialList)
   ...
   {shoppingList.map(item => (
      <ShoppingListItem name={item.name} key={item.id} />
   ))}
   ```
   
---
transition: slide-left
---

# Input (pg.4)

```tsx
   const handleSubmit = () => {
      if (value) {
         const newShoppingList = [
            { id: new Date().toISOString(), name: value },
            ...shoppingList
         ]

         setShoppingList(newShoppingList);
         setValue("")
      }
   }
   ...
   onSubmitEditing={handleSubmit}
```
- try typing some items to the list - does it work?



---
layout: image-right
transition: slide-left
image: /assets/harvard.png
backgroundSize: 444px 380px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 🏫 [Harvard's CS50 React Native](https://pll.harvard.edu/course/cs50s-mobile-app-development-react-native)
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

# Scrolling

- Try adding lots of items so that it "should be" scrollable -- how does this differ from the web?
- To make it scrollable, we need to replace `<View>` with `<ScrollView>`
   ```tsx
   import { ..., ScrollView } from 'react-native';
   ...
   <ScrollView style={styles.container}>
   ```
   - does it scroll now?
- try commenting out our `paddingTop` from our StyleSheet.create - does scrolling work now?
- try adding a new props:
   ```tsx
   <ScrollView ... contentContainerStyle={styles.contentContainer} stickyheaderIndices={[0]}>
   ...
   content: { padding: 12 } // remove previous paddingTop 
   ...
   contentContainer: { paddingBottom: 24, } 
   ```
    - try scrolling now
      - add textInput style `backgroundColor: theme.colorWhite` so that header is not transparent
---
transition: slide-left
---

# ScrollView vs FlatList

| Feature / Criteria                        | `ScrollView`                                            | `FlatList`                                                               |
| ----------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Best For**                              | Small lists with a limited number of items              | Large or dynamic lists with many items                                   |
| **Performance**                           | ❌ Poor with large data sets – renders all items at once | ✅ Optimized for performance – renders only what's visible (windowing)    |
| **Ease of Use**                           | ✅ Simple to implement for short static lists            | ⚠️ Slightly more complex due to item rendering and keys                  |
| **Lazy Loading / Virtualization**         | ❌ Not supported                                         | ✅ Built-in support (virtualization, windowing, etc.)                     |
| **Support for Pull to Refresh**           | ❌ Requires manual setup                                 | ✅ Built-in via `onRefresh` and `refreshing` props                        |



---
transition: slide-left
---

# FlatList (pg.1)
a more optimized way of rendering large lists of items

- there is no equivalent of FlatList on the web
- we'll need to replace our `<ScrollView>` with `<FlatList>`
```tsx
import { ... FlatList } from 'react-native';
...
<FlatList
   data={shoppingList}
   contentContainerStyle={styles.contentContainer}
   stickyHeaderIndices={[0]}
   renderItem={({item}) => <ShoppingListItem name={item.name} />}
   ListHeaderComponent={<TextInput placeholder="apples" />}
/>
```

---
transition: slide-left
---

# FlatList (pg.2)

- we can even specify a component if list is empty (avoids `if (shoppingList.length === 0)`)
   ```tsx
   <FlatList ...
      ListEmptyComponent={
         <View styles={styles.listEmptyContainer}>
            <Text>Your list is empty</Text>
         </View>
      }
   ```
   - may have to add styles: justifyContent, alignItems, marginVertical
   - can now get rid of our initialList; thus initialize our `setInitialList([])`
   - Does empty component now show?
---
transition: slide-left
---

# Homework

- Think about your Capstone Planning Project
   - can submit before due date if you wish