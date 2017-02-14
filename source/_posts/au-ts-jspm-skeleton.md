---
title: Creating an Aurelia app from scratch using typescript & jspm
date: 2017-02-14 20:48:41
tags: [aurelia, typescript, jspm, systemjs, setup, skeleton]
---

There are a number of ways to create a new Aurelia app (the CLI, the skeleton projects etc), and plenty of demos on how to do it.  Having been through this process, I was keen to create my own skeleton application, specifically using typescript and jspm/systemjs, to help embed the things I'd learned.  Here's what I did...


## Prerequisites
Before getting started, make sure you have node an npm installed, and the following packages installd globaly

```
npm install -g typescript
npm install -g jspm
npm install -g gulp-cli
```

I also use lite-server to host apps for testing, you can install that globally too...
```
npm install -g lite-server
```

## Starting with an empty folder...
We are going to use npm to manage development dependencies, and jspm to manage the the libraries our finished app will use.

### npm
Create a new npm project with the follwing commandand accept the default settings...
```
npm init
```

Then install our development dependencies..
```
npm install --save-dev typescript jspm lite-server gulp gulp-typescript @types/es6-shim
```

At this point, our directory should now contain a package.json file and a node_modules folder.

### jspm

After that, create a new jspm project with...
```
jspm init
```
Accept all the defaults EXCEPT for *Which ES6 transpiler would you like to use, Babel, TypeScript or Traceur?* - make sure to choose TypeScript.

Then we can install the libraries we want to use to build our app...

```
jspm install aurelia-framework aurelia-bootstrapper aurelia-pal-browser aurelia-polyfills
```

### aurelia typings
At the time of writing (to the best of my knowledge), there is an issue with the typescript typings resolution for libraries that provide their own types (rather than through npm install @types/..) that you install via jspm. Aurelia is one such library - the typescript compiler cannot find the Aurelia type definitions if you install aurelia with jspm.

To get round this, we can also install the aurelia libraries as development dependencies with npm...
```
npm install --save-dev aurelia-framework aurelia-bootstrapper aurelia-pal-browser aurelia-polyfills
```
**Note**: This is only so that the typescript compiler can find the aurelia type efinitions, and won't actually be used from the node_modules folder.
 

We should now have a config.js file and jspm_packages alongside our package.json and node_modules folder...

![setup complete](./au-ts-jspm-skeleton/setup.PNG)


## Creating the app...
All of the code we write, will go into a folder caled *src*. We are going to create a typical aurelia configure function and a simple page to illustrate data binding.

Create the *src* folder in the project rot directory, and create 3 new files within it..

![Creating source files](./au-ts-jspm-skeleton/src.PNG)

### main.ts
```typescript
import { Aurelia } from "aurelia-framework";

export function configure(aurelia: Aurelia) {

    aurelia
        .use
        .standardConfiguration()
        .developmentLogging();

    aurelia.start().then(a => a.setRoot());
}
```

### app.ts
```typescript
export class App {
    constructor() {
        this.welcomeMessage = "Welcome to our Aurelia application";
    }

    private welcomeMessage: string;
    private inputText: string;
}
```

### app.html
```html
<template>

    <h2>${welcomeMessage}</h2>

    <div>
        <input type="text" value.bind="inputText">
    </div>

    <div>
        ${inputText}
    </div>

</template>
```

## Building the output...
We will serve files from a directory called *dist*. This folder will contain the result of buildin/processing/transpiling the code we have written in the *src* folder. To do this, we wil use **gulp**.

### tscongig.json
The first thing we need to do is turn the typescript files we have written into javascript files. We use the typescript compiler to do this, with some options specific to our project - these options go into a file called *tsconfig.json*.

Create the tsconfig.jsonn file in the prohect root and add the following...
```
{
    "compilerOptions": {
        "module": "amd",
        "moduleResolution": "node",
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true,
        "sourceMap": true,
        "target": "es5",
        "outDir": "dist/"
    },
    "include": [
        "./src/**/*.ts"
    ]
}
```

### update config.js
We can also updated the config.js file to tell systemjs to always start in the dist/ folder when looking for the modules we've written...
```
System.config({
  baseURL: "/",
  defaultJSExtensions: true,
  transpiler: "typescript",
  paths: {
    "*": "dist/*", // <------------------------ ADD THIS LINE
    "github:*": "jspm_packages/github/*",
    "npm:*": "jspm_packages/npm/*"
  },
  map: {
      ...
      ...
      ...
```  

### gulp
Add a new file, gulpfile.js, in the project root and add the follwing...
```javascript
const gulp = require("gulp");
const ts = require("gulp-typescript");

const tsProject = ts.createProject("tsconfig.json");

gulp.task("build:ts", () => {
    tsProject
        .src()
        .pipe(tsProject())
        .pipe(gulp.dest("dist/"));
});

gulp.task("build:html", () => {
    gulp
        .src("src/**/*.html")
        .pipe(gulp.dest("dist/"));
});

gulp.task("build", ["build:ts", "build:html"]);

```

Now run `gulp build` - you see that the dist/ folder has been created with our html templates and transpiled typescript files...

![Our built files](./au-ts-jspm-skeleton/build.PNG)


### Serving up...
Al that's left is to create the index.html and serve what we've done locally using lite-server.

## index.html
Create the folloing index.html in the project rot...
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Aurelia App</title>

    <script src="jspm_packages/system.js"></script>
    <script src="config.js"></script>
    <script>
        System.import("aurelia-bootstrapper");
    </script>
</head>

<body aurelia-app="main">

</body>

</html>
```

## lite-server configuration
Create a file caled bs-config.json in the project root with the following options...
```
{
    "port": 3000,
    "files": ["./**/*.{html,htm,css,js}"],
    "server": {
        "baseDir": "./"
    },
    "exclude":[
        "node_modules/",
        "jspm_packages/"
    ]
}
```

## Run it...
From the command line, run...
```
lite-server
```


