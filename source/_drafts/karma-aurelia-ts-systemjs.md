---
title: Setting up Karma with Aurelia, Typescript & SystemJS
tags: [aurelia, karma, testing, jspm, systemjs, mocha, chai]
---


INTRO



PREVIOS POST




First, we install the libraries we want to use using npm, saving them as development dependencies...
```
npm install --save-dev mocha chai karma karma-systemjs karma-mocha karma-chai karma-phantomjs-launcher
```
the relevent type definitions...
```
npm install --save-dev @types/karma @types/mocha @types/chai
```
and install chai with jspm as a development dependency (for systemjs)...
```
jspm install --dev chai
```

Next, create a new karma config file by running `karma init` using the default settings except the following...

*Which testing framework do you want to use ?* - Mocha
*Do you want to capture any browsers automatically ?* - PhantomJS

Now we have a karma.conf.js in the project root, we need to add configuration for systemjs, and tweak a couple of other options.

Open the karma.conf.js file and update the following...

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
]
```
* Add the following systejs configuration...
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
}
```









