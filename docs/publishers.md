# Using Elasticsearch Publisher

PerfKit data can optionally be published to an Elasticsearch server. To enable
this, the `elasticsearch` Python package must be installed.

```bash
$ pip install elasticsearch
```

Note: The `elasticsearch` Python library and Elasticsearch must have matching
major versions.

The following are flags used by the Elasticsearch publisher. At minimum, all
that is needed is the `--es_uri` flag.

| Flag         | Notes                                                     |
| ------------ | --------------------------------------------------------- |
| `--es_uri`   | The Elasticsearch server address and port (e.g.           |
:              : localhost\:9200)                                          :
| `--es_index` | The Elasticsearch index name to store documents (default: |
:              : perfkit)                                                  :
| `--es_type`  | The Elasticsearch document type (default: result)         |

Note: Amazon ElasticSearch service currently does not support transport on port
9200 therefore you must use endpoint with port 80 eg.
`search-<ID>.es.amazonaws.com:80` and allow your IP address in the cluster.

# Using InfluxDB Publisher

No additional packages need to be installed in order to publish Perfkit data to
an InfluxDB server.

InfluxDB Publisher takes in the flags for the Influx uri and the Influx DB name.
The publisher will default to the pre-set defaults, identified below, if no uri
or DB name is set. However, the user is required to at the very least call the
`--influx_uri` flag to publish data to Influx.

| Flag               | Notes                               | Default        |
| ------------------ | ----------------------------------- | -------------- |
| `--influx_uri`     | The Influx DB address and port.     | localhost:8086 |
:                    : Expects the format hostname\:port   :                :
| `--influx_db_name` | The name of Influx DB database that | perfkit        |
:                    : you wish to publish to or create    :                :

