# compression

[![NPM Version][npm-image]][npm-url]
[![NPM Downloads][downloads-image]][downloads-url]
[![Build Status][travis-image]][travis-url]
[![Test Coverage][coveralls-image]][coveralls-url]
[![Gratipay][gratipay-image]][gratipay-url]

Node.js compression middleware.

  ```node.js压缩程序```

The following compression codings are supported:

  ```支持下列压缩编码```
  - deflate
  - gzip

## Install

This is a [Node.js](https://nodejs.org/en/) module available through the
[npm registry](https://www.npmjs.com/). 

```这是一个可以通过npm来注册的模块```

Installation is done using the
[`npm install` command](https://docs.npmjs.com/getting-started/installing-npm-packages-locally):

```使用以下安装命令完成安装```

```bash
$ npm install compression
```

## API

<!-- eslint-disable no-unused-vars -->

```js
var compression = require('compression')
```

### compression([options])

```压缩```

Returns the compression middleware using the given `options`.

```使用给定选项返回压缩中间件```

The middleware
will attempt to compress response bodies for all request that traverse through
the middleware, based on the given `options`.

```中间件将尝试根据给定的选项压缩贯穿中间件的所有请求的响应主体。```

This middleware will never compress responses that include a `Cache-Control`
header with the [`no-transform` directive](https://tools.ietf.org/html/rfc7234#section-5.2.2.4),
as compressing will transform the body.

```这个中间件永远不会压缩包含一个无转换指令的缓存控制头的响应，因为压缩将改变主体。```

#### Options

```选项```

`compression()` accepts these properties in the options object. In addition to
those listed below, [zlib](http://nodejs.org/api/zlib.html) options may be
passed in to the options object.

```compression()接受这些属性选项对象。除了下面列出的选项，zlib可能通过在选项对象。```

##### chunkSize

```分片```

The default value is `zlib.Z_DEFAULT_CHUNK`, or `16384`.

```默认值是zlib.z_default_chunk，或16384。```

See [Node.js documentation](http://nodejs.org/api/zlib.html#zlib_memory_usage_tuning)
regarding the usage.

##### filter

```过滤器```

A function to decide if the response should be considered for compression.

```决定是否应考虑压缩响应的函数。```

This function is called as `filter(req, res)` and is expected to return
`true` to consider the response for compression, or `false` to not compress
the response.

```这个功能被称为过滤器（REQ，RES）和预计回报真正考虑压缩响应，或假不压缩的响应。```

The default filter function uses the [compressible](https://www.npmjs.com/package/compressible)
module to determine if `res.getHeader('Content-Type')` is compressible.

```默认的过滤功能，采用可压缩模块定义如果res.getheader（'content-type”）是可压缩的。```

##### level

```压缩等级```

The level of zlib compression to apply to responses.

```压缩等级用于反映压缩效果。```

A higher level will result in better compression, but will take longer to complete.

```更高的级别将导致更好的压缩，但需要更长的时间才能完成```

A lower level willresult in less compression, but will be much faster.

```一个较低的水平将导致更少的压缩，但会快得多。```

This is an integer in the range of `0` (no compression) to `9` (maximum compression). 

```这是一个0（没有压缩）到9（最大压缩）的整数。```

The special value `-1` can be used to mean the "default
compression level", which is a default compromise between speed and
compression (currently equivalent to level 6).

```特殊值1可用于表示“默认压缩级别”，这是速度和压缩之间的默认折衷（目前相当于6级）。```

  - `-1` Default compression level (also `zlib.Z_DEFAULT_COMPRESSION`).
  - `0` No compression (also `zlib.Z_NO_COMPRESSION`).
  - `1` Fastest compression (also `zlib.Z_BEST_SPEED`).
  - `2`
  - `3`
  - `4`
  - `5`
  - `6` (currently what `zlib.Z_DEFAULT_COMPRESSION` points to).
  - `7`
  - `8`
  - `9` Best compression (also `zlib.Z_BEST_COMPRESSION`).

The default value is `zlib.Z_DEFAULT_COMPRESSION`, or `-1`.

**Note** in the list above, `zlib` is from `zlib = require('zlib')`.

##### memLevel

```MEM水平  mem是DOS命令：显示随机存取存贮器的分配信息```

This specifies how much memory should be allocated for the internal compression
state and is an integer in the range of `1` (minimum level) and `9` (maximum
level).

```
这指定了内部压缩状态应该分配多少内存，并且在1（最小级别）和9（最大级别）范围内是一个整数。
```

The default value is `zlib.Z_DEFAULT_MEMLEVEL`, or `8`.

```
默认值是zlib.z_default_memlevel，或8。
```

See [Node.js documentation](http://nodejs.org/api/zlib.html#zlib_memory_usage_tuning)
regarding the usage.

##### strategy

```策略```

This is used to tune the compression algorithm. This value only affects the
compression ratio, not the correctness of the compressed output, even if it
is not set appropriately.

```
这个用来调整压缩算法。这个值只会影响压缩比。不会影响输出的正确性。
从上面可以看出，zlib来自于zlib=require('zlib');
```

  - `zlib.Z_DEFAULT_STRATEGY` Use for normal data.
  - `zlib.Z_FILTERED` Use for data produced by a filter (or predictor).
    Filtered data consists mostly of small values with a somewhat random
    distribution. In this case, the compression algorithm is tuned to
    compress them better. The effect is to force more Huffman coding and less
    string matching; it is somewhat intermediate between `zlib.Z_DEFAULT_STRATEGY`
    and `zlib.Z_HUFFMAN_ONLY`.
  - `zlib.Z_FIXED` Use to prevent the use of dynamic Huffman codes, allowing
    for a simpler decoder for special applications.
  - `zlib.Z_HUFFMAN_ONLY` Use to force Huffman encoding only (no string match).
  - `zlib.Z_RLE` Use to limit match distances to one (run-length encoding).
    This is designed to be almost as fast as `zlib.Z_HUFFMAN_ONLY`, but give
    better compression for PNG image data.

**Note** in the list above, `zlib` is from `zlib = require('zlib')`.

##### threshold

```阈值```

The byte threshold for the response body size before compression is considered
for the response, defaults to `1kb`. This is a number of bytes, any string
accepted by the [bytes](https://www.npmjs.com/package/bytes) module, or `false`.

```
字节阈值响应体的大小压缩前是响应，默认值为1KB。这是字节数，字节模块接受的任何字符串，或false。
```

**Note** this is only an advisory setting; if the response size cannot be determined
at the time the response headers are written, then it is assumed the response is
_over_ the threshold. To guarantee the response size can be determined, be sure
set a `Content-Length` response header.

```
注意，这只是一个咨询设置；如果在响应标头写入时不能确定响应大小，则假定响应超过阈值。为了确保响应大小可以确定，请务必设置一个内容长度响应头。
```

##### windowBits

```窗位```

The default value is `zlib.Z_DEFAULT_WINDOWBITS`, or `15`.

```默认值是zlib.z_default_windowbits，或15。```

See [Node.js documentation](http://nodejs.org/api/zlib.html#zlib_memory_usage_tuning)
regarding the usage.

#### .filter

```过滤器```

The default `filter` function. This is used to construct a custom filter
function that is an extension of the default function.

```默认的过滤功能。这用于构造自定义筛选器函数，该函数是默认函数的扩展名。```

```js
var compression = require('compression')
var express = require('express')

var app = express()
app.use(compression({filter: shouldCompress}))

function shouldCompress (req, res) {
  if (req.headers['x-no-compression']) {
    // don't compress responses with this request header
    return false
  }

  // fallback to standard filter function
  return compression.filter(req, res)
}
```

### res.flush

This module adds a `res.flush()` method to force the partially-compressed
response to be flushed to the client.

```这个模块增加了一个部分压缩的res.flush()方法。把部分压缩的功能提供给顾客。
```

## Examples

### express/connect

When using this module with express or connect, simply `app.use` the module as
high as you like. Requests that pass through the middleware will be compressed.

```当使用这个模块进行连接的时候，这个简单的app.use模块就会像你一样高。通过中
间件的请求将被压缩。
```

```js
var compression = require('compression')
var express = require('express')

var app = express()

// compress all responses
app.use(compression())

// add all routes
```

### Server-Sent Events

```服务器发送事件```

Because of the nature of compression this module does not work out of the box
with server-sent events. To compress content, a window of the output needs to
be buffered up in order to get good compression. Typically when using server-sent
events, there are certain block of data that need to reach the client.

```由于压缩的性质，此模块不能在服务器发送事件的框中运行。为了压缩内容，需要对

输出窗口进行缓冲，以获得良好的压缩。通常，当使用服务器发送的事件时，需要到

达客户机的某些数据块。```

You can achieve this by calling `res.flush()` when you need the data written to
actually make it to the client.

```当你需要把数据写到客户机的时候，你可以调用res.flush()这个方法。```

```js
var compression = require('compression')
var express = require('express')

var app = express()

// compress responses
app.use(compression())

// server-sent event stream
app.get('/events', function (req, res) {
  res.setHeader('Content-Type', 'text/event-stream')
  res.setHeader('Cache-Control', 'no-cache')

  // send a ping approx every 2 seconds
  var timer = setInterval(function () {
    res.write('data: ping\n\n')

    // !!! this is the important part
    res.flush()
  }, 2000)

  res.on('close', function () {
    clearInterval(timer)
  })
})
```

## License

[MIT](LICENSE)

[npm-image]: https://img.shields.io/npm/v/compression.svg
[npm-url]: https://npmjs.org/package/compression
[travis-image]: https://img.shields.io/travis/expressjs/compression/master.svg
[travis-url]: https://travis-ci.org/expressjs/compression
[coveralls-image]: https://img.shields.io/coveralls/expressjs/compression/master.svg
[coveralls-url]: https://coveralls.io/r/expressjs/compression?branch=master
[downloads-image]: https://img.shields.io/npm/dm/compression.svg
[downloads-url]: https://npmjs.org/package/compression
[gratipay-image]: https://img.shields.io/gratipay/dougwilson.svg
[gratipay-url]: https://www.gratipay.com/dougwilson/
