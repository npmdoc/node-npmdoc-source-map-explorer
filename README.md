# api documentation for  [source-map-explorer (v1.3.3)](https://github.com/danvk/source-map-explorer#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-source-map-explorer.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-source-map-explorer) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-source-map-explorer.svg)](https://travis-ci.org/npmdoc/node-npmdoc-source-map-explorer)
#### Analyze and debug space usage through source maps

[![NPM](https://nodei.co/npm/source-map-explorer.png?downloads=true)](https://www.npmjs.com/package/source-map-explorer)

[![apidoc](https://npmdoc.github.io/node-npmdoc-source-map-explorer/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-source-map-explorer_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-source-map-explorer/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-source-map-explorer/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-source-map-explorer/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Dan Vanderkam",
        "url": "danvdk@gmail.com"
    },
    "bin": {
        "source-map-explorer": "index.js"
    },
    "bugs": {
        "url": "https://github.com/danvk/source-map-explorer/issues"
    },
    "dependencies": {
        "btoa": "^1.1.2",
        "convert-source-map": "^1.1.1",
        "docopt": "^0.6.2",
        "file-url": "^1.0.1",
        "open": "0.0.5",
        "source-map": "^0.5.1",
        "temp": "^0.8.3",
        "underscore": "^1.8.3"
    },
    "description": "Analyze and debug space usage through source maps",
    "devDependencies": {
        "chai": "^3.3.0",
        "mocha": "^2.3.3"
    },
    "directories": {},
    "dist": {
        "shasum": "ff15cb038465fb652e57fa54ecd103f51e01bc43",
        "tarball": "https://registry.npmjs.org/source-map-explorer/-/source-map-explorer-1.3.3.tgz"
    },
    "gitHead": "e7397ed56a78568ae42062b62e4a84508c17e0dd",
    "homepage": "https://github.com/danvk/source-map-explorer#readme",
    "keywords": [
        "source-map",
        "browser",
        "minification"
    ],
    "license": "Apache-2.0",
    "main": "index.js",
    "maintainers": [
        {
            "name": "danvk",
            "email": "danvdk@gmail.com"
        }
    ],
    "name": "source-map-explorer",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/danvk/source-map-explorer.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "1.3.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module source-map-explorer](#apidoc.module.source-map-explorer)
1.  [function <span class="apidocSignatureSpan">source-map-explorer.</span>adjustSourcePaths (sizes, findRoot, finds, replaces)](#apidoc.element.source-map-explorer.adjustSourcePaths)
1.  [function <span class="apidocSignatureSpan">source-map-explorer.</span>commonPathPrefix (array)](#apidoc.element.source-map-explorer.commonPathPrefix)
1.  [function <span class="apidocSignatureSpan">source-map-explorer.</span>computeGeneratedFileSizes (mapConsumer, generatedJs)](#apidoc.element.source-map-explorer.computeGeneratedFileSizes)
1.  [function <span class="apidocSignatureSpan">source-map-explorer.</span>loadSourceMap (jsFile, mapFile)](#apidoc.element.source-map-explorer.loadSourceMap)
1.  [function <span class="apidocSignatureSpan">source-map-explorer.</span>mapKeys (obj, fn)](#apidoc.element.source-map-explorer.mapKeys)



# <a name="apidoc.module.source-map-explorer"></a>[module source-map-explorer](#apidoc.module.source-map-explorer)

#### <a name="apidoc.element.source-map-explorer.adjustSourcePaths"></a>[function <span class="apidocSignatureSpan">source-map-explorer.</span>adjustSourcePaths (sizes, findRoot, finds, replaces)](#apidoc.element.source-map-explorer.adjustSourcePaths)
- description and source-code
```javascript
function adjustSourcePaths(sizes, findRoot, finds, replaces) {
  if (findRoot) {
    var prefix = commonPathPrefix(_.keys(sizes));
    var len = prefix.length;
    if (len) {
      sizes = mapKeys(sizes, function(source) { return source.slice(len); })
    }
  }

  for (var i = 0; i < finds.length; i++) {
    var before = new RegExp(finds[i]),
        after = replaces[i];
    sizes = mapKeys(sizes, function(source) {
      return source.replace(before, after);
    });
  }

  return sizes;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.source-map-explorer.commonPathPrefix"></a>[function <span class="apidocSignatureSpan">source-map-explorer.</span>commonPathPrefix (array)](#apidoc.element.source-map-explorer.commonPathPrefix)
- description and source-code
```javascript
function commonPathPrefix(array){
  if (array.length == 0) return '';
  var A= array.concat().sort(),
  a1= A[0].split(/(\/)/), a2= A[A.length-1].split(/(\/)/), L= a1.length, i= 0;
  while(i<L && a1[i] === a2[i]) i++;
  return a1.slice(0, i).join('');
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.source-map-explorer.computeGeneratedFileSizes"></a>[function <span class="apidocSignatureSpan">source-map-explorer.</span>computeGeneratedFileSizes (mapConsumer, generatedJs)](#apidoc.element.source-map-explorer.computeGeneratedFileSizes)
- description and source-code
```javascript
function computeGeneratedFileSizes(mapConsumer, generatedJs) {
  var lines = generatedJs.split('\n');
  var sourceExtrema = {};  // source -> {min: num, max: num}
  var numChars = 0;
  var lastSource = null;
  for (var line = 1; line <= lines.length; line++) {
    var lineText = lines[line - 1];
    var numCols = lineText.length;
    for (var column = 0; column < numCols; column++, numChars++) {
      var pos = mapConsumer.originalPositionFor({line:line, column:column});
      var source = pos.source;
      if (source == null) {
        // Often this is from the '// #sourceMap' comment itself.
        continue;
      }

      if (source != lastSource) {
        if (!(source in sourceExtrema)) {
          sourceExtrema[source] = {min: numChars};
          lastSource = source;
        } else {
          // source-map reports odd positions for bits between files.
        }
      } else {
        sourceExtrema[source].max = numChars;
      }
    }
  }
  return _.mapObject(sourceExtrema, function(v) {
    return v.max - v.min + 1
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.source-map-explorer.loadSourceMap"></a>[function <span class="apidocSignatureSpan">source-map-explorer.</span>loadSourceMap (jsFile, mapFile)](#apidoc.element.source-map-explorer.loadSourceMap)
- description and source-code
```javascript
function loadSourceMap(jsFile, mapFile) {
  var jsData = fs.readFileSync(jsFile).toString();

  var mapConsumer;
  if (mapFile) {
    var sourcemapData = fs.readFileSync(mapFile).toString();
    mapConsumer = new sourcemap.SourceMapConsumer(sourcemapData);
  } else {
    // Try to read a source map from a 'sourceMappingURL' comment.
    var converter = convert.fromSource(jsData);
    if (!converter) {
      converter = convert.fromMapFileSource(jsData, path.dirname(jsFile));
    }
    if (!converter) {
      console.error('Unable to find a source map.');
      console.error('See ', SOURCE_MAP_INFO_URL);
      return null;
    }
    mapConsumer = new sourcemap.SourceMapConsumer(converter.toJSON());
  }

  if (!mapConsumer) {
    console.error('Unable to find a source map.');
    console.error('See ', SOURCE_MAP_INFO_URL);
    return null;
  }

  return {
    mapConsumer: mapConsumer,
    jsData: jsData
  };
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.source-map-explorer.mapKeys"></a>[function <span class="apidocSignatureSpan">source-map-explorer.</span>mapKeys (obj, fn)](#apidoc.element.source-map-explorer.mapKeys)
- description and source-code
```javascript
function mapKeys(obj, fn) {
  return _.object(_.map(obj, function(v, k) { return [fn(k), v]; }));
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
