---
title: 'Setting up Karma with Aurelia, Typescript & SystemJS'
photos: '/images/karma-aurelia-ts-systemjs/toolchain.png'
tags:
  - aurelia
  - karma
  - testing
  - jspm
  - systemjs
  - mocha
  - chai
date: 2017-04-29 11:36:37
---

I'm relatively new to setting up unit testing for a javascript project, so when I set out to start adding it to an existing project I obviously turned straight to google.

After a bit of searching I settled on using Karma, Mocha and Chai.

As you'd expect, I found loads of information about unit testing javascript and typescript, and how to use mocha and karma etc, but struggled to get it to work with my project configuration.

I was using a combination of Karma, Mocha and Chai, Typescript, SystemJS and Aurelia, and I struggled to find much information about this specific setup.

I persevered and finally managed to get it to work, so wanted to write down what I did for anyone else who finds themselves in the same boat.


## Aurelia Skeleton Project

I'm starting with a basic Aurelia project (using Typescript and SystemJS) that I created quite quickly from scratch.  You can see how I did that here... [Creating an Aurelia app from scratch using typescript & jspm](https://amcn41r.github.io/blog/2017/02/14/au-ts-jspm-skeleton/).

Or, you can grab the source code from [here](https://github.com/AMCN41R/blog-repos/tree/master/au-ts-jspm-skeleton).

## Setting up

### Dependencies

First, we install the test libraries we want to use using npm, saving them as development dependencies...
```
npm install --save-dev karma mocha chai
```

some useful karma plugins...
```
npm install -- save-dev karma-systemjs karma-mocha karma-chai karma-phantomjs-launcher karma-mocha-reporter
```

the relevant type definitions...
```
npm install --save-dev @types/karma @types/mocha @types/chai
```

and install chai with jspm as a development dependency (to make it easier for SystemJS to load it when running the tests)...
```
jspm install --dev chai
```

### Karma Configuration

Next, create a new karma config file by running `karma init`.

Use the default settings **EXCEPT** the following...

*Which testing framework do you want to use ?* - **Mocha**
*Do you want to capture any browsers automatically ?* - **PhantomJS**

Now that we have a `karma.conf.js` in the project root, we need to add configuration for SystemJS, and tweak a couple of other options.

Open the `karma.conf.js` file and update the following...

* Add 'systemjs' as the first item in the frameworks array...
```
frameworks: ['systemjs', 'mocha' ]
```
* Set the files array to the following...
```
files: [
    'test/setup.ts',
    { 'pattern': 'test/*.ts' },
    { 'pattern': 'test/**/*.ts' }
],
```
* Change the reporter to use he mocha reporter...
```
reporters: ['mocha'],
```
* Add the following SystemJS configuration...
```
systemjs: {
    configFile: 'config.js',
    config: {
        paths: {
            '*': '*',
            'src/*': 'src/*',
            'typescript': 'node_modules/typescript/lib/typescript.js',
            'systemjs': 'node_modules/systemjs/dist/system.js',
            'system-polyfills': 'node_modules/systemjs/dist/system-polyfills.js',
            'es6-module-loader': 'node_modules/es6-module-loader/dist/es6-module-loader.js'
        },
        packages: {
            'test': {
                defaultExtension: 'ts'
            },
            'src': {
                defaultExtension: 'ts'
            }
        },
        transpiler: 'typescript',
    },
    serveFiles: [
        'src/**/*.*',
        'jspm_packages/**/*.js',
        'jspm_packages/**/*.json'
    ]
},
```

### calc.ts
Let's add a simple class that we can write tests for.

Create `calc.ts` in the `src` folder and add the following...
```typescript
export class Calc {
    add(a: number, b: number){
        return a + b;
    }
}
```

## Writing Some Tests
We will add all our test files inside a folder called 'test', so create this folder in the project root.

### setup.ts
Add a file inside the 'test' folder called `setup.ts`.

The `setup.ts` file is used to load the aurelia polyfills and browser abstraction layer for our tests...

```typescript
import "aurelia-polyfills";
import {initialize} from "aurelia-pal-browser";
initialize();
```
### calc.spec.ts
Now we can write a simple test for the calculator class.

Add a file called `calc.spec.ts` into the 'test' folder and add the following code...

```typescript
import {expect} from "chai";
import {Calc} from "../src/calc";

describe("Calc tests", () => {
    describe("Add", () => {
        it("passing 2 positive numbers returns the expected result", () => {
            // arrange
            const sut = new Calc();

            // act
            const result = sut.add(2,5);

            // assert
            expect(result).to.equal(7);
        });
        it("passing 2 negative numbers returns the expected result", () => {
            // arrange
            const sut = new Calc();

            // act
            const result = sut.add(-2,-5);

            // assert
            expect(result).to.equal(-7);
        });
    });
});
```

## Testing, testing
All that's left to do is run the tests.

You can do this from the command line with...
```
karma start
```

![karma test results](/blog/images/karma-aurelia-ts-systemjs/karma.png)


## And that's it!
You can find the finished source code [here](https://github.com/AMCN41R/blog-repos/tree/master/karma-aurelia-ts-systemjs).
