## UPLOAD ONE DOC
```
curl -X POST "localhost:9200/bank/_doc/?pretty" -H 'Content-Type: application/json' -d'
{
  "account_number": 123,
  "balance": 50000,
  "firstname": "Joe",
  "lastname": "Smoe",
  "age": 25,
  "gender": "M",
  "address": "60 State Street",
  "employer": "Pluralsight",
  "email": "joesmoe@pluralsight.com",
  "city": "Boston",
  "state": "MA"
}'
```

## UPLOAD BULK DOCS WITH PRETTY FORMAT
```curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?pretty&refresh" --data-binary "@accounts.json"```

### UPLOAD BULK DOCS WITHOUT PRETTY FORMAT
```curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?refresh" --data-binary "@accounts.json"```

refresh tells all shards that are affected to refresh and make data available for search.
data-binary flag used for when you're bulk uploading text

## MAPPINGS FOR BANK INDEX
```
curl -X PUT "localhost:9200/bank/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "account_number": {
        "type": "long"
      },
      "balance": {
        "type": "double"
      },
      "firstname": {
        "type": "keyword"
      },
      "lastname": {
        "type": "keyword"
      },
      "age": {
        "type": "integer"
      },
      "gender": {
        "type": "keyword"
      },
      "address": {
        "type": "text"
      },
      "employer": {
        "type": "keyword"
      },
      "email": {
        "type": "keyword"
      },
      "city": {
        "type": "keyword"
      },
      "state": {
        "type": "keyword"
      }
    }
  }  
}'
```

## Fetch Data

```curl -X GET "localhost:9200/bank/_doc/0?pretty"```

## Search Data

### Search by query params (NOT RECOMMENDED)
```curl -X GET "localhost:9200/bank/_search?pretty&q=Holt"```

### Search by body
```
curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "firstname": "Holt"
    }
  }
}
'
```
You can specify whether is whether the operation done for the above command is AND or OR by the following:
```
"query": {
  ...
  "match": {
    ...
    "operator: "and"
  }
}
```

### Search by phrase prefixing
```
curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_phrase_prefix": {
      "firstname": "Hol"
    }
  }
}
'
```