https://www.youtube.com/watch?v=xQgBJIye5EU

`tsc --init`
- This command will create a new `tsconfig.json` file in the current directory.

Recommended to enabled:

```json
{
  "compilerOptions": {
    "target": "ESNext", /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    "module": "ESNext", /* Specify what module code is generated. */
    "lib": ["ESNext", "DOM"], /* Specify library files to be included in the compilation. */
    "esModuleInterop": true, /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
    "forceConsistentCasingInFileNames": true, /* Ensure that casing is correct in imports. */
    "strict": true, /* Enable all strict type-checking options. Such as 'strictNullChecks', 'strictPropertyInitialization' */
    "skipLibCheck": true, /* Skip type checking all .d.ts files. */
    "noUncheckedIndexedAccess": true, /* Disallow indexing dynamically typed objects. */
    "outDir": "./dist", /* Specify the output directory for emitted JavaScript files. */
  },
  "include": ["src/**/*"] /* List of files to include in the compilation. */
}
```

`target`
- This will set the JavaScript language version for emitted JavaScript and include compatible library declarations.
- The default in this case is `ES2016`.
- But common for people to use `ESNEXT`, which uses the most recent features of ECM script.
- If we want to support older browsers we can use `ES5`, this will impact how the code is compiled.

`module`
- This will specify what module code is generated.
- The default in this case is `CommonJS`.
- Usually if you are using a module bundler this may need to be cahnged to work with your bundler.

`lib`
- Checking this is saying okay these are libraries that I want to include in the execution environment that we expect for this code to run in you know in production and let me use the methods and globals that are available in that execution environment 
- Example if we want to use `document` and `console` we can add `DOM` to the list of libraries.
- Or if we want to use newer JavaScript features we can add `ESNext` for later versions or a specific version to use features up to that version.

`"noUncheckedIndexedAccess": true`
- This flag will ensure that the compiler checks that the type of every indexed access is either `undefined` or assignable to the type of the property being accessed.
- Adding this will help avoid issues like this

```typescript
function add(a:number, b:number):number {
    return a + b;
}

const numbers: Array<number> = [];

const result = add(1, numbers[0]); // Argument of type 'number | undefined' is not assignable to parameter of type 'number'.  Type 'undefined' is not assignable to type 'number'.ts(2345)
console.log(result);

```
In this case the compiler will throw an error because `numbers[0]` is not assignable to `number`. By adding the `noUncheckedIndexedAccess` flag the compiler will check that the type of `numbers[0]` is `number`.
Which in this case is `undefined` because the numbers array is empty and we are trying to access `0` which does not exist.

Thus we can rectify the issue:
```ts
function add(a:number, b:number):number {
    return a + b;
}

const numbers: Array<number> = [];

const result = add(1, numbers[0] ? numbers[0] : 0);
console.log(result);

```


`"strict":true`
- This flag will ensure that the compiler will not allow you to use any undeclared variables.


## Auto Rebuild
Using `tsc -watch` to watch for changes.
- This command will watch for changes and rebuild the project based on the configuration in the `tsconfig.json` file every time the file changes.


# package.json

```json
{
  "name": "typescript-template",
  "version": "1.0.0",
  "description": "",
  "main": "dist/main.js",
  "scripts": {
    "clean": "rimraf dist",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "rimraf": "^5.0.1"
  }
}

```

We set the main entry point to the `dist/<name>.js` file. Which is where the generated code will be placed.

- `rimraf` is a command line utility that removes files and folders 
- Installed with `npm install rimraf --save-dev` if using windows.