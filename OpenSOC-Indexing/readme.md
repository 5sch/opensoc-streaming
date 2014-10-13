#OpenSOC-Indexing

##Module Description

This module provides the indexing capability to OpenSOC components.  The primary indexing engine for now is Elastic Search, but Solr may be supported at some point in the future as well.  There are three types of messages that are commonly indexed in OpenSOC topologies: messages, alerts, and errors.  Messages are telemetry messages parsed by the parser bolt.  Alerts are alerts generated by the alerts bolt.  Errors are an optional feature where each OpenSOC bolt in addition to outputting errors in the log file will also index them for immediate analysis.

###Index bolt

The signature of the index bolt is as follows:

```
TelemetryIndexingBolt indexing_bolt = new TelemetryIndexingBolt()
.withIndexIP(config.getString("es.ip"))
.withIndexPort(config.getInt("es.port"))
.withClusterName(config.getString("es.clustername"))
.withIndexName(
config.getString("bolt.error.indexing.indexname"))
.withDocumentName(
config.getString("bolt.error.indexing.documentname"))
.withBulk(config.getInt("bolt.error.indexing.bulk"))
.withIndexAdapter(adapter)
.withMetricConfiguration(config);

```

###IndexAdapters

*com.opensoc.indexing.adapters.ESBaseBulkAdapter - bulk ingest messages into Elastic Search
*com.opensoc.indexing.adapters.ESBaseBulkRotatingAdapter - does everything adapter above does, but is able to rotate the index names based on size
*com.opensoc.indexing.adapters.ESTimedBulkRotatingAdapter - does everything adapter above does, but is able to rotate the index names based on size and time
*com.opensoc.indexing.adapters.SolrAdapter - currently under development