---
date: "2016-02-08"
summary: I've found a nice solution for reading a file, be able to perform other
  operations with the read data and write it to another file.
categories:
- developer
tags:
- javascript
- node.js
title: How to read a file and write it again with separate functions with node.js
---

I've found an interesting [answer](http://stackoverflow.com/a/17645814) to a question on Stack Overflow about how to read a file, be able to perform other operations with that information and finally write it the another file.

This is the first solution proposed:

```javascript
function copyData(savPath, srcPath) {
  fs.readFile(srcPath, 'utf8', function (err, data) {
    if (err) throw err;
      //Do your processing, MD5, send a satellite to the moon, etc.
      fs.writeFile (savPath, data, function(err) {
        if (err) throw err;
          console.log('complete');
      });
  });
}
```

But the solutions that I prefer is that one with two separate methods:

```javascript
function getFileContent(srcPath, callback) { 
  fs.readFile(srcPath, 'utf8', function (err, data) {
    if (err) throw err;
      callback(data);
    }
  );
}

function copyFileContent(savPath, srcPath) { 
  getFileContent(srcPath, function(data) {
    fs.writeFile (savPath, data, function(err) {
      if (err) throw err;
        console.log('complete');
      });
  });
}
```

Here is a post about [Reading and Writing File in Node.js](http://www.javabeat.net/nodejs-read-write-file/) that explain how to use *fs* module for file I/O operations.