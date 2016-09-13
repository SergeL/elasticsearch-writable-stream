<a name="ElasticsearchWritable"></a>

## ElasticsearchWritable(client, options)
A simple wrapper around Elasticsearch for bulk writing items

**Kind**: global function  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| client | <code>Elasticsearch.Client</code> |  | Elasticsearch client |
| options | <code>Object</code> |  | Options |
| [options.highWaterMark] | <code>number</code> | <code>16</code> | Number of items to buffer before writing. Also the size of the underlying stream buffer. |
| [options.flushTimeout] | <code>number</code> | <code></code> | Number of ms to flush records after, if highWaterMark hasn't been reached |
| [options.logger] | <code>Object</code> |  | Instance of a logger like bunyan or winston |

