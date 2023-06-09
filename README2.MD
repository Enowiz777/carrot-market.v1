# Carrot Market

# Requirements:

1. NextJS
- NextJS: Apps React, React-DOM, NextJS
*How do you install NextJS?*
```js
// for typescript
npx create-next-app@latest --typescript
```

2. TailwindCSS

*How do you install?*
Steps:
a. Install tailwindcss, postcss, autoprefixer.
```
npm install -D tailwindcss postcss autoprefixer
```

b. Initiate tailwindcss
```bash
npx tailwindcss init -p
: << 'COMMENT'
Created Tailwind CSS config file: tailwind.config.js
Created PostCSS config file: postcss.config.js
COMMENT
>>
```

c. Modify tailwind.config.js to match our configurations.

tailwind.config.js
```js
// Inside any folders of page/ and component/ you will use tailwindcss
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};

```

d. Change the styles/globals/css to import tailwindCSS

```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

e. Test it by running below
```bash
npm run dev
```

# Dev journal

20220805
- Added @tailwindcss/forms
```
npm i @tailwindcss/forms
```

- Modify tailwind.config.js to use it
```js
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  darkMode: "media", // class
  plugins: [require("@tailwindcss/forms")],
};
```

# 1. Create pages with TailwindCSS

## a. Create unique pages
- If you were to create item pages url, you just need to create a bracket around. ([id].tsx)


## b. Build upload screen
- You can just add below path and it will create the path in NextJS.
Path: pages/items/upload.tsx
- File choosing technique


## c. Create Community page
pages/community/index
- People will go into community and see IDs. 

## d. Chats page
divide-y-2 to create a border below

## e. Chat Detail
- Create a detail page in the chat by doing pages/chat/[id].tsx
- TailwindCSS used:
- w-6
- h-6
- space-x-reverse (reverse the position)
- bottom = 0 - put div in the bottom.
- absolute - container father needs to be relative.

## f. Profile page
- Create sub pages of
bought, loved, sold

## g. Create live screen
- People will see the current livestream. 

## h. Create live
- People watch videos.

## i. Finish creating a live stream.
- You create livestream detail ver similar to the product. 

## j. Layout fix

- If the screen gets too big, it becomes too ugly - extended or stretched all the way.
- Can be controlled with the app.tsx
- You can create another div surround the curren prop and you can set
```ts
// Set max width
// 
w-full max-w-xl mx-auto
```

- Create title and navigation bar.
- Create a folder called Component/layout
components/layout:
```ts
import React from "react";
import { cls } from "../libs/utils";

interface LayoutProps {
  title?: string;
  canGoBack?: boolean;
  hasTabBar?: boolean;
  children: React.ReactNode;
}

export default function Layout({
  title,
  canGoBack,
  hasTabBar,
  children,
}: LayoutProps) {
  return (
    <div>
      <div className="bg-white w-full text-lg font-medium py-3 fixed text-gray-800 border-b top-0 justify-center flex items-center">
        {title ? <span>{title}</span> : null}
      </div>
      <div className={cls("pt-16", hasTabBar ? "pb-16" : "")}>{children}</div>
      {hasTabBar ? (
        <nav className="bg-white text-gray-800 border-t fixed bottom-0 pb-10 pt-3 flex justify-between items-center"></nav>
      ) : null}
    </div>
  );
}
```

# 2. Back-End

# 2.1. Database

- Set up database with PlanetScale
*What is Prisma?*

```
## Prisma
nodejs, typescript의 ORM
ORM은 typescript와 데이터베이스 사이의 다리 역할
우선 schema.prisma에 데이터의 모양을 알려주어야 함
Prisma가 타입을 알고 있으면 client를 생성해줌
client를 통해 타입스크립트로 데이터베이스와 직접 상호작용
Prisma studio (Visual Database Browser): Admin 패널같이 데이터를 관리할 수 있음
```
- Prisma is a Node.js and TypeScript ORM (Object Relational Mapping)
- Translator or bridge between the Typescript code and database code. 
- We do not want to write DB code but you want to write the TypeScript code and let ORM translate into the DB language. 
- Schema prisma - explains the shape of your data with types and names of data. 
- It generates client for you. It hsa all the names you need. 
Ex: Model named user
- There will be prisma.users.create() that will automatically generate users. 
- There are many autocomplete that you can utilize.
- Prisma Studio: Visual Database Browser that create Admin Panel for your database. 
- It works for PostgreSQL, MySQL, SQL Server, SQLite,MongoDB.
- PlanetScale is mySQL operable database. 

# 2.1 Prisma Setup
Steps:
1.  Install VSCODE prisma extension
https://marketplace.visualstudio.com/items?itemName=Prisma.prisma

2. Install Prisma with developer dependencies.
```js
npm i prisma -D
// install prisma with developer dependencies.
```
*Need to have nodejs version higher than 14.17*

3. Invoke prisma using npx
```
npx prisma
```

```
npx prisma init
```
Results:
- It created prisma folder inside .env.
- It created .env folder inside our application.

Instructions:
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2. Set the provider of the datasource block in schema.prisma to match your database: postgresql, mysql, sqlite, sqlserver, mongodb or cockroachdb.
3. Run prisma db pull to turn your database schema into a Prisma schema.
4. Run prisma generate to generate the Prisma Client. You can then start querying your database.

We can do step 2 by going into the prisma folder. 
prisma/shema.prisma:
```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql" -> "mysql"
  url      = env("DATABASE_URL")
}

