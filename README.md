reflect-copy
===================
A verified copy for Node.js, available across filesystems and networks. It even works for directories.

Master Build Status: 
[![Build Status](https://travis-ci.org/aerisweather/node-reflect-copy.svg?branch=master)](https://travis-ci.org/aerisweather/node-reflect-copy)
[![Coverage Status](https://coveralls.io/repos/aerisweather/node-reflect-copy/badge.svg?branch=master&service=github)](https://coveralls.io/github/aerisweather/node-reflect-copy?branch=master)

__This project is sponsored by:__

[![AerisWeather](http://branding.aerisweather.com/logo-dark-small.png)](http://www.aerisweather.com) - Empowering the next generation, [aerisweather.com](https://www.aerisweather.com)

Our use case deals with a mounted remote file system. We need a way to reliably copy things across the network. Since the remote 
is just a folder, we have limited capabilities, so we read in the file(s), get a hash of them, copy them over, then verify our copy
produces the same hash. Not fool proof, but pretty good at ensuring there weren't network hiccups along the way.

## Quick Example

```javascript
var rCopy = require('reflect-copy').copy;

rCopy('/path/to/source', 'path/to/dest', {}, function(err, results) {
    console.log(results.source); //The source file's hash
    console.log(results.destination); //The destination file's hash
});
```
## Features
We needed a nice copy to clone files across file systems. File operations should be verifiable yet simple. Using checksums we can ensure a file has been copied correctly, in it's entirety. We use the module `fs-extra` (because it's awesome) as a base, then just wrap it with a nice checksum verification method.

* Pre and post checksum comparison
* Locking for file copies
* Copy to temp location first, move into position (useful for network transfers)
* Configuration for common options

## Options

| Option | Type | Default | Description |
| ------ | ---- | ------- | ----------- |
| `clobber` | (boolean) | `false` | Whether to overwrite if this file exists. |
| `srcHash` | (string) | `(empty)` | The known hash of the source file, if known |
| `hashMethod` | (string) | `'md5'` | Which hash to use for the checksum: 'md5' or 'sha1' |
| `tmpSuffix` | (string) | `'.copy-tmp'` | Whether to overwrite if this file exists. |
| `useLock` | (boolean) | `true` | (Locking) Should we use lock files to ensure we are the only reflect-copy writing to this location. |
| `stale` | (int, ms) | `30000` | (Locking) Duration to wait for the lock file before it is deemed stale (in milliseconds) |
| `wait` | (int, ms) | `10000` | (Locking) How long to wait for a lock file before erroring (in milliseconds) |

## Roadmap

 1. Support folders with multiple files
 1. Create a copySync method
