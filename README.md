[![Build Status](https://travis-ci.org/piccard21/npm-es6-webpack-skeleton.svg?branch=develop)](https://travis-ci.org/piccard21/npm-es6-webpack-skeleton)

# npm-es6-webpack-skeleton

A skeleton for developing npm-modules in ES6 by using webpack with babel-loader.

## Install

Install with [npm](https://www.npmjs.com/)

## Features

* Production, development & development-server mode
* Minifying
	* Tree-Shaking
* Hot development-server
	* Hot module replacement
	* webpage is automatically created
	* use of a template 
		* modify **index.ejs** for your needs
* clean dist-directory before recompiling
* configuration-file in **config/app.js**
* sourcemap if wanted
* postcss & cssnext
* Babel for converting ES6
* mocha & chai for testing


## Access 
This package is bundled in a way that will achieve the following goals:
 
* Setting the library name as **npm-es6-webpack-skeleton**
  * change the name in **package.json**
* Exposing the library as a variable  **npmEs6WebpackSkeleton**.
  * change the constant **libraryVarName** in **config/app.js**
* Being able to access the library inside **Node.js**.

Also, the consumer should be able to access the library the following ways:

* **ES2015 module**
* **CommonJS module**
* **Global variable**, when included through script tag.

## Usage

### Setup your module-skeleton

```sh
npm init -y 
npm i npm-es6-webpack-skeleton 
mv node_modules/npm-es6-webpack-skeleton /WHERE/EVER/YOU/WANT/IT
cd /WHERE/EVER/YOU/WANT/IT
mv package.json.example package.json
vim package.json
npm i -D
 ```   


### Write your module

Open **src/index.js** and begin coding. Webpack will only bundle the code you are importing, which is also the case for your css & sass files, so you have explicitly ask for it. Here's an example:

```
// import essential libraries
import _ from 'lodash';
import * as mathLib from './lib/math.js';
import {
    cube,
    square,
    diag as d
} from './lib/math.js';
import printMe from './lib/print.js';
import * as pointLib from './lib/class.Point.js';

// import some css & sass
import './css/styles-01.css'; 
import './css/styles-02.css'; 
import './css/styles-03.css'; 
import './sass/sass-01.scss';
import './sass/sass-02.scss'; 

// are we in development-mode?
if (process.env.NODE_ENV !== 'production') {
    console.log('Looks like we are in development mode!');
}

// exports
export default function(x) {
    return 'I am the default function.';
}
export function hello() {
    return _.join(['Hello', 'world'], ' ');
};
export function print(num) {
    return printMe();
};
export function numFormatter(number, locale) {
    return number.toLocaleString(locale);
};
export const math = mathLib;
export const mathSelection = {
    cube,
    square,
    diag: d,
};
export const point = pointLib;
```

For easier development use **npm --watch** or **npm start**. The latter one starts the development-server and brings up a page, which you can modify for your needs in **src/template/index.ejs**.

**npm --watch** creates the files **app.css** && **app.js** in **dist/**, whereas **npm prod** creates the minfied versions as well.


### Import the module 

The module already works out of the box, so you can link it easily to an application, without publishing it:

```sh
cd /WHERE/EVER/YOU/WANT/IT
npm link
cd /PROJECT/WHICH/WILL/USE/THE/MODULE
npm link NAME-OF-YOUR-MODULE
```

Import ...

```
// ES2015 module import
import theDefault, * as ns from 'npm-es6-webpack-skeleton';

// CommonJS module require
var ns = require('npm-es6-webpack-skeleton');
```

... and use it

``` 
console.info("theDefault", theDefault());
console.info("ns.hello", ns.hello());
console.info("ns.print", ns.print());
console.info("ns.numFormatter", ns.numFormatter(21)); 
 
// math
console.log("ns.math.sqrt",ns.math.sqrt(9)); 
console.log("ns.math.square", ns.math.square(11)); 
console.log("ns.math.cube", ns.math.cube(11));
console.log("ns.math.diag", ns.math.diag(4, 3));


// mathSelection
console.log("ns.mathSelection.square", ns.mathSelection.square(21)); 
console.log("ns.mathSelection.cube", ns.mathSelection.cube(11));
console.log("ns.mathSelection.diag", ns.mathSelection.diag(11,22));


// pointLib
const point = new ns.point.Point();
const colorpoint = new ns.point.ColorPoint();
point.create(11,22)
colorpoint.create(55,88, "red");
console.log("point", point.toString());
console.log("colorpoint", colorpoint.toString());
```

The consumer also can use the library by loading it via a script tag:

```
<html>
...
<script src="node_modules/npm-es6-webpack-skeleton/dist/app.js"></script>
<script>
  // ...
  // Global variable
  npmEs6WebpackSkeleton.hello()
  // Property in the window object
  window.npmEs6WebpackSkeleton.print()
  // ...
</script>
</html>
```


### Configuration

In **config/app.js** you have some options for basic configuration, like the library's variable-name, if you wanna have a sourcemap or the [target](https://webpack.js.org/guides/author-libraries/), which is **umd** per default. 

```
const libraryVarName = 'npmEs6WebpackSkeleton';
const libraryTarget = 'umd';
const sourcemap = (process.env.NODE_ENV === 'PRODUCTION') ? true : false;
```

## Scripts

* npm run
  * **start** - bring up the development server, where an index.html is automatically created. Modify the template, which you will find in **src/template/index.ejs**, so it fits your needs. The server is reachable under *localhost:8080*
  * **dev** - create dist/app.js, not minimized
  * **watch** - watch the src-directory
  * **prod** - create dist/app.js in a minified version, where tree-shaking is triggered as well 
  * **test** - mocha & chai tests 
 

## Running tests

Install dev dependencies:

```sh
npm i 
npm run dev
npm test
```
 

## Author

**Andreas Stephan** 

## License

Copyright © 2017 [Andreas Stephan](https://cafe-serendipity.com)
Released under the MIT license. 