```

Create first model
prisma/shema.prisma:
```js
model User {
  id Int @id 
  // @id: this is an id of a model. 
  phone Int? 
  // ?: make it optional
  // @unique: make it unique.
  email String? @ unique
  name String
  avatar String?
  createdAt DateTime @default(now())
  updatedAt DataTime @updatedAt

}
```
- Prisma will read this file and generate typescript client and it knows how the user look like. 
- Documentations will be there on the prisma website. 

# 2.2 PlanetScale

```
## PlanetScale

mysql과 호환(Compatible)되는 serverless 데이터베이스 플랫폼

scaling을 자동으로 해줌 ( + no vacumming, no rebalencing, no query planning, no downtime)

Vitess 오픈소스를 통해 MySQL Scaling

CLI를 통해 쉽게 데이터베이스를 다룰 수 있음

마치 Git처럼 메인 데이터베이스 이외에 Branch 데이터베이스를 사용할 수도 있음

이후 Merge를 하면 자동으로 배포가 됨
```

*What is Planet Scale?*
- The MySQL-compatible serverless database platform. 
- Serverless: We do not have to maintain, secure, update for the server. 
- This is not like AWS RDS. There you have to create a server, right size and do everyting on your own. IF there are many people connecting to the DB you have to scale yourself. 
- Serverless. It is something different. 
- there are two lines of code. 
- YOur database never goes down. 
- Vitess platform: most scalable open source databse out there. 
- It is created by Google to scale YouTube.com.
- Scaling is the hardest part. 
- One server can do a lot but when it takes millions, the challenge starts to happen. 
- Horizontal, sharding and maintain downtime and others. 
- Vitess is used by Many people like Affirm, Airb&B and others. 
- It is a database clustering system that easily scale mySQL. 
  - Horizontal Scale: Transparent horizontal sharding.
  - High Availability: Vitess's primary replicat configuration allows for seamless failoever from a primary to replica.
  - MySQL Compatible: Migrate your existing MySQL applications with minimal changes while
  - Online Schema Migrations: 
  - Free plan gives 10GB 100 million read/ 10 million write/month
  1000 connections simluatanoues
  - Automatically daily backups.
  - Community Suppor.t 

- They have nice CLI as if we are interacting with git. If you are adding a new model to your db, instead of modifying the main db, they make the branch of your db, you can modify in that db. 
- Once you sure that that works, you can migrate or merge the schema. 
- Combine with Prisma, you will notice how easy it is to build. 
- DX is incredible and it maintains DB for you. 
- Click signin and create an account > login with github. 

## 2.3 Connect to PlanetScale

- Leverage CLI to install PlanetScale. 
*How do you install PlanetScale CLI?*
Ref: https://github.com/planetscale/cli

Step 1: Install Scoop
Windows Installation:
혹시 저처럼 윈도우10 환경에서 설치를 헤매고 계시다면 scoop 깃헙보고 따라하시면 됩니다.
1. Powershell 관리자로 실행
2. https://github.com/ScoopInstaller/Install#readme
3. 위 주소에서 Prerequisites, For Admin 따라하시면 됩니다!

Step 2: Install planetscale CLI

Windows
pscale is available via scoop, and as a downloadable binary from the releases page:
```
scoop bucket add pscale https://github.com/planetscale/scoop-bucket.git
scoop install pscale mysql
```
To upgrade to the latest version:

scoop update pscale

Step 3: Confirm installation

- If everything is installed correctly, you can run the below script.

```js
pscale
/*
Output:
pscale is a CLI library for communicating with PlanetScale's API.

Usage:
  pscale [command]

Available Commands:
  audit-log      List audit logs
  auth           Login and logout via the PlanetScale API
  backup         Create, list, show, and delete branch backups
  branch         Create, delete, diff, and manage branches
  completion     Generate completion script for your shell
  connect        Create a secure connection to a database and branch for a local client
  data-imports   Create, list, and delete branch data imports
  database       Create, read, delete, and dump/restore databases
  deploy-request Create, review, diff, revert, and manage deploy requests
  help           Help about any command
  org            List, show, and switch organizations
  password       Create, list, and delete branch passwords
  region         List regions
  service-token  Create, list, and manage access for service tokens
  shell          Open a MySQL shell instance to a database and branch
  signup         Signup for a new PlanetScale account

Flags:
      --api-token string          The API token to use for authenticating against the PlanetScale API.
      --api-url string            The base URL for the PlanetScale API. (default "https://api.planetscale.com/")
      --config string             Config file (default is $HOME/.config/planetscale/pscale.yml) 
      --debug                     Enable debug mode
  -f, --format string             Show output in a specific format. Possible values: [human, json, csv] (default "human")
  -h, --help                      help for pscale
      --no-color                  Disable color output
      --service-token string      Service Token for authenticating.
      --service-token-id string   The Service Token ID for authenticating.
      --version                   Show pscale version

Use "pscale [command] --help" for more information about a command.
*/
```

- Navigate CLI

- Login to Planetscale CLI
```
pscale auth login
- confirm login
```

- View region list
```powershell
pscale region list
<#
NAME (9)                            SLUG                 ENABLED
 ----------------------------------- -------------------- ---------
  AWS us-east-1 (Northern Virginia)   us-east              Yes
  AWS us-west-2 (Oregon)              us-west              Yes
  AWS eu-west-1 (Dublin)              eu-west              Yes
  AWS ap-south-1 (Mumbai)             ap-south             Yes
  AWS ap-southeast-1 (Singapore)      ap-southeast         Yes
  AWS ap-northeast-1 (Tokyo)          ap-northeast         Yes
  AWS eu-central-1 (Frankfurt)        eu-central           Yes
  AWS ap-southeast-2 (Sydney)         aws-ap-southeast-2   Yes
  AWS sa-east-1 (Sao Paulo)           aws-sa-east-1        Yes
