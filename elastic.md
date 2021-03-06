---
layout: default
title: elastic
---



Welcome to the `elastic` screencast!

Follow along below in your favorite R client. I'll be using the RStudio IDE.

<iframe src="//player.vimeo.com/video/124659179" width="600" height="381" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

elastic screencast
==========

## Intro

> Hi, I'm Scott Chamberlain

> In this screencast, I'll do a brief intoduction to the `elastic` package - an `R` client for [Elasticsearch](https://www.elastic.co/products/elasticsearch)

> You can install elastic from [CRAN](http://cran.rstudio.com/web/packages/elastic), or install the [development version from GitHub](https://github.com/ropensci/elastic).

> `elastic` allows you to interact with any Elasticsearch installation

> What is Elasticsearch? Elasticsearch is a powerful database, with a built in HTTP
API for easy integration (like in this package), a powerful query engine, and easy
data input b/c you don't have to specify a schema manaully beforehand

> I'll walk you through a variety of things you can do with the elastic R package

## Installation and load


```r
install.packages("elastic")
```


```r
library("elastic")
```

## Install and start Elasticsearch

* [Elasticsearch installation help](http://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html)
* Navigate to elasticsearch: `cd /usr/local/elasticsearch`
* Start elasticsearch: `bin/elasticsearch`


## Security

> Out of the box, Elasticsearch is not secure. Be careful on public IP addresses.

## Initialize connection

> `connect()` is used before doing anything else to set the connection details to your remote or local elasticsearch store


```r
connect()
#> uri:       http://127.0.0.1 
#> port:      9200 
#> username:  NULL 
#> password:  NULL 
#> elasticsearch details:   
#>    status:                  200 
#>    name:                    Chamber 
#>    Elasticsearch version:   1.5.1 
#>    ES version timestamp:    2015-04-09T13:41:35Z 
#>    lucene version:          4.10.4
```

> On package load, base url and port set to `http://127.0.0.1` and `9200`
If you don't need a port, set the port to NULL.

> Authentication: for now basic authentication is supported in the `connect()`
function.

## Elasticsearch management

### Cat

> Get list of cat endpoints


```r
cat_()
#> =^.^=
#> /_cat/allocation
#> /_cat/shards
#> /_cat/shards/{index}
#> /_cat/master
#> /_cat/nodes
#> /_cat/indices
#> /_cat/indices/{index}
#> /_cat/segments
#> /_cat/segments/{index}
#> /_cat/count
#> /_cat/count/{index}
#> /_cat/recovery
#> /_cat/recovery/{index}
#> /_cat/health
#> /_cat/pending_tasks
#> /_cat/aliases
#> /_cat/aliases/{alias}
#> /_cat/thread_pool
#> /_cat/plugins
#> /_cat/fielddata
#> /_cat/fielddata/{fields}
```

> Get list of indices


```r
cat_indices()
#> yellow open flowers               5 1     150     0  43.6kb  43.6kb 
#> yellow open stuff_m               5 1       0     0    575b    575b 
#> yellow open flights               5 1  156766     0  21.3mb  21.3mb 
#> yellow open asdfdf                5 1       0     0    575b    575b 
#> yellow open things2               5 1       0     0    575b    575b 
#> yellow open twitter               5 1       0     0    575b    575b 
#> yellow open -----                 5 1       0     0    575b    575b 
#> yellow open afjaljfalsfjalksdfadf 5 1       0     0    575b    575b 
#> yellow open arrests               5 1      50     0  23.9kb  23.9kb 
#> yellow open animals               5 1       2     0   3.1kb   3.1kb 
#> yellow open diam                  5 1   53940     0   6.6mb   6.6mb 
#> yellow open logstash-2018.02.28   5 1       0     0    575b    575b 
#> yellow open stuff                 5 1       0     0    575b    575b 
#> yellow open gbif                  5 1     899     0     1mb     1mb 
#> yellow open stuff_y               5 1       0     0    575b    575b 
#> yellow open gbifnewgeo            5 1       2     0   5.8kb   5.8kb 
#> yellow open geoshape              5 1     222     0  42.9kb  42.9kb 
#> yellow open stuff_x               5 1       0     0    575b    575b 
#> yellow open afjaljfalsfjalksdf    5 1       0     0    575b    575b 
#> yellow open stuff_q               5 1       0     0    575b    575b 
#> yellow open diamfromlist          5 1   53940     0     6mb     6mb 
#> yellow open test_close_open       5 1       0     0    575b    575b 
#> yellow open stuff_d               5 1       0     0    575b    575b 
#> yellow open plos                  5 1    1000     0 413.4kb 413.4kb 
#> yellow open stuff_h               5 1       0     0    575b    575b 
#> yellow open stuff_c               5 1       0     0    575b    575b 
#> yellow open stuff_s               5 1       0     0    575b    575b 
#> yellow open diamonds              5 1    7500     0     1mb     1mb 
#> yellow open pos                   5 1       0     0    575b    575b 
#> yellow open shakespeare2          5 1       0     0    575b    575b 
#> yellow open stuff_g               5 1       0     0    575b    575b 
#> yellow open geonames              5 1 6646030 20005   1.9gb   1.9gb 
#> yellow open gbifgeo               5 1     600     0 861.9kb 861.9kb 
#> yellow open yep                   5 1       2     0   7.2kb   7.2kb 
#> yellow open diamonds_small        5 1    1000     0 209.2kb 209.2kb 
#> yellow open iris                  5 1     150     0  40.1kb  40.1kb 
#> yellow open stuff_k               5 1       0     0    575b    575b 
#> yellow open things                5 1       0     0    575b    575b 
#> yellow open shakespeare           5 1    5000     0     1mb     1mb 
#> yellow open stuff_j               5 1       0     0    575b    575b 
#> yellow open gbifgeopoint          5 1     600     0 953.5kb 953.5kb 
#> yellow open hello                 5 1      32     0  33.6kb  33.6kb
```

### Cluster info

> Get cluster health information


```r
cluster_health()
#> $cluster_name
#> [1] "elasticsearch"
#> 
#> $status
#> [1] "yellow"
#> 
#> $timed_out
#> [1] FALSE
#> 
#> $number_of_nodes
#> [1] 1
#> 
#> $number_of_data_nodes
#> [1] 1
#> 
#> $active_primary_shards
#> [1] 210
#> 
#> $active_shards
#> [1] 210
#> 
#> $relocating_shards
#> [1] 0
#> 
#> $initializing_shards
#> [1] 0
#> 
#> $unassigned_shards
#> [1] 210
#> 
#> $number_of_pending_tasks
#> [1] 0
```

### Node info

> Get node information


```r
nodes_info()
```

> You can do things like stop and start nodes, change their settings and so on.

## Indices

> An index is like a database, and a type within an index is like a table within
a database. Then documents within types, sort of like rows in a table.
There are many functions to interact with indices

* `index_create()`
* `index_delete()`
* `index_exists()`
* `index_get()`
* `index_stats()`
* ...

> Create an index


```
#> $acknowledged
#> [1] TRUE
```


```r
index_create("animals")
#> $acknowledged
#> [1] TRUE
```


```r
index_get("animals")
#> $animals
#> $animals$aliases
#> named list()
#> 
#> $animals$mappings
#> named list()
#> 
#> $animals$settings
#> $animals$settings$index
#> $animals$settings$index$creation_date
#> [1] "1428698531452"
#> 
#> $animals$settings$index$number_of_shards
#> [1] "5"
#> 
#> $animals$settings$index$number_of_replicas
#> [1] "1"
#> 
#> $animals$settings$index$version
#> $animals$settings$index$version$created
#> [1] "1050199"
#> 
#> 
#> $animals$settings$index$uuid
#> [1] "TLYQm-HRRBaag_dmdNDdXA"
#> 
#> 
#> 
#> $animals$warmers
#> named list()
```

## Documents

> Documents are the individual pieces of data within each index

* `docs_create()`
* `docs_get()`
* `docs_mget()`
* `docs_delete()`
* `docs_bulk()`

> Create a few documents


```r
docs_create(index='animals', type='bears', id=1, body=list(id="12345", name="big bear"))
#> $`_index`
#> [1] "animals"
#> 
#> $`_type`
#> [1] "bears"
#> 
#> $`_id`
#> [1] "1"
#> 
#> $`_version`
#> [1] 1
#> 
#> $created
#> [1] TRUE
docs_create(index='animals', type='lions', id=1, body=list(id="6789", name="scary lion!"))
#> $`_index`
#> [1] "animals"
#> 
#> $`_type`
#> [1] "lions"
#> 
#> $`_id`
#> [1] "1"
#> 
#> $`_version`
#> [1] 1
#> 
#> $created
#> [1] TRUE
```

> The documents are there now, get one


```r
docs_get(index='animals', type='bears', id=1)
#> $`_index`
#> [1] "animals"
#> 
#> $`_type`
#> [1] "bears"
#> 
#> $`_id`
#> [1] "1"
#> 
#> $`_version`
#> [1] 1
#> 
#> $found
#> [1] TRUE
#> 
#> $`_source`
#> $`_source`$id
#> $`_source`$id[[1]]
#> [1] "12345"
#> 
#> 
#> $`_source`$name
#> $`_source`$name[[1]]
#> [1] "big bear"
```

> Get multiple documents at once


```r
docs_mget(index_type_id=list(c("animals","bears",1), c("animals","lions",1)))
#> $docs
#> $docs[[1]]
#> $docs[[1]]$`_index`
#> [1] "animals"
#> 
#> $docs[[1]]$`_type`
#> [1] "bears"
#> 
#> $docs[[1]]$`_id`
#> [1] "1"
#> 
#> $docs[[1]]$`_version`
#> [1] 1
#> 
#> $docs[[1]]$found
#> [1] TRUE
#> 
#> $docs[[1]]$`_source`
#> $docs[[1]]$`_source`$id
#> $docs[[1]]$`_source`$id[[1]]
#> [1] "12345"
#> 
#> 
#> $docs[[1]]$`_source`$name
#> $docs[[1]]$`_source`$name[[1]]
#> [1] "big bear"
#> 
#> 
#> 
#> 
#> $docs[[2]]
#> $docs[[2]]$`_index`
#> [1] "animals"
#> 
#> $docs[[2]]$`_type`
#> [1] "lions"
#> 
#> $docs[[2]]$`_id`
#> [1] "1"
#> 
#> $docs[[2]]$`_version`
#> [1] 1
#> 
#> $docs[[2]]$found
#> [1] TRUE
#> 
#> $docs[[2]]$`_source`
#> $docs[[2]]$`_source`$id
#> $docs[[2]]$`_source`$id[[1]]
#> [1] "6789"
#> 
#> 
#> $docs[[2]]$`_source`$name
#> $docs[[2]]$`_source`$name[[1]]
#> [1] "scary lion!"
```

## Search

### First, get data

> Elasticsearch has a bulk load API to load data in fast. The format is weird - it's sort of JSON. I include a few data sets in `elastic` so it's easy to get up and running, and so when you run examples in this package they'll actually run the same way (hopefully).

> Shakespeare data


```r
shakespeare <- system.file("examples", "shakespeare_data.json", package = "elastic")
```

> Then load the data into Elasticsearch:


```r
docs_bulk(shakespeare)
```

### Search the data

> Search the `shakespeare` index and only return 1 result


```r
Search(index = "shakespeare", size = 1)$hits$hits
#> [[1]]
#> [[1]]$`_index`
#> [1] "shakespeare"
#> 
#> [[1]]$`_type`
#> [1] "line"
#> 
#> [[1]]$`_id`
#> [1] "4"
#> 
#> [[1]]$`_version`
#> [1] 6
#> 
#> [[1]]$`_score`
#> [1] 1
#> 
#> [[1]]$`_source`
#> [[1]]$`_source`$line_id
#> [1] 5
#> 
#> [[1]]$`_source`$play_name
#> [1] "Henry IV"
#> 
#> [[1]]$`_source`$speech_number
#> [1] 1
#> 
#> [[1]]$`_source`$line_number
#> [1] "1.1.2"
#> 
#> [[1]]$`_source`$speaker
#> [1] "KING HENRY IV"
#> 
#> [[1]]$`_source`$text_entry
#> [1] "Find we a time for frighted peace to pant,"
```

> Search the `shakespeare` index, and the `scene` document type, and query for _york_


```r
Search(index="shakespeare", type="scene",
       q="york", size=2, fields = 'speaker')$hits$hits
#> [[1]]
#> [[1]]$`_index`
#> [1] "shakespeare"
#> 
#> [[1]]$`_type`
#> [1] "scene"
#> 
#> [[1]]$`_id`
#> [1] "2588"
#> 
#> [[1]]$`_version`
#> [1] 6
#> 
#> [[1]]$`_score`
#> [1] 1.377465
#> 
#> [[1]]$fields
#> [[1]]$fields$speaker
#> [[1]]$fields$speaker[[1]]
#> [1] "SIR WALTER BLUNT"
#> 
#> 
#> 
#> 
#> [[2]]
#> [[2]]$`_index`
#> [1] "shakespeare"
#> 
#> [[2]]$`_type`
#> [1] "scene"
#> 
#> [[2]]$`_id`
#> [1] "2634"
#> 
#> [[2]]$`_version`
#> [1] 6
#> 
#> [[2]]$`_score`
#> [1] 1.377465
#> 
#> [[2]]$fields
#> [[2]]$fields$speaker
#> [[2]]$fields$speaker[[1]]
#> [1] "ARCHBISHOP OF YORK"
```

> Fuzzy search


```r
Search(index="shakespeare", q="text_entry:ma~")$hits$total
#> [1] 893
```

> Ranges, here where `line_id` value is between 10 and 20


```r
Search(index="shakespeare", q="line_id:[10 TO 20]")$hits$total
#> [1] 11
```

> Give explanation of search in result


```r
Search(index="shakespeare", size=1, explain=TRUE)
#> $took
#> [1] 12
#> 
#> $timed_out
#> [1] FALSE
#> 
#> $`_shards`
#> $`_shards`$total
#> [1] 5
#> 
#> $`_shards`$successful
#> [1] 5
#> 
#> $`_shards`$failed
#> [1] 0
#> 
#> 
#> $hits
#> $hits$total
#> [1] 5000
#> 
#> $hits$max_score
#> [1] 1
#> 
#> $hits$hits
#> $hits$hits[[1]]
#> $hits$hits[[1]]$`_shard`
#> [1] 0
#> 
#> $hits$hits[[1]]$`_node`
#> [1] "JBp-up-tRgqWtTuYvLkjhA"
#> 
#> $hits$hits[[1]]$`_index`
#> [1] "shakespeare"
#> 
#> $hits$hits[[1]]$`_type`
#> [1] "line"
#> 
#> $hits$hits[[1]]$`_id`
#> [1] "4"
#> 
#> $hits$hits[[1]]$`_version`
#> [1] 6
#> 
#> $hits$hits[[1]]$`_score`
#> [1] 1
#> 
#> $hits$hits[[1]]$`_source`
#> $hits$hits[[1]]$`_source`$line_id
#> [1] 5
#> 
#> $hits$hits[[1]]$`_source`$play_name
#> [1] "Henry IV"
#> 
#> $hits$hits[[1]]$`_source`$speech_number
#> [1] 1
#> 
#> $hits$hits[[1]]$`_source`$line_number
#> [1] "1.1.2"
#> 
#> $hits$hits[[1]]$`_source`$speaker
#> [1] "KING HENRY IV"
#> 
#> $hits$hits[[1]]$`_source`$text_entry
#> [1] "Find we a time for frighted peace to pant,"
#> 
#> 
#> $hits$hits[[1]]$`_explanation`
#> $hits$hits[[1]]$`_explanation`$value
#> [1] 1
#> 
#> $hits$hits[[1]]$`_explanation`$description
#> [1] "ConstantScore(*:*), product of:"
#> 
#> $hits$hits[[1]]$`_explanation`$details
#> $hits$hits[[1]]$`_explanation`$details[[1]]
#> $hits$hits[[1]]$`_explanation`$details[[1]]$value
#> [1] 1
#> 
#> $hits$hits[[1]]$`_explanation`$details[[1]]$description
#> [1] "boost"
#> 
#> 
#> $hits$hits[[1]]$`_explanation`$details[[2]]
#> $hits$hits[[1]]$`_explanation`$details[[2]]$value
#> [1] 1
#> 
#> $hits$hits[[1]]$`_explanation`$details[[2]]$description
#> [1] "queryNorm"
```

## More info

> Look out for `elasticdsl` package, which will make it much easier to perform queries against Elasticsearch

> A good place to ask questions/get involved is in the repo at ropensci/elastic or
in our discussion forum https://discuss.ropensci.org

## end

> That's it! Thanks for watching...

