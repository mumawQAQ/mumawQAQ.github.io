---
layout: post
title:  "Write your own npm package"
date:   2024-05-20 20:00:00 -0400
categories: [npm, javascript, typescript, tutorial]
---
In this tutorial, we will learn how to create a simple npm package using TypeScript. We will cover the following topics:
1. Setting up the project
2. Writing the code
3. Testing the package
4. Publishing the package

## Setting up the project
Create a new directory for the project and navigate to it.
```bash
mkdir my-npm-package
cd my-npm-package
```
<br>

Initialize the npm project with the following command, and `-y` flag will create a `package.json` file with default values, we will modify it later.
```bash
npm init -y
```
<br>

Then open up the project in your favorite code editor. Now, let's install the necessary dependencies; these are the dependencies there are used by the package.
```bash
npm install --save-dev @types/node @types/react @types/react-dom typescript react react-dom
```
<br>

After that, we need to define some peer dependencies, these are the dependencies that are required by the package that are not installed by the package itself. We will define these in the `package.json` file. **Note that the version numbers should match in the dev dependencies and peer dependencies.**
```json
{
  ...
  "peerDependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  }
}
```
<br>

Now, edit the `description` and `main` fields in the `package.json` file and add a key call `module`. The package will compile two versions of code, one for CommonJS and one for ESM.
```json
{
  ...
  "description": "A simple npm package",
  "main": "dist/cjs/index.js",
  "module": "dist/esm/index.js"
}
```
<br>

Next, we need to add the `files` field and `scripts` field to the `package.json` file. The `files` field is an array of files that should be included in the package. The `scripts` field is a set of scripts that can be run using `npm run <script-name>`.
```json
{
  ...
  "files": [
    "dist"
  ],
  "scripts": {
    "build": "rm -rf dist/ && npm run build:esm && npm run build:cjs",
    "build:esm": "tsc",
    "build:cjs": "tsc --module commonjs --outDir dist/cjs"
  }
}
```
<br>

Now, We need to init a TypeScript config file. Run the following command to create a `tsconfig.json` file. For more detail configuration, you can check this website https://www.typescriptlang.org/tsconfig/.
```bash
npx tsc --init
``` 
<br>

## Writing the code
Create a new directory called `src` and navigate to it. Then create a new file called `index.ts` in the `src` folder.
```bash
mkdir src
cd src
```
<br>

Create a new folder called `components` in the `src` folder. Then create a new file called `Button.tsx` in the `components` folder.
```bash
mkdir components
cd components
```
<br>

Add the following code to the `Button.tsx` file. This is a simple React component that renders a button.
```tsx
import React from "react";


export interface ButtonProps extends React.DetailedHTMLProps<React.ButtonHTMLAttributes<HTMLButtonElement>, HTMLButtonElement> {
    backgroundColor?: string;
    color?: string;
}


export const Button: React.FunctionComponent<ButtonProps> = (props) => {
    const {children, backgroundColor, color, style} = props;
    let _style: React.CSSProperties = style || {};

    if (backgroundColor) _style.backgroundColor = backgroundColor;
    if (color) _style.color = color;


    return (
        <button style={_style} {...props}>
            {children}
        </button>
    );
}
```
<br>

Create a new file called `index.ts` in the `component` folder. This file will export all the components in one place.
```ts
export * from "./Button";
```
<br>


Edit the `index.ts` file in the `src` folder and add the following code. This file will export all the libs in one place.
```ts
export * from "./components";
```
<br>


## Testing the package
Now, we need to build the package. Run the following command to compile the TypeScript code.
```bash
npm run build
```
<br>

Create a new directory for testing the package and navigate to it. Init a new React project in the test directory.
```bash
mkdir test
cd test
npx create-react-app my-app --template typescript
cd my-app
```
<br>

Install the package in the test project and start the React app.
```bash
npm install ../../
npm start
```
<br>

Now, you can test the package by importing the components from the package and using them in the React app.

## Publishing the package

To publish the package, you need to create an account on npmjs.com. Then, run the following command to log in to your account.
```bash
npm login
```
<br>

After that, run the following command to publish the package.
```bash
npm publish
```
<br>

That's it! You have successfully created and published an npm package.


this tutorial is inspired by [this video](https://www.youtube.com/watch?v=V_5ImTOmMh0&t=0s) by [The Nerdy Canuck](https://www.youtube.com/@TheNerdyCanuck)

