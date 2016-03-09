# elasticsearch-bulk-index-stream

A writable stream for bulk indexing records in Elasticsearch

[![build status](https://travis-ci.org/voldern/elasticsearch-bulk-index-stream.svg)](https://travis-ci.org/voldern/elasticsearch-bulk-index-stream)
[![modules status](https://david-dm.org/voldern/elasticsearch-bulk-index-stream.svg)](https://david-dm.org/voldern/elasticsearch-bulk-index-stream)

[![npm badge](https://nodei.co/npm/elasticsearch-bulk-index-stream.png?downloads=true)](https://nodei.co/npm/elasticsearch-bulk-index-stream)

# Usage

## Format

The records written to the stream has to have the following format:
```javascript
{
  index: 'name-of-index',
  type: 'recordType',
  id: 'recordId',
  body: {
    name: 'Foo Bar'
  }
}
```

## Buffering

The `highWaterMark` option set on the stream defines how many items
will be buffered before doing a bulk indexing operation. The stream
will also write all buffered items if its is closed, before emitting
the `finish` event.

## Flushing

Its also possible to send in the option `flushTimeout` to indicate
that the items currently in the buffer should be flushed after the
given amount of milliseconds if the `highWaterMark` haven't been
reached.

## Logging

A [bunyan](https://www.npmjs.com/package/bunyan),
[winston](https://www.npmjs.com/package/winston) or similar logger
instance that have methods like `debug`, `error` and `info` may be
sent in as `options.logger` to the constructor.

# Example

```javascript
var ElasticsearchBulkIndexStream = require('elasticsearch-bulk-index-stream');

var stream = new ElasticsearchBulkIndexStream(elasticsearchClient, {
  highWaterMark: 256,
  flushTimeout: 500
});

someInputStream
  .pipe(stream)
  .on('error', function(error) {
    // Handle error
  })
  .on('finish', function() {
    // Clean up Elasticsearch client?
  })
```

# API

See [api.md](api.md).

# See

- [elasticsearch-streams](https://www.npmjs.com/package/elasticsearch-streams)

Elasticsearch readable and writable streams. The main difference
between the bulk writer in `elasticsearch-streams` and this library is
that this library requires the `index` and `type` of the data being
written to exist in the record instead of being set in a callback when
the records written.

`elasticsearch-streams` also implements its own event named `close` to
indicate that all the data has been written to Elasticsearch. This
will break modules like [pump](https://www.npmjs.com/package/pump)
that depend on the `finish` event.

# License

MIT
