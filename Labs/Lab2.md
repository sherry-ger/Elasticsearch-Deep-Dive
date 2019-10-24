# Queries, Aggregations, and Elasticsearch SQL

## Overview

* Full Text Search Queries
* Combined queries
* Aggregations
* Elasticsearch SQL

### Prepare to run the queries

We will be going over some sample queries together. We may not go through all of them.  Please have a look at them if you have time.

First thing first, please:

1. In Kibana, click on the kibana icon.

![kibanaicon](/Labs/images/kibana_logo.png)

2. Click on Load a data set and a Kibana dashboard under Add sample data

![sampledata](/Labs/images/addsampledata.png)

3. Click on Add data in the Sample Web Log pane

![sampledata](/Labs/images/sampleweblog.png)

4. Go to Dev Tools
![sampledata](/Labs/images/dev_tools.png)

5. Copy and paste the following into Dev Tools Console

```
# see what indices are on the cluster
GET _cat/indices

# get mappings of an index
GET filebeat-*/

# filter search
GET filebeat-*/_search
{
  "size": 100,
  "query": {
    "term": {
      "service.type": {
        "value": "elasticsearch"
      }
    }
  }
}

# full text search example 1
GET filebeat-*/_search
{
  "query": {
    "match": {
      "message": "update_mapping"
    }
  }  
}

# full text search example 2
GET filebeat-*/_search
{
  "query": {
    "match": {
      "message": "update mapping"
    }
  }  
}

# search with filter and query context
GET filebeat*/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "http.response.status_code": 200
          }
        },
        {
          "term": {
            "source.geo.country_iso_code": "US"
          }
        }
      ],
      "must": [
        {
          "match": {
            "url.original": "/services"
          }
        }
      ]
    }
  }
}

# Filter and aggregate
GET filebeat*/_search
{
  "size": 0,
    "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "http.response.status_code": 200
          }
        }
      ]
    }
  },
  "aggs": {
    "NAME": {
      "terms": {
        "field": "source.geo.continent_name",
        "size": 100
      },
      "aggs": {
        "NAME": {
          "terms": {
            "field": "source.geo.country_iso_code",
            "size": 100
          },
          "aggs": {
            "NAME": {
              "terms": {
                "field": "source.geo.region_name",
                "size": 10
              }
            }
          }
        }
      }
    }
  }
}

# nested document
DELETE nested_example

# nested mapping
PUT nested_example
{
  "mappings": {
    "properties": {
      "user": {
        "type": "nested" 
      }
    }
  }
}

PUT nested_example/_doc/1
{
  "group" : "fans",
  "user" : [
    {
      "first" : "John",
      "last" :  "Smith"
    },
    {
      "first" : "Alice",
      "last" :  "White"
    }
  ]
}

# nested query
GET nested_example/_search
{
  "query": {
    "nested": {
      "path": "user",
      "query": {
        "bool": {
          "must": [
            { "match": { "user.first": "Alice" }},
            { "match": { "user.last":  "Smith" }} 
          ]
        }
      }
    }
  }
}

GET nested_example/_search
{
  "query": {
    "nested": {
      "path": "user",
      "query": {
        "bool": {
          "must": [
            { "match": { "user.first": "Alice" }},
            { "match": { "user.last":  "White" }} 
          ]
        }
      },
      "inner_hits": { 
        "highlight": {
          "fields": {
            "user.first": {}
          }
        }
      }
    }
  }
}

# nested aggregation
GET nested_example/_search
{
  "aggs": {
    "lastname": {
      "nested": {
        "path": "user"
      },
      "aggs": {
        "user_lastname": {
          "terms": {
            "field": "user.last.keyword"
          }
        }
      }
    }
  }
}

# Parent Child 
DELETE parent_child_example

# join type mapping
PUT parent_child_example
{
  "mappings": {
    "properties": {
      "my_join_field": { 
        "type": "join",
        "relations": {
          "question": "answer" 
        }
      }
    }
  }
}

PUT parent_child_example/_doc/1?refresh
{
  "text": "This is a question",
  "my_join_field": {
    "name": "question" 
  }
}

PUT parent_child_example/_doc/2?refresh
{
  "text": "This is second question",
  "my_join_field": {
    "name": "question"
  }
}

PUT parent_child_example/_doc/3?routing=1
{
  "text": "This is an answer",
  "my_join_field": {
    "name": "answer", 
    "parent": "1" 
  }
}

PUT parent_child_example/_doc/4?routing=2
{
  "text": "This is second question",
  "my_join_field": {
    "name": "answer",
    "parent": "2"
  }
}

# Aggregate by parent and by child

DELETE child_example

PUT child_example
{
  "mappings": {
    "properties": {
      "join": {
        "type": "join",
        "relations": {
          "question": "answer"
        }
      }
    }
  }
}

PUT child_example/_doc/1
{
  "join": {
    "name": "question"
  },
  "body": "<p>I have Windows 2003 server and i bought a new Windows 2008 server...",
  "title": "Whats the best way to file transfer my site from server to a newer one?",
  "tags": [
    "windows-server-2003",
    "windows-server-2008",
    "file-transfer"
  ]
}

PUT child_example/_doc/2?routing=1
{
  "join": {
    "name": "answer",
    "parent": "1"
  },
  "owner": {
    "location": "Norfolk, United Kingdom",
    "display_name": "Sam",
    "id": 48
  },
  "body": "<p>Unfortunately you're pretty much limited to FTP...",
  "creation_date": "2009-05-04T13:45:37.030"
}

PUT child_example/_doc/3?routing=1&refresh
{
  "join": {
    "name": "answer",
    "parent": "1"
  },
  "owner": {
    "location": "Norfolk, United Kingdom",
    "display_name": "Troll",
    "id": 49
  },
  "body": "<p>Use Linux...",
  "creation_date": "2009-05-05T13:45:37.030"
}

POST child_example/_search?size=0
{
  "aggs": {
    "top-names": {
      "terms": {
        "field": "owner.display_name.keyword",
        "size": 10
      },
      "aggs": {
        "to-questions": {
          "parent": {
            "type" : "answer" 
          },
          "aggs": {
            "top-tags": {
              "terms": {
                "field": "tags.keyword",
                "size": 10
              }
            }
          }
        }
      }
    }
  }
}

POST child_example/_search?size=0
{
  "aggs": {
    "top-tags": {
      "terms": {
        "field": "tags.keyword",
        "size": 10
      },
      "aggs": {
        "to-answers": {
          "children": {
            "type" : "answer" 
          },
          "aggs": {
            "top-names": {
              "terms": {
                "field": "owner.display_name.keyword",
                "size": 10
              }
            }
          }
        }
      }
    }
  }
}

# SQL 
# We will be using the sample web log data we loaded earlier
GET _sql?format=txt
{
  "query": "SELECT COUNT(*) as total_errors, timestamp FROM kibana_sample_data_logs WHERE tags LIKE '%error%' GROUP BY timestamp ORDER BY timestamp DESC"
}

GET _sql?format=csv
{
  "query": "SELECT SUM(bytes) as total_bytes, host FROM kibana_sample_data_logs GROUP BY host"
}

# Convert SQL to query DSL

GET _sql/translate
{
  "query": "SELECT COUNT(*) as total_errors FROM kibana_sample_data_logs WHERE tags LIKE '%warning%' OR tags LIKE '%error%'"
}
```
