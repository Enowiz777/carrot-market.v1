
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