# api documentation for  [grunt-newer (v1.2.0)](https://github.com/tschaub/grunt-newer)  [![npm package](https://img.shields.io/npm/v/npmdoc-grunt-newer.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-grunt-newer) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-grunt-newer.svg)](https://travis-ci.org/npmdoc/node-npmdoc-grunt-newer)
#### Run Grunt tasks with only those source files modified since the last successful run.

[![NPM](https://nodei.co/npm/grunt-newer.png?downloads=true)](https://www.npmjs.com/package/grunt-newer)

[![apidoc](https://npmdoc.github.io/node-npmdoc-grunt-newer/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-grunt-newer_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-grunt-newer/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-grunt-newer/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-grunt-newer/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Tim Schaub",
        "url": "http://tschaub.net/"
    },
    "bugs": {
        "url": "https://github.com/tschaub/grunt-newer/issues"
    },
    "dependencies": {
        "async": "^1.5.2",
        "rimraf": "^2.5.2"
    },
    "description": "Run Grunt tasks with only those source files modified since the last successful run.",
    "devDependencies": {
        "chai": "^3.5.0",
        "grunt": "0.4.5",
        "grunt-cafe-mocha": "0.1.13",
        "grunt-cli": "^1.1.0",
        "grunt-contrib-clean": "^1.0.0",
        "grunt-contrib-jshint": "^1.0.0",
        "grunt-contrib-watch": "^1.0.0",
        "mock-fs": "^3.8.0",
        "tmp": "0.0.28",
        "wrench": "1.5.8"
    },
    "directories": {},
    "dist": {
        "shasum": "77f81e6afb7a82b3f91c14137361998d58866841",
        "tarball": "https://registry.npmjs.org/grunt-newer/-/grunt-newer-1.2.0.tgz"
    },
    "engines": {
        "node": ">= 0.8.0"
    },
    "gitHead": "e0f22968871907052db6a7669937fe5b57ca48e2",
    "homepage": "https://github.com/tschaub/grunt-newer",
    "keywords": [
        "files",
        "grunt",
        "gruntplugin",
        "newer"
    ],
    "license": "MIT",
    "main": "gruntfile.js",
    "maintainers": [
        {
            "name": "tschaub",
            "email": "tim.schaub@gmail.com"
        }
    ],
    "name": "grunt-newer",
    "optionalDependencies": {},
    "peerDependencies": {
        "grunt": ">=0.4.1"
    },
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/tschaub/grunt-newer.git"
    },
    "scripts": {
        "start": "grunt test watch",
        "test": "grunt test"
    },
    "version": "1.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module grunt-newer](#apidoc.module.grunt-newer)
1.  object <span class="apidocSignatureSpan">grunt-newer.</span>util

#### [module grunt-newer.util](#apidoc.module.grunt-newer.util)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>anyNewer (paths, time, tolerance, override, callback)](#apidoc.element.grunt-newer.util.anyNewer)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterFilesByHash (files, taskName, targetName, callback)](#apidoc.element.grunt-newer.util.filterFilesByHash)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterFilesByTime (files, previous, tolerance, override, callback)](#apidoc.element.grunt-newer.util.filterFilesByTime)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterPathsByHash (paths, cacheDir, taskName, targetName, callback)](#apidoc.element.grunt-newer.util.filterPathsByHash)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterPathsByTime (paths, time, tolerance, override, callback)](#apidoc.element.grunt-newer.util.filterPathsByTime)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>generateFileHash (filePath, callback)](#apidoc.element.grunt-newer.util.generateFileHash)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>getExistingHash (filePath, cacheDir, taskName, targetName, callback)](#apidoc.element.grunt-newer.util.getExistingHash)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>getHashPath (cacheDir, taskName, targetName, filePath)](#apidoc.element.grunt-newer.util.getHashPath)
1.  [function <span class="apidocSignatureSpan">grunt-newer.util.</span>getStampPath (cacheDir, taskName, targetName)](#apidoc.element.grunt-newer.util.getStampPath)



# <a name="apidoc.module.grunt-newer"></a>[module grunt-newer](#apidoc.module.grunt-newer)



# <a name="apidoc.module.grunt-newer.util"></a>[module grunt-newer.util](#apidoc.module.grunt-newer.util)

#### <a name="apidoc.element.grunt-newer.util.anyNewer"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>anyNewer (paths, time, tolerance, override, callback)](#apidoc.element.grunt-newer.util.anyNewer)
- description and source-code
```javascript
anyNewer = function (paths, time, tolerance, override, callback) {
  if (paths.length === 0) {
    process.nextTick(function() {
      callback(null, false);
    });
    return;
  }
  var complete = 0;
  function iterate() {
    fs.stat(paths[complete], function(err, stats) {
      if (err) {
        return callback(err);
      }

      var pathTime = stats.mtime.getTime();
      var comparisonTime = time.getTime();
      var difference = pathTime - comparisonTime;

      if (difference > tolerance) {
        return callback(null, true);
      } else {
        override(paths[complete], time, function(include) {
          if (include) {
            callback(null, true);
          } else {
            ++complete;
            if (complete >= paths.length) {
              return callback(null, false);
            }
            iterate();
          }
        });
      }
    });
  }
  iterate();
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.filterFilesByHash"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterFilesByHash (files, taskName, targetName, callback)](#apidoc.element.grunt-newer.util.filterFilesByHash)
- description and source-code
```javascript
filterFilesByHash = function (files, taskName, targetName, callback) {
  var modified = false;
  async.map(files, function(obj, done) {

    filterPathsByHash(obj.src, taskName, targetName, function(err, src) {
      if (err) {
        return done(err);
      }
      if (src.length) {
        modified = true;
      }
      done(null, {src: src, dest: obj.dest});
    });

  }, function(err, newerFiles) {
    callback(err, newerFiles, modified);
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.filterFilesByTime"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterFilesByTime (files, previous, tolerance, override, callback)](#apidoc.element.grunt-newer.util.filterFilesByTime)
- description and source-code
```javascript
filterFilesByTime = function (files, previous, tolerance, override, callback) {
  async.map(files, function(obj, done) {
    if (obj.dest && !(obj.src.length === 1 && obj.dest === obj.src[0])) {
      fs.stat(obj.dest, function(err, stats) {
        if (err) {
          // dest file not yet created, use all src files
          return done(null, obj);
        }
        return anyNewer(
          obj.src, stats.mtime, tolerance, override, function(err, any) {
          done(err, any && obj);
        });
      });
    } else {
      filterPathsByTime(
          obj.src, previous, tolerance, override, function(err, src) {
        if (err) {
          return done(err);
        }
        done(null, {src: src, dest: obj.dest});
      });
    }
  }, function(err, results) {
    if (err) {
      return callback(err);
    }
    // get rid of file config objects with no src files
    callback(null, results.filter(function(obj) {
      return obj && obj.src && obj.src.length > 0;
    }));
  });
}
```
- example usage
```shell
...
    path: filePath,
    time: time
  };
  options.override(details, include);
}

var files = grunt.task.normalizeMultiTaskFiles(config, targetName);
util.filterFilesByTime(
  files, previous, options.tolerance, override, function(e, newerFiles) {
  if (e) {
    return done(e);
  } else if (newerFiles.length === 0) {
    grunt.log.writeln('No newer files to process.');
    return done();
  }
...
```

#### <a name="apidoc.element.grunt-newer.util.filterPathsByHash"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterPathsByHash (paths, cacheDir, taskName, targetName, callback)](#apidoc.element.grunt-newer.util.filterPathsByHash)
- description and source-code
```javascript
filterPathsByHash = function (paths, cacheDir, taskName, targetName, callback) {
  async.filter(paths, function(filePath, done) {
    async.parallel({
      previous: function(cb) {
        getExistingHash(filePath, cacheDir, taskName, targetName, cb);
      },
      current: function(cb) {
        generateFileHash(filePath, cb);
      }
    }, function(err, hashes) {
      if (err) {
        return callback(err);
      }
      done(String(hashes.previous) !== String(hashes.current));
    });
  }, callback);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.filterPathsByTime"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>filterPathsByTime (paths, time, tolerance, override, callback)](#apidoc.element.grunt-newer.util.filterPathsByTime)
- description and source-code
```javascript
filterPathsByTime = function (paths, time, tolerance, override, callback) {
  async.map(paths, fs.stat, function(err, stats) {
    if (err) {
      return callback(err);
    }

    var olderPaths = [];
    var newerPaths = paths.filter(function(filePath, index) {
      var newer = stats[index].mtime - time > tolerance;
      if (!newer) {
        olderPaths.push(filePath);
      }
      return newer;
    });

    async.filter(olderPaths, function(filePath, include) {
      override(filePath, time, include);
    }, function(overrides) {
      callback(null, newerPaths.concat(overrides));
    });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.generateFileHash"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>generateFileHash (filePath, callback)](#apidoc.element.grunt-newer.util.generateFileHash)
- description and source-code
```javascript
generateFileHash = function (filePath, callback) {
  var md5sum = crypto.createHash('md5');
  var stream = new fs.ReadStream(filePath);
  stream.on('data', function(chunk) {
    md5sum.update(chunk);
  });
  stream.on('error', callback);
  stream.on('end', function() {
    callback(null, md5sum.digest('hex'));
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.getExistingHash"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>getExistingHash (filePath, cacheDir, taskName, targetName, callback)](#apidoc.element.grunt-newer.util.getExistingHash)
- description and source-code
```javascript
getExistingHash = function (filePath, cacheDir, taskName, targetName, callback) {
  var hashPath = getHashPath(cacheDir, taskName, targetName, filePath);
  fs.exists(hashPath, function(exists) {
    if (!exists) {
      return callback(null, null);
    }
    fs.readFile(hashPath, callback);
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.getHashPath"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>getHashPath (cacheDir, taskName, targetName, filePath)](#apidoc.element.grunt-newer.util.getHashPath)
- description and source-code
```javascript
getHashPath = function (cacheDir, taskName, targetName, filePath) {
  var hashedName = crypto.createHash('md5').update(filePath).digest('hex');
  return path.join(cacheDir, taskName, targetName, 'hashes', hashedName);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-newer.util.getStampPath"></a>[function <span class="apidocSignatureSpan">grunt-newer.util.</span>getStampPath (cacheDir, taskName, targetName)](#apidoc.element.grunt-newer.util.getStampPath)
- description and source-code
```javascript
getStampPath = function (cacheDir, taskName, targetName) {
  return path.join(cacheDir, taskName, targetName, 'timestamp');
}
```
- example usage
```shell
...
} else if (Array.isArray(config.files) &&
    typeof config.files[0] === 'string') {
  config.src = config.files;
  delete config.files;
  srcFiles = false;
}

var stamp = util.getStampPath(options.cache, taskName, targetName);
var previous;
try {
  previous = fs.statSync(stamp).mtime;
} catch (err) {
  // task has never succeeded before
  previous = new Date(0);
}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