#>

```

- Let's create it in the us-west-2 because it is the closest location from where I am at. 

Type "pscale database" to view options

- Create database in a us-west-2 region.
```powershell
pscale database create carrot-market --region us-west
# Database carrot-market was successfully created.
```

- Modify schema.prisma > .env to point to the correct database.
- Again, people never touch the real database in a computer. They have two database. They work on the fake database and update using AWS or heroku. 
- Instead, you can have a secure tunnel between the scale and your computer. 
- You don't have to write anything to .env

```
DATABASE_URL=""
```

- Make a connection to the carrot-market
```powershell
pscale connect carrot-market
<#
Secure connection to database carrot-market and branch main is established!.

Local address to connect your application: 127.0.0.1:60762 (press ctrl-c to quit)

# connected with the planetscale server.
#>
```

- Paste the Database URL to .env
  - Copy the url and paste as below
.env:
```ps1
DATABASE_URL="mysql://127.0.0.1:60762/carrot-market"
# Format: <db type>://<url>/<name of the db>
```

## 2.4 Push to PlanetScale

- Prisma looks at shcema.prisma for two things:
1. Take model and push to your database and perform SQL migration.
2. Create a client so that you can talk to the DB. 
3. Add types and autocomplete to the client. 

- Need to add two lines of code before you push. 
  - planetscale is mySQL compatible. 
  - There are couple of things that they do differently. 

- Vitess takes a lot of data > There are couple of thing sthat mySQL does that Vitess doesn't. 
1. Foreign key constraint. 
*What is FKC?*
- If we were to create a comment for Users DB
It will get to the User DB with UID and then add in the comment DB. 
- In normal mySQL and postSQL, if you want to create a forign key the forieng key here exist. 

- Vitess focuse on scaling and breaking down and storing. They do not check if the user exist before the comment is added. 

- We would like to get some help, if the db is not oging to help us, prisma will help us notify whether the user exist or not. 

schema.prisma:
```js
generator client {
  provider = "prisma-client-js"
  previewFeatures = ["referentialIntegrity"]
}
```
- Now we are ready to push schema.prism ato planetScale. 
*How do you push to planetscale?*
```ps1
npx prisma db push
<#
Output:
✔ Generated Prisma Client (4.2.1 | library) to .\node_modules\@prisma\client in 68ms
#>
```

- You can confirm whether the schema has been pushed or not from the planetscale website.
PlanetScale > open DB > Branches tab > click main branch > click schema tab.

## 2.5 Prisma Client

Admin Panel:
```
npx prisma studio
```
- You click user, and it is ready to receive users and its properties. 
- With admin panel, you can create your first user. (input data inside.)
- 

*How do you use primsa client?*


- Initialize client
Steps:
1. install @prisma/client
```
npm i @prisma/client
```
2. Import @prisma/client from lib/client.ts
libs/create client.ts:
```
import { PrismaClient } from "@prisma/client";

