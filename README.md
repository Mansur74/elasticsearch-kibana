# elasticsearch-kibana
```
//PART1
// put the index
PUT product

//remove the index
DELETE product

//CRUD
//create
POST product/_doc
{
  "type": "furniture",
  "name": "chair",
  "price": 2500
}

//to add a spesific id
POST product/_doc/1
{
  "type": "furniture",
  "name": "chair",
  "price": 2500
}

//read
GET product/_doc/1

//update
POST product/_update/1
{
  "doc": 
  {
    "price": 3000
  }
}

//delete
DELETE product/_doc/1

GET product/_search
{
  "query": {
    "match_all": {}
  }
}

PUT product

//PART2

//search all
GET news_headlines/_search

//increase the total value
GET news_headlines/_search
{
  "track_total_hits": true
}

//search with range
GET news_headlines/_search
{
  "query": {
    "range": {
      "date": {
        "gte": "2015-06-20",
        "lte": "2015-09-22"
      }
    }
  }
}

//search by categories
GET news_headlines/_search
{
  "aggs": {
    "by_categories": {
      "terms": {
        "field": "category",
        "size": 100
      }
    }
  }
}

//search for the most significant term in a category
GET news_headlines/_search
{
  "query": {
    "match": {
      "category": "ENTERTAINMENT"
    }
  }, 
  "aggs": {
    "popular_in_entertainment": {
      "significant_text": {
        "field": "headline"
      }
    }
  }
}

//search with or operator as default
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": "Khloe Kardashian Kendall Jenner"
    }
  }
}

//search with and operator
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner",
        "operator": "and"
      }
    }
  }
}


//search with minimum size
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner",
        "minimum_should_match": 3
      }
    }
  }
}


//PART3

GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Shape of you"
      }
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "multi_match": {
      "query": "Michelle Obama",
      "fields": ["headline", "short_description", "authors"]
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "multi_match": {
      "query": "Michelle Obama",
      "fields": ["headline^2", "short_description", "authors"]
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "multi_match": {
      "query": "Michelle Obama",
      "fields": ["headline^2", "short_description"]
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "multi_match": {
      "query": "Michelle Obama",
      "fields": ["headline^2", "short_description"],
      "type": "phrase"
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "match_phrase": {
      "headline": "Michelle Obama"
    }
  },
  "aggregations": {
    "category_mentions":{
      "terms" :{
        "field": "category",
        "size": 100
      }
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "headline": "Michelle Obama"
          }
        }
      ]
    }
  }
}
GET news_headlines/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "headline": "Michelle Obama"
          }
        }
      ],
      "must_not": [
        {
          "match_phrase": {
            "category": "BLACK VOICES"
          }
        }
      ]
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "headline": "Michelle Obama"
          }
        }
      ],
      "filter": [
        {
          "range": {
            "date": {
              "gte": "2014-03-25",
               "lte": "2016-03-25"
            }
          }
        }
      ]
    }
  }
}

GET news_headlines/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "headline": "Michelle Obama"
          }
        }
      ],
      "should": [
        {
          "match": {
            "headline": "Becoming"
          }
        },
        {
          "match": {
            "headline": "women"
          }
        },
        {
          "match": {
            "headline": "empower"
          }
        }
      ]
    }
  }
}


//PART 4 
PUT ecommerce_data
{
  "mappings": {
    "properties": {
      "Country": {
        "type": "keyword"
      },
      "CustomerID": {
        "type": "long"
      },
      "Description": {
        "type": "text"
      },
      "InvoiceDate": {
        "type": "date",
        "format": "M/d/yyyy H:m"
      },
      "InvoiceNo": {
        "type": "keyword"
      },
      "Quantity": {
        "type": "long"
      },
      "StockCode": {
        "type": "keyword"
      },
      "UnitPrice": {
        "type": "double"
      }
    }
  }
}

POST _reindex
{
  "source": {
    "index": "news_headlines"
  },
  "dest": {
    "index": "ecommerce_data"
  }
}

POST ecommerce_data/_delete_by_query
{
  "query": {
    "range": {
      "UnitPrice": {
        "lte": 0
      }
    }
  }
}

POST ecommerce_data/_delete_by_query
{
  "query": {
    "range": {
      "UnitPrice": {
        "gte": 500
      }
    }
  }
}
```
