## Section 4.5: Check Permissions of a File or Directory

`fs.access()` determines whether a path exists and what permissions a user has to the 
file or directory at that path. `fs.access` doesn't return a result rather, if it 
doesn't return an error, the path exists and the user has the desired permissions.
The permission modes are available as a property on the fs object, `fs.constants`

- `fs.constants.F_OK` - Has read/write/execute permissions (If no mode is provided, this is the default)
- `fs.constants.R_OK` - Has read permissions
- `fs.constants.W_OK`- Has write permissions
- `fs.constants.X_OK`- Has execute permissions (Works the same as `fs.constants.F_OK` on Windows)

### Asynchronously

```js
const fs = require('fs');

let path = '/path/to/check';

// checks execute permission
fs.access(path, fs.constants.X_OK, (err) => {
  if (err) {
    console.log("%s doesn't exist", path);
  } else {
    console.log('can execute %s', path);
  }
});

// Check if we have read/write permissions
// When specifying multiple permission modes
// each mode is separated by a pipe : `|`
fs.access(path, fs.constants.R_OK | fs.constants.W_OK, (err) => {
  if (err) {
    console.log("%s doesn't exist", path);
  } else {
    console.log('can read/write %s', path);
  }
});
```

### Synchronously
`fs.access` also has a synchronous version `fs.accessSync`. When using `fs.accessSync` 
you must enclose it within a `try/catch`block.

```js
// Check write permission
try {
  fs.accessSync(path, fs.constants.W_OK);
  console.log('can write %s', path);
}
catch (err) {
  console.log("%s doesn't exist", path);
}
```