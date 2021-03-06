
Files ending with `.js` or `.mjs`, or lacking any extension,
will be loaded as ES modules when the nearest parent `package.json` file
contains a top-level field `"type"` with a value of `"module"`.

The nearest parent `package.json` is defined as the first `package.json` found
when searching in the current folder, that folder’s parent, and so on up
until the root of the volume is reached.

<!-- eslint-skip -->
```js
// package.json
{
  "type": "module"
}
```

```sh
# In same folder as above package.json
node --experimental-modules my-app.js # Runs as ES module
```

If the nearest parent `package.json` lacks a `"type"` field, or contains
`"type": "commonjs"`, extensionless and `.js` files are treated as CommonJS.
If the volume root is reached and no `package.json` is found,
Node.js defers to the default, a `package.json` with no `"type"`
field.

`import` statements of `.js` and extensionless files are treated as ES modules
if the nearest parent `package.json` contains `"type": "module"`.

```js
// my-app.js, part of the same example as above
import './startup.js'; // Loaded as ES module because of package.json
```

