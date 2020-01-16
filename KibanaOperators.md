# Kibana for Operators
* Web UI for ElasticSearch
* Turn data into visualizations
* Enterprise Features
    * Security - Free (Mostly)
    * Alerting
    * Monitorying
    * Reporting
    * Graph
    * Machine Learning
    * Elastic SQL
    * Canvas  

Connect to `https://classroom.perched.io/`
```
Select Space
Click Manage Space to create a space

Index Management - Delete Indices
Index Life Cycle - 
Rollup Jobs

Index patterns - Search Patterns


* Discover Tab
    * New - new Search
    * Save - saves current search (overwrites current search)  
    * Share -> Permalink -> Short URL
        * http://classroom.perched.io:5601/s/hunt/goto/05d7ba76b0dca01681e30234d49a1b38  
    * Inspect - statistic data

    IP in between operation 
        First IP in inclusive 
        Last IP is exclusive (must go next ip higer)  


Phrases
Must and must not be present - Lucene Feature
Grouping things on the right

_exists_: http.uri

Regex begins with "/"
```


## Visualizations
vaga github - Visualizations

Metric - How to tally data
Bucket - How to sort it


## Watcher 

(https://www.elastic.co/guide/en/elasticsearch/reference/current/trigger-schedule.html)
```
{
  "trigger": {
    "schedule": {
      "interval": "30m"
    }
  },
  "input": {
    "search": {
      "request": {
        "body": {
          "size": 0,
          "query": {
            "match_all": {}
          }
        },
        "indices": [
          "*"
        ]
      }
    }
  },
  "condition": {
    "compare": {
      "ctx.payload.hits.total": {
        "gte": 10
      }
    }
  },
  "actions": {
    "my-logging-action": {
      "logging": {
        "text": "There are {{ctx.payload.hits.total}} documents in your index. Threshold is 10."
      }
    }
  }
}
```

* Schedule
    * A schedule for running a query and checking the condition.
* Query  
    * The query to run as input to the condition. Watches support the full Elasticsearch query language, including aggregations.
* Condition
    * A condition that determines whether or not to execute the actions. You can use simple conditions (always true), or use scripting for more sophisticated scenarios.
* Actions
    * One or more actions, such as sending email, pushing data to 3rd party systems through a webhook, or indexing the results of the query.  

## Graph

## Machine Learning

