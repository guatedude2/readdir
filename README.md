# readdir
A lightweight node.js module to recursively read files in a directory.

## Installation

    npm install readfiles

## Usage

### readfiles(dir, [options], callback, [doneCallback])
Asynchronusly read the files in a directory

#### dir
A relative or absolute path of the directory to read files.

#### options

An optional object parameter with the following properties:

* **reverse**: a bollean value that reverses the order of the list of files before traversing them (defaults to false)
* **filenameFormat**: one of `readdir.FULL_PATH`, `readdir.RELATIVE`, or `readdir.FILENAME`, wether the callback's returns the full-path, relative-path or only the filenames of the traversed files. (default is `readdir.RELATIVE`)
* **doneOnError**: a bollean value wether to stop and trigger the "doneCallback" when an error occurs (defaults to true)
* **filter**: a string, or an array of strings of path expression that match the files being read (defaults to '**')
  * `?` matches one character
  * `*` matches zero or more characters
  * `**` matches zero or more 'directories' in a path
* **readContents**: a boolean value wether to read the file contents when traversing the files <sup>[\[1\]](#read-files)</sup> (defaults to true)
* **encoding**: a string with the encoding used when reading a file (defaults to 'utf8')
* **depth**: an integer value which limits the number sub-directories levels to traverse for the given path where `-1` is infinte, and `0` is none (defaults to -1)
* **hidden**: a boolean value wether to exclude hidden files prefixed with a `.` (defaults to true)


### callback(err, filename, content)

The callback function that is triggered everytime a file is read. If there's an error while reading the file the `err` parameter will contain the error that occured, otherwise the if `readContents` is true, the `contents` parameter will be populated with the contents of the file encoded using the `encoding` option.

<span id="read-files">[1]</span> The `contents` parameter will be `null` when the `readContents` option is `false`.

### doneCallback(err, files, count)

The callback function that is triggered once all the files have been read, passing the number of files read and an array with the full path of all files.

## Examples

The default behavior, is to recursively list all files in a directory. By default `readdir` will exclude all dot files.

```javascript
var readfiles = require('readfiles');

readfiles('/path/to/dir/', function (err, filename, contents) {
  if (err) throw err;
  console.log('File ' + filename + ':');
  console.log(content);
}, function (err, count, files) {
  console.log('Read ' + count + ' file(s)');
  console.log(files.join('\n'));
});
```

Read all files in a directory, excluding sub-directories.

```javascript
var readfiles = require('readfiles');

readfiles('/path/to/dir/', {
  depth: 0
}, function (err, content, filename) {
  if (err) throw err;
  console.log('File ' + filename + ':');
  console.log(content);
}, function (err, count, files) {
  console.log('Read ' + count + ' file(s)');
  console.log(files.join('\n'));
});
```

The above can also be accomplished using `filter`.

```javascript
var readfiles = require('readfiles');

readfiles('/path/to/dir/', {
  filter: '*' // instead of the default '**'
}, function (err, content, filename) {
  if (err) throw err;
  console.log('File ' + filename + ':');
  console.log(content);
}, function (err, count, files) {
  console.log('Read ' + count + ' file(s)');
  console.log(files.join('\n'));
});
```

Recursively read all files with "txt" extension in a directory and display the contents.

```javascript
var readfiles = require('readfiles');

readfiles('/path/to/dir/', {
  filter: '*.txt'
}, function (err, content, filename) {
  if (err) throw err;
  console.log('File ' + filename + ':');
  console.log(content);
}, function (err, count, files) {
  console.log('Read ' + count + ' text file(s)');
});

```

Recursively read all files with that match "t?t" in a directory and display the contents.

```javascript
var readfiles = require('readfiles');

readfiles('/path/to/dir/', {
  filter: '*.t?t'
}, function (err, content, filename) {
  if (err) throw err;
  console.log('File ' + filename + ':');
  console.log(content);
}, function (err, count, files) {
  console.log('Read ' + count + ' text file(s)');
});

```

Recursively list all json files in a directory including all sub-directories, without reading the files.

```javascript
var readfiles = require('readfiles');

readfiles('/path/to/dir/', {
  filter: '*.json',
  readContents: false
}, function (err, content, filename) {
  if (err) throw err;
  console.log('File ' + filename);
});

```

## License
MIT licensed (See LICENSE.txt)