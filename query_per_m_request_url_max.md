# 查询每分钟内，请求次数最多的url地址

```
$ curl -XGET "http://192.168.84.25:9200/filebeat-*/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "@timestamp": {
              "from": "2018-08-31 15:00:00.0 +0800",
              "to": "2018-09-01 00:00:00.0 +0800",
              "include_lower": true,
              "include_upper": true,
              "format": "yyyy-MM-dd HH:mm:ss.SSS Z",
              "boost": 1
            }
          }
        }
      ]
    }
  },
  "size": 0,
  "aggs": {
    "per_minute_request_times": {
      "date_histogram": {
        "field": "@timestamp",
        "interval": "minute",
        "time_zone": "+08:00"
      },
      "aggs": {
        "genders": {
          "terms": {
            "field": "nginx.access.url",
            "size": 1,
            "order": {
              "_count": "desc"
            }
          }
        }
      }
    }
  }
}'|python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3121  100  2287  100   834   433k   158k --:--:-- --:--:-- --:--:--  446k
{
    "_shards": {
        "failed": 0,
        "skipped": 0,
        "successful": 6,
        "total": 6
    },
    "aggregations": {
        "per_minute_request_times": {
            "buckets": [
                {
                    "doc_count": 2,
                    "genders": {
                        "buckets": [
                            {
                                "doc_count": 1,
                                "key": "/app/api/classes/"
                            }
                        ],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 1
                    },
                    "key": 1535703900000,
                    "key_as_string": "2018-08-31T16:25:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535703960000,
                    "key_as_string": "2018-08-31T16:26:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704020000,
                    "key_as_string": "2018-08-31T16:27:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704080000,
                    "key_as_string": "2018-08-31T16:28:00.000+08:00"
                },
                {
                    "doc_count": 1,
                    "genders": {
                        "buckets": [
                            {
                                "doc_count": 1,
                                "key": "/app/paths"
                            }
                        ],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704140000,
                    "key_as_string": "2018-08-31T16:29:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704200000,
                    "key_as_string": "2018-08-31T16:30:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704260000,
                    "key_as_string": "2018-08-31T16:31:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704320000,
                    "key_as_string": "2018-08-31T16:32:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704380000,
                    "key_as_string": "2018-08-31T16:33:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704440000,
                    "key_as_string": "2018-08-31T16:34:00.000+08:00"
                },
                {
                    "doc_count": 0,
                    "genders": {
                        "buckets": [],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704500000,
                    "key_as_string": "2018-08-31T16:35:00.000+08:00"
                },
                {
                    "doc_count": 3,
                    "genders": {
                        "buckets": [
                            {
                                "doc_count": 3,
                                "key": "/app/api/schools/"
                            }
                        ],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535704560000,
                    "key_as_string": "2018-08-31T16:36:00.000+08:00"
                }
            ]
        }
    },
    "hits": {
        "hits": [],
        "max_score": 0.0,
        "total": 6
    },
    "timed_out": false,
    "took": 2
}
```
