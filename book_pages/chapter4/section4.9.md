## Section 4.9: Avoiding race conditions when creating or using an existing directory

Due to Node's asynchronous nature, creating or using a directory by first:
- checking for its existence with `fs.stat()`, then
- creating or using it depending of the results of the existence check,

can lead to a race condition if the folder is created between the time of the check 
and the time of the creation. The method below wraps `fs.mkdir()` and `fs.mkdirSync()` 
in error-catching wrappers that let the exception pass if its code is `EEXIST` 
(already exists). If the error is something else, like `EPERM` (pemission denied), 
throw or pass an error like the native functions do.

### Asynchronous version with fs.mkdir()

```js
let fs = require('fs');

function mkdir (dirPath, callback) {
  fs.mkdir(dirPath, (err) => {
    callback(err && err.code !== 'EEXIST' ? err : null);
  });
}

mkdir('./existing-dir', (err) => {
  if (err)
    return console.error(err.code);
  // Do something with `./existingDir` here
});
```


### Synchronous version with fs.mkdirSync()
```js
function mkdirSync (dirPath) {
  try {
    fs.mkdirSync(dirPath);
  } catch(e) {
    if ( e.code !== 'EEXIST' ) throw e;
  }
}

mkdirSync('./existing-dir');
```