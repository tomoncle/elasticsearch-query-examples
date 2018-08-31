# 查询每分钟内url请求的成功率
> 因为没有找到rate函数，所以查询对应返回码的数量，通过程序再去计算成功率.
> success code in [200,300), error code in 300+ ;
> success_rate = success_size / total_size.

```
$ curl -XGET "http://192.168.84.25:9200/filebeat-*/_search" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "sales_per_month": {
      "date_histogram": {
        "field": "@timestamp",
        "interval": "1m",
        "time_zone": "+08:00"
      },
      "aggs": {
        "genders": {
          "terms": {
            "field": "nginx.access.response_code"
          }
        }
      }
    }
  }
}'|python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   912  100   588  100   324   194k   107k --:--:-- --:--:-- --:--:--  287k
{
    "_shards": {
        "failed": 0,
        "skipped": 0,
        "successful": 3,
        "total": 3
    },
    "aggregations": {
        "sales_per_month": {
            "buckets": [
                {
                    "doc_count": 1,
                    "genders": {
                        "buckets": [
                            {
                                "doc_count": 1,
                                "key": 200
                            }
                        ],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535710080000,
                    "key_as_string": "2018-08-31T18:08:00.000+08:00"
                },
                {
                    "doc_count": 3,
                    "genders": {
                        "buckets": [
                            {
                                "doc_count": 2,
                                "key": 200
                            },
                            {
                                "doc_count": 1,
                                "key": 404
                            }
                        ],
                        "doc_count_error_upper_bound": 0,
                        "sum_other_doc_count": 0
                    },
                    "key": 1535710140000,
                    "key_as_string": "2018-08-31T18:09:00.000+08:00"
                }
            ]
        }
    },
    "hits": {
        "hits": [],
        "max_score": 0.0,
        "total": 4
    },
    "timed_out": false,
    "took": 0
}
```
