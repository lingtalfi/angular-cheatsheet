Angular 2 Cheatsheet
=========================
2016-07-22




package.json
-----------------

```json
{
  "name": "angularbase",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "angular2": "2.0.0-beta.0",
    "es6-promise": "3.0.2",
    "es6-shim": "0.33.13",
    "reflect-metadata": "0.1.2",
    "rxjs": "5.0.0-beta.0",
    "zone.js": "0.5.10",
    "systemjs": "^0.19.31"
  }
}

```



tsconfig.json
----------------

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "system",
    "sourceMap": true,
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false,
    "watch": true
  },
  "exclude": [
    "node_modules"
  ]
}

```