// export default 
const client = new PrismaClient();

client.user.create({
  data: {
    email: "hi",
    name: "hi",
  },
});
```

3. Generate a client
```ps1
npx prisma generate
# ✔ Generated Prisma Client (4.2.1 | library) to .\node_modules\@prisma\client 
```
- you can confirm this info by going into the path that it is generated inside the node_modules.

\node_modules\@prisma\client
```
/**
 * Model User
 * 
 */
export type User = {
  id: number
  phone: number | null
  email: string | null
  name: string
  avatar: string | null
  createdAt: Date
  updatedAt: Date
}

```
- Prisma looks at our schema and it generated a client. 

In conclusion:
Prisma is helping with three things:
1. Prisma modifies our db implementation.
2. Provide an Admin panel
3. Creates TypeScript type for client for auto-completion.

You can modify the user by using the auto-complete. 
ex:
```
client.user.create({data: {
  email: 13
  ......
}})
```

- We shouldn't be able to run the prisma client form a browser. 
- IT ONLY RUN SERVER. 

## 2.6 API Routes.


**Since I can't run the prisma client on the browser, how can I run it?**

- Create front end - ReactJS
- Backend - NodeJS, GraphQL


- Next JS doesn't need to have another server to make your own API. 
- NextJS can make front end

- Create new folder called api in the page. 

*20220810- I notice that the folder is already there.*
client-test.tsx
```ts
import { NextApiRequest, NextApiResponse } from "next";
import client from "../../libs/client";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  await client.user.create({
    data: {
      email: "hi",
      name: "hi",
    },
  });
  res.json({
    ok: true,
  });
}
```

- If you just put in the localhost:3000/api/client-test
- It will just start the API. 
- This is like a 
- API route is where we are going to talk to the DB. 
- 

Summary:
- Use Prisma, PlanetScale, API routes. 
- Recap
1. Create schema.prisma: configure prisma and explain prisma how our data is going to look like. 
2. npm prisma db push - push db schema to database. 
3. we have to create a secure tunnel and we put the url into the DATABASE_URL inside .env file. 
- PUt the secure connection to .env file.
4. prisma db push: explanation to in prisma and prisma is going to put them in PlanetScale. 
5. Prisma offers super good admin panel. 
6. We generate client
- Client is where you can talk to the DB. 
- Server needs to talk to DB. 
- Prisma gives nice client. 
- It comes form npm module. This has the typescript type based on the schema. 
- NextJS can create API route within the framework. 
- Usually, we have to creat the API route in the back-end. 
Created it under api folder. 

- JWT or cookies.

# 3. React hook form

- Form without help would be very annoying. 
- You have to eventhandling, error, change error message etc. 
- We have a lot of forms. 
- React hook form allows you to build form with validation and events that you need with minimal code. 
- This is amazin package. 
- The moment you dominate react form, it takes 5 minutes to implement form.


# 3.1 Making form alone

- npm install reacthookform.
- Problem. React-hook form with react 16 or 17. 
```
npm i react-hook-form
```
- We add the --legacy-peer-deps next to the npm i to avoid error. 
```
npm i react-hook-form --legacy-peer-deps
```

*How do you create a form?*

pages/form.tsx
```
export default function Forms() {
	return (
		<form>
		
	
	);

}
```

Steps:

1. Without react form we create three states. We connect state with inputs. 

2. WE give them default value of empty string
3. Listen to the own change event
```
import { useState } from "react";

