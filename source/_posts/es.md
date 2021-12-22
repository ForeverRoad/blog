---
title: es
date: 2021-12-17 16:08:09
tags: data
categories: data
---

### ES简要命令


#### 索引操作命令

> 类DDL语句

```ES
//插入文档，没有索引会自动创建
PUT /data/student/1
{
	"name":"张三",
	"sex":"男",
	"age":28
	"class":["数学","语文"]
}
//删除索引
DELTE /data/student



```


#### 简单的QueryString语句

> 可以根据get获取文档

```ES
GET /data/student/1

GET /data/student/_search

_search是关键词



```


#### 搜索结构体

> 根据json格式编写搜索语句



 

``` JSON
{
	"query":{
		"bool":{
			"must":{
				"match":{
					"name":"张三"
				},
				"filter":{
					"age":{"gt":20}
				}
			}
		}
	}
}

// query表示查询，bool表示布尔查询，只返回符合条件的,must相当于and，（对应的有should相当于or），match表示匹配相当于like，但是会进行分词处理，会搜张，会搜三，会搜张三（当然还有更复杂的用法）,filter表示过滤掉
```


- bool语句允许组合条件的查询
	- must ，必须，类似and

	- must_not	必须不，类似not

	- should	应该，类似or

	- filter	过滤，匹配符合条件的,当精确匹配的时候优先使用filter，能用缓存，而不用像query一样进行评分过滤，更高效


- term
	- 精确匹配，可用于数字时间，或者not_analyzed的字符串（不被分析的字符串，keyWord）

- terms
	- 指定多个值的精确匹配，类似于in

- match
	- 进行分词匹配搜索

- match_phrase
	- 按关键词进行搜索
- highlight
	- 高亮显示搜索结果，与query同级
	- 搜索结果会加上<em>标签</em>

- aggregations
	- 聚合，类似于group by
	- 对数据进行分组统计，可多级聚合分组
	- 与query同级
	- 可以使用聚合函数进行计算操作，如avg，sum，count等


```JSON
{
	"query":{
		"bool":{
			"must":{
				"match":{
					"name":"张三"
				},
				"filter":{
					"age":{"gt":20}
				}
			}
		}
	},
	"aggs": {
    "all_interests": {
      "terms": { "field": "sex" }
    }
  }
}
// 查询结果除了命中的，还会给出聚合数据，与hits同级，把不同的聚合key分类统计，

{
	"aggregations" : {
    "interests" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : 0,
          "doc_count" : 6
        },
        {
          "key" : 1,
          "doc_count" : 3
        },
        {
          "key" : 2,
          "doc_count" : 3
        }
      ]
    }
  }
}

```

- 分页，from size 类似于limit，表示起始位置和分页大小


- exists
	- 查询包含指定字段的文档，类似is not null

- missing
	- 查询不包含指定字段的文档，类似is null


- sort
	- 默认会根据相关性进行排序，出现最多的拍最上面，可以用sort关键字，指定字段进行排序
