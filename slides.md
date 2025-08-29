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

- [ ] Expo Router, Navigation
- [ ] Modal, Button Tabs, Nested Navigation
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

- Where we last left off https://github.com/avcoder/mobile-react-native-01

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

```md
my-expo-app/
├── app/                     # All routes go here (like pages/)
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

- in package.json, change to `"main": "expo-router/entry"`
- create new folder `./app` and move `App.tsx` into it (let VS Code update imports)
- rename `App.tsx` to `index.tsx`


---
layout: image-right
transition: slide-left
image: /assets/rn.png
backgroundSize: 444px 380px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 👩‍💻 [React Native on FCC](https://www.freecodecamp.org/news/build-a-meditation-app-with-react-native-expo-router/)
- 📄 [UI: React Native Paper](https://reactnativepaper.com/)
- ⚛️ [UI: React Native Elements](https://reactnativeelements.com/)
- 🧭 [React Navigation](https://reactnavigation.org/)


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