export default function Forms() {
  const [username, setUsername] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [formErrors, setFormErrors] = useState("");
  const [emailError, setEmailError] = useState("");
  const onUsernameChange = (event: React.SyntheticEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setUsername(value);
  };
  const onEmailChange = (event: React.SyntheticEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setEmailError("");
    setEmail(value);
  };
  const onPasswordChange = (event: React.SyntheticEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setPassword(value);
  };
  const onSubmit = (event: React.SyntheticEvent<HTMLFormElement>) => {
    event.preventDefault();
    if (username === "" || email === "" || password === "") {
      setFormErrors("All fields are required");
    }
    if (!email.includes("@")) {
      setEmailError("email is required");
    }
  };
  return (
    <form onSubmit={onSubmit}>
      <input
        value={username}
        onChange={onUsernameChange}
        type="text"
        placeholder="Username"
        required
        minLength={5}
      />
      <input
        value={email}
        onChange={onEmailChange}
        type="email"
        placeholder="Email"
        required
      />
      {emailError}
      <input
        value={password}
        onChange={onPasswordChange}
        type="password"
        placeholder="Password"
        required
      />
      <input type="submit" value="Create Account" />
    </form>
  );
}
```

- Remember that user needs to submit a form. 

- We can change three react synthetic event to one function. 
- But to make a long story short, these are too much work that we are doing. 
- If we only has HTML validation, users can go to inspect and change the HTML to change the required and take different types. 
- Therefore, we do need a separate validation. 
	- We have to think about error. 
	- Validation example:
```js
const onSubmit = (event: React.SyntheticEvent<HTMLFormElement>) => {
    event.preventDefault();
    if (username === "" || email === "" || password === "") {
      setFormErrors("All fields are required");
    }
    if (!email.includes("@")) {
      setEmailError("email is required");
    }
  };
 // What if the email doesn't include @
 // What if the email field is empty.
 // min length how big it is all these parts, you have to do it. 
```
- There are many things > 
	- show error. 
	- input type
	- invalid
	- Red
	- Do validation of what the user has typed. 
	- All these things are so many implementation by ourselves. 
	
# 3.2. Take all the forms into a React hook Form

- Create a wishlist for a form component. 
1. Less Code
2. Control over validation
3. Have control over inputs. 
4. Have State control.

pages/forms.tsx
```tsx
// import useForm
import { useForm } from "react-hook-form";

// Less code (c)
// Better validation
// Better Erros (set, clear, display)
// Have control over inputs
// Dont deal with events (c)
// Easier Inputs (c)

export default function Forms() {
  // create useForm (register)
  const { register } = useForm();
  return (
    <form>
      <input
        {...register("username")}
        type="text"
        placeholder="Username"
        required
      />
      <input {...register("email")} type="email" placeholder="Email" required />
      <input
        {...register("password")}
        type="password"
        placeholder="Password"
        required
      />
      <input type="submit" value="Create Account" />
    </form>
  );
}
```

- watch allows you to watch the code. 
- basically, it will keep track of any changes made on the input.

# 3.3 Validation

- Maybe the browser doesn't support required.
- You want to check using JavaScript and validate its function. 
- You can write out a validation using {required:true}

```tsx
import { useForm } from "react-hook-form";

// Better validation
// Better Erros (set, clear, display)
// Have control over inputs

export default function Forms() {
  const { register, handleSubmit } = useForm();
  const onValid = () => {
    console.log("im valid bby");
  };
  return (
    <form onSubmit={handleSubmit(onValid)}>
      <input
        {...register("username", {
          required: true,
        })}
        type="text"
        placeholder="Username"
      />
      <input
        {...register("email", { required: true })}
        type="email"
        placeholder="Email"
      />
      <input
        {...register("password", { required: true })}
        type="password"
        placeholder="Password"
      />
      <input type="submit" value="Create Account" />
    </form>
  );
}
```
handleSubmit(onValid): take two functions from you.
1. function that will be executed if your form is valid. 
2. another function that will be called onSubmit={}

- You do not need to validate individually but it will handle all validations for you.

- There are pre-written rules that are common for most forms and it will implement that for you. Once you know what is being checked, you can add separate validations if needed. 

- Validation continued......

- If you hover your mouse over, you will see what are options that we get. 
- options are min, max, maxLength, minLength, there are some. These are supported in HTML. 


```tsx
import { FieldErrors, useForm } from "react-hook-form";

