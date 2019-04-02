# MapStream2

[![npm (scoped)](https://img.shields.io/npm/v/map-stream2.svg)](https://github.com/MGardner02/map-stream)
[![npm bundle size (minified)](https://img.shields.io/bundlephobia/min/map-stream2.svg)](https://github.com/MGardner02/map-stream)

This package is a fork of the original map-stream which can be found here: http://github.com/dominictarr/map-stream.
The map-stream package is an incredibly useful tool, but the recursion used to write the stream's queue can become an issue for very high volume data streams. This package converts the recursive method to using a loop which eliminates any concerns with overloading the call stack. Aside from this change behind the scenes MapStream and MapStream2 operate the same. 

Substantial credit should go to Dominic Tarr for creating the original MapStream.

##map (asyncFunction[, options])

Create a through stream from an asyncronous function.  

``` js
var map = require('map-stream')

map(function (data, callback) {
  //transform data
  // ...
  callback(null, data)
})

```

Each map MUST call the callback. It may callback with data, with an error or with no arguments, 

  * `callback()` drop this data.  
    this makes the map work like `filter`,  
    note:`callback(null,null)` is not the same, and will emit `null`

  * `callback(null, newData)` turn data into newData
    
  * `callback(error)` emit an error for this item.

>Note: if a callback is not called, `map` will think that it is still being processed,   
>every call must be answered or the stream will not know when to end.  
>
>Also, if the callback is called more than once, every call but the first will be ignored.

##Options 

 * `failures` - `boolean` continue mapping even if error occured. On error `map-stream` will emit `failure` event. (default: `false`)
