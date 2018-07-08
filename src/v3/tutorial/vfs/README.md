# VFS

The Virtual Filesystem (VFS) provides methods to use and manipulate local (or remote) filesystems.

## Usage

To use the Virtual Filesystem, make the provided service:

```javascript
const vfs = core.make('osjs/vfs');

const list = await vfs.readdir('osjs:/');
console.log(list);
```

## Methods

* `readdir(file) => stat[]` - Reads given directory (see [stat](#stat))
* `readfile(file, type) => *type` - Reads given file (see [encoding](#encoding))
* `writefile(file, data) => boolean` - Writes to given file
* `copy(src, dst) => boolean` - Copy given file/directory
* `rename(src, dst) => boolean` - Rename or move given file/directory
* `mkdir(file) => boolean` - Creates given directory
* `unlink(file) => boolean` - Removes given file/directory
* `exists(file) => boolean` - Checks if given path exists
* `stat(file) => stat` - Get the stat of given file/directory (see [stat](#stat))
* `url(file) => string` - Create a URL to resource

## Stat

The VFS responds with file statistics in some cases, containing:

```json
{
  "isDirectory": boolean,
  "isFile": boolean,
  "mime": string,
  "size": integer,
  "path": string,
  "filename": string,
  "stat": {
    /* See https://nodejs.org/api/fs.html#fs_class_fs_stats */
  }
}
```

## Encoding

By default, files are read as `ArrayBuffer`, but you can specify any of these types:

* `string` - A `UTF-8` encoded string
* `uri` - A `base64` encoded resource link
* `blob` - A `Blob`

## Custom Adapter

You can make your own Adapter for the methods above by creating a simple object:

### Client

```javascript
const adapter = (core) => ({
  readdir: (path, options) => Promise.resolve([])
});
```

### Server

```javascript
module.exports = (core) => ({
  readdir: vfs => (path) => Promise.resolve([])
});
```