// Better validation
// Better Erros (set, clear, display)
// Have control over inputs

interface LoginForm {
  username: string;
  password: string;
  email: string;
}

export default function Forms() {
  const { register, handleSubmit } = useForm<LoginForm>();
  // onValid > prints "im valid bby"
  const onValid = (data: LoginForm) => {
    console.log("im valid bby");
  };
  // onInvalid > print errors. 
  const onInvalid = (errors: FieldErrors) => {
    console.log(errors);
  };
  return (
    // Instead of putting required "true," you can write the error message that users will see.
    <form onSubmit={handleSubmit(onValid, onInvalid)}>
      <input
        {...register("username", {
          required: "Username is required",
          minLength: {
            message: "The username should be longer than 5 chars.",
            value: 5,
          },
        })}
        type="text"
        placeholder="Username"
      />
      <input
        {...register("email", { required: "Email is required" })}
        type="email"
        placeholder="Email"
      />
      <input
        {...register("password", { required: "Password is required" })}
        type="password"
        placeholder="Password"
      />
      <input type="submit" value="Create Account" />
    </form>
  );
}
```

minLength {set up length, and message for users}
Two options:
1. Write the value of your rule.
```
minLength: 5
```
2. Write the message (threw when the rule is broken.) along with the rule. 
```
minLength: {
  message: "The username should be longer than 5 chars.",
  value: 5,
},
```

# 3.4 Errors

- We figured out that we have browser validation that is given in HTML. 
- If you want to create a custom validation, you can do this. 

```
<input
 {...register("email"), {
    required: "Email is required",
    validate: {
      notGmail: (value => !value.include("@gmail.com"),
      },
 })}
 ......
>
```

- You can even get the email error and display to the front-end. 
  - retrieve an error 
  - make the error goes away. 

- Validation patterns
1. When the user submits, validation happens. 
2. When the user types. 

- There are some website that immediately let the user know as they type. (Turn green)

- Change the border depending on the state of the form. 
```tsx
// If the error message exists, it will change the border-red-500.
<input
  className={`${Boolean(errors.email?.message) ? "border-red-500"}`}
>
```

# 3.5 Extras

- Path: react-hook-form.com
- Please look at the documentation

Functions:
1. watch("email"): watch email input from the form.
2. clearErrors. 
3. setErrors: if you submit to a back-end, you get an error, and you want to show that error. 
4. reset
5. resetField.

# 4. Refactoring

# 4.1 Enter Form

- Create interface options because there are two ways to login
Input:
```js
interface EnterForm {
  email?: string;
  phone?: string;
}
```

- Depending on user's preference, we give different input tags.
```
   {method === "email" ? (
            <Input
              register={register("email", {
                required: true,
              })}
              name="email"
              label="Email address"
              type="email"
              required
            />
          ) : null}
          {method === "phone" ? (
            <Input
              register={register("phone")}
              name="phone"
              label="Phone number"
              type="number"
              kind="phone"
              required
            />
          ) : null}
```

- When we click submit, the input data remains. In order to clear the form, you use reset() to clear inputs.
```
  const onPhoneClick = () => {
    reset();
    setMethod("phone");
  };
```
- Onvalid, accept data.
```
  const onValid = (data: EnterForm) => {
    console.log(data);
  };
```

- shorcut: [key: string]: any; (allows to send any props to components)


- useFormRegisterReturn
- 
```
```

# 4.2. Form Submission

- Create POST request.
pages/api/users/enter.tsx
```js
import { NextApiRequest, NextApiResponse } from "next";
import client from "../../../libs/client";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method !== "POST") {
    // status of bad request
    res.status(401).end();
  }
  console.log(req.body.email);
  res.status(200).end();
}
```

- Show on the page
- ONvalid, it will call the API fetch.

pages/enter.tsx
```js

const Enter: NextPage = () => {
  const [submitting, setSubmitting] = useState(false);
  const { register, handleSubmit, reset } = useForm<EnterForm>();
  const [method, setMethod] = useState<"email" | "phone">("email");
  const onEmailClick = () => {
    reset();
    setMethod("email");
  };
  const onPhoneClick = () => {
    reset();
    setMethod("phone");
  };
  const onValid = (data: EnterForm) => {
    setSubmitting(true);
    fetch("/api/users/enter", {
      method: "POST",
      body: JSON.stringify(data),
      headers: {
        "Content-Type": "application/json",
      },
    }).then(() => {
      setSubmitting(false);
    });
  };
  return (
    ......
  )

```

- What does the API return?
For now, you just make sure that you are getting success or failure.

- If you read the documentation, req.body gets encoded based on the encoding of req.

- utility functions will help us make our lives better.

Steps:
1. Fetch - Post
2. Post - Stringify
3. Set Content-Type.
4. Users - we are submitting or not
5. We have to error response if the status is not what we want. 
6. SWR - get data, cache data, mutate data. 
POST - we need to have our own cutom function. 

# 4.3 Clean Code

- Create  client/useMutation.tsx
- Put in hook. 
What is the best developer
What are you going to do?
1. Receive an array from my hook. 
pages/enter.tsx
```tsx
// receive an array from my hook.
const [enter, { loading, data, error }] = useMutation("/api/users/enter");
```
POST means you send data to the BE, which means you can mutate the status of DB. 
enter function triggers the posting.
second arg is notification. You want to receive an object.
onValid -> enter() function -> add the whole data (...data)
This is the developer experience that Nico wants. 
useMutation needs to know what url that you are going to mutate. 
"/api/users/enter"

- Function and objects.

- useMutation development:

```tsx
export default function useMutation(url:string) {
	const [loading, setLoading] = useState(false);
	const [data, setData] = useState<undefined |any>(undefined)
	const [error, setError] = use
	function mutation(data:any) {}
	return [mutation, ]

}

```

- Implement useMutation function

Steps:
We receive url from the user and we want to notify users when it is loading, if there is data or error. 
Steps:
1. SetLoading(true)
2. fetch (url, method:"POST"
3. Send headers to content-Type application json. 
4. body - json stringify.
5. Return json response - catch - receive json. 
6. 

- This is a short code to create a POST request.
- You can create a state and make it into an object. 
- 

# 4.4 withHandler

- Make a funciton that makes eaiser to write a handler. 
Replace below:
```
  if (req.method !== "POST") {
    res.status(401).end();
  }
```

- Handler is the one that handles the view. 
- We have to export default the function.
- Whatever code that we make has to return a function that NextJS is going to export. 
- Export default withHandler
```js
export default withHandler("POST", handler);
// First arg: method
// recieve the function that you want to execute. 
```
- NextJS likes to have return and sync. 
- We want to retun what NextJS execute. 
- We are going to retun a function that what nextJS is returning. 
- localhost/users/api -> You get hello.
- When we call a withhandler function, returns a handler and handler gets executed.

1. Withhandler runs > replace the code > instead of function you are running the handler. 
```
export default withHandler("POST", handler); 
->
import { NextApiRequest, NextApiResponse } from "next";

export default function withHandler(
  method: "GET" | "POST" | "DELETE",
  fn: (req: NextApiRequest, res: NextApiResponse) => void
) {
  return async function (req: NextApiRequest, res: NextApiResponse) {
    if (req.method !== method) {
      return res.status(405).end();
    }
    try {
      await fn(req, res);
    } catch (error) {
      console.log(error);
      return res.status(500).json({ error });
    }
  };
}
->

```

Steps to build withHandler
1. create a function that takes req, res
```
export default function withHandler(
  method: "GET" | "POST" | "DELETE",
  fn: (req)
)
```
- Call withHander -> return another function below
```js
async function (req: NextApiRequest, res: NextApiResponse) {
    if (req.method !== method) {
      return res.status(405).end();
    }
    try {
      await fn(req, res);
    } catch (error) {
      console.log(error);
      return res.status(500).json({ error });
    }
  };
```

- withHandler is just a shell. 

# 4.5 Paths

- If you don't want to see "../../...," you can set the path in the tsconfig.json. 

Steps:
1. Create paths in the tsconfig.json
```
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "baseUrl": ".",
    "paths": {
      "@libs/*": ["libs/*"],
      "@components/*": ["components/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

2. use @<key word> to replace all ../ that leads to a particular folder that you want. 



# 5. Authentication

## 5.1 Introduction

We are going to create an authentication

Steps:
1. User sends a phone number that they have.
2. Front-end (mutation function)
3. Back-end: look for the user based on a phone number
- If exist, we bring it out from the DB.
- If not, we create a new account. 
4. User will get a passcode (We use Twilio platform to accomplish this) (SMS)
5. User will receive a token in their phone number. After you get a phone number.
6. User will input a token number. We will take it into a backend and Log the user in after we authenticate.

==Optional==
7. If you are not logged in, you are not able to see that page. 
8. API function -> Learn how to get the information about the user in the request.

Authenticcation: Learn who the user is.
Authorization: Check if the user is allowed to see what they are going to see. 

# 5.1 Accounts Logic

Steps:
1. We are going to start with API.
pages/api/users/enter.tsx
```js
import { NextApiRequest, NextApiResponse } from "next";
import withHandler from "@libs/server/withHandler";
import client from "@libs/server/client";


async function handler(req: NextApiRequest, res: NextApiResponse) {
  const {phone, email} = req.body;
  // if the email provided is there, we get it from the DB.
  if(email){
    const user = await client.user.findUnique({
        where: {
          email,
        }
    });
    if (!user) {
      await client.user.create({
        data {
          name: "Anonymous",
          email,
        },
      });
    }
    console.log(user);
  }
  return res.status(200).end();
}

export default withHandler("POST", handler);
```

- Before you test your code, run psscale. 
- Quick convert to numbers = +
- ursert: update if eixst; create if doesn't exist. 
```js
  const user = await client.user.upsert({
    where: {
      ...payload,
    },
    create: {
      name: "Anonymous",
      ...payload,
    },
    update: {},
  });
  console.log(user);
```
- ES6 feature: if..else inside the object.
```js
// if the phone number is there, we return 
  where: {
    ...(phone ? {phone:+phone}:{}),
    ...(email ? {email}: {}),
  }
```
More shortcut:
```js
  const payload = phone ? { phone: +phone } : { email };
  const user = await client.user.upsert({
    where: {
      ...payload,
    },

```

# 5.2 Token logic

You can set up a token logic inside the schema.prisma.
prisma/schema.prisma
```js
......

model User {
  id        Int      @id @default(autoincrement())
  phone     Int?     @unique
  email     String?  @unique
  name      String
  avatar    String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  tokens    Token[]
}

model Token {
  id        Int      @id @default(autoincrement())
  payload   String   @unique
  user      User     @relation(fields: [userId], references: [id])
  userId    Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

- After you created a shema, you have to push to the db. 
```
npx prisma db push
```

- Check on the planetScale console whether the token is applied or not.

- Our db is ready to handle tokens. 

pages/api/users/enter.tsx
```js

```

- You can view whether the token is generated or not from the planetScale studio.
- createOrCreate: if the user with the specification already exist, please connect to the token. If the user with the specification doesn't exist, please create with those properties and connect to the token. 

- Payload should be unique. If you try to run the code, payload will not be unique.
- 

# 5.3 Twilo Setup

- Payload token random generator added. 
- Twilo: we have a trial for $15 trail
	- Create a messaging service. 
- Twilio_SID needs to be included in the env.
- Messaging > services > new messaging service "carrot-market" > Use this to notify the users. 
- Try it out > Get set up > Tell us this messaging service, we need to buy a phone number. 
- SMS - API request that you need to make to send messages. 
- There are ID, bodytext inputs and you can send SMS messages.

# 5.4 Sending SMS

- You have to get Account SID and Auth token. 
- You have to click the name of the account and it will take you to the dashbaords. 
- Copy paste SID to env file.  
- There are four things to get this ready:
1. SID
2. Token
3. Messaging Service SID
4. Phone number to github or put it under .env file. 

Steps to set up Twilo SDK:
1. await twilioClient.messages.create({})
2. 
pages/api/users/enter.tsx
```js
import twilio from "twilio";
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";

const twilioClient = twilio(process.env.TWILIO_SID, process.env.TWILIO_TOKEN);

  if (phone) {
    const message = await twilioClient.messages.create({
      messagingServiceSid: process.env.TWILIO_MSID,
      to: process.env.MY_PHONE!,
      body: `Your login token is ${payload}.`,
    });
    console.log(message);
  }
```

- Twilo needs a country code because it is an international platform. 
- Make sure to refresh the server once you update .env file. 
- When you test, you should be able to receive the token that you inputed.