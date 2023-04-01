
prisma, planetscale, React 18 is coming
server-side streaming

# 2.1 Requirement

- Take NextJS starter
- 

# 2.2 Recording Plan

- We are going to build UI on Camera.
- Tailwind modifiers help open your mind.
	- people think that CSS framework limit your capability where in reality it doesn't.
- We are going to build with the fake data. 
- 

# 3.0 NextJS Setup

- We are going to decide whether you want to use typescript or not.
- RC - release candidate until everything is validated.
- Install nextjs
- Set up tailwind css
```
npx install -D tailwind postcss autoprefixer
```

```
Tailwind CSS설치 및 초기화

npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p (-p를 붙이면 postcss.config.js파일까지 생성)
```

- The tailwind data will show
- postcss.config and tailwind.config.js gets created.
- Tell tailwind where we are going to use tailwind in tailwind.config.js
tailwind.config.js
- Modify the content so that any file inside the page and components directory will apply tailwind.
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}

```

- Global.css that is already created modified to below.

```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## 4.0 Tailwind

- Pay attention to tailwind style that they write on the class. 

- This is just about adding and combining classname.
- grid: add display grid.
- aspect-square: add square.
- bg-background color. 
- Shadow: it is hard to make. Instead, you tell the tailwind to make the shadow really well. 
- When you use other framework, they tend to use certain styles. 
- There are classname everywhere for Tailwind css and they are not enforcing certain styles. 
- We are going to make dark mode later. 
- Images. Talking about the World-class IDE. 
- TailwindCSS has many classname and is hard to remember. If you want to get an autocomplete, you just need to install Tailwind CSS intellisense.
- 

# 4.1 Test pt.1

- When you first start with TailwindCSS, you have to go back and forth with the documentation.
- The more you use tailwind, the easier it gets. 
- padding, rem (responsive design), 
- CTRL-SPACE if autocomplete doesn't show.
- flex flex-col space-y-5: flex, flex column give child margin top and bottom 5pixels.

# 4.2 Test pt.2

- overflow-hidden
- Add padding bottom to make the card larger. 
- relative: position relative - top-14 
- 

# 4.4 Modifier

- If you want to change the checkout button, you can make it to a button. 
- you can use :hover using the modifier. 
- Modifier is something that we write
```
// when hover over, the background changes to teal.
hover:bg-teal-500
```
- There are hover:, active:, and other modifiers available. Please refer to the documentation.
- 

# 4.5 Transition

- There are other modifiers. 
- ring-offset-2 (create some space between the border and the item)
- put your mouse over; setting css variable. 
- if you put the word transition, it will make the smooth transition.
- 

# 5.0 Tailwind practice

- If you get comfortable with TailwindCSS, you will be efficient as a developer. 
- Nico recommends investing time on TailwindCSS
- 

- Designing enter page
- Notable concepts:
  - border-b w-full - border bot width full
  - 

# 5.2 Auth part

- Tailwind plugins - plug that you can use to create the basic style.
- Install the core plugins called form. 
- plug-in add extra functions in the configurations.
- ex: typography - you can use prose.
- you can use the @tailwindcss/forms.
```
npm i @tailwindcss/forms
```
add to tailwind.config.js

```
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [require("@tailwindcss/forms")],
}

```
- use relative on the parent and absolute on the child. 
- 

# 5.3 Home Screen

- Front page will be a list of the product.
- mapping the array

```js
      {[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1].map((_, i) => (<div></div>)
```

- heroicons is the SVG icons.
- They are useful.
- 

# 5.4 Item Detail page

- Create a profile.
- We render similar items.
- Start with the constraint and padding. 
- px, py, h, bg(background), flex items-center space-x-3, 
- w-12, h-12, border-t (boder top), border-b(border bot)

# 5.5 Upload Item

- Upload screen is what they will see when they click on the plus button. 
- The technic that everyone uses for the file choice. 
- Tailwind used: px, py, w-full (width full), 
- input: hidden; icon in the center, you hide the input with css, the moment you click it will appear on the screen. 
- htmlFor="input"
- relative - absolute to set the dollar sign inside the input. 
- block
- ring-orange-500: set the color of the ring
- focus ring-2: create a ring with the boldness 2 on the focus

# 5.6 Community

- space-y-8
- flex space-x-
- iterate an array Array.map()

# 5.7 Community Detail

- You can create the community you can have two pages. Index with ID. 
- Index - /community
- /community/detailID - route to the detailed pages. 
- For the code that repeats, you can make them into a component. 

# 5.8 Chats screen

- divide-y-2: line gets thicker; add a border-top to the item that has next to others. 
- 

# 5.9 Chats Detail page.

- chats to chats/index.tsx
- TailwindCSS used
- flex items-center space-x-2.
- w-1/2 text-sm text-gray-700 p-2 rounded-md.
- space-x-reverse: add space in reverse.
- input - fixed - bottom of the screen: 
  - fixed w-full mx-auto max-w-md bottom-0 inset-x-0
- create a chat input
  - focuse:outline-orange-500; 

# 5.10 Profile screen

- TailwindCSS used
- mt-12, flex item-center
- w-12 h-12 rounded-full 

# 5.11 Bought, Loved and Sold.

- rename the profile page to index.tsx
- Add Loved, bought, and sold page in the profile folder.

# 5.12 Edit Profile

- Edit profile will contain ways to edit the profile, which are many inputs. 
- TailwindCSS: take all the space w-full.
- input type file looks really ugly so you can hide completely and modify the label
<label className="cursor-pointer">
- change input to take only img file. 

# 5.13 Streams

- Aspect-video: you can the aspect ratio of the video 16:9. When we creat the circle, we can use the aspect square.
- Create a button that can go live. 
- 