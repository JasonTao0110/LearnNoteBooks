### ELESTICSEARCH 异常录



#### Search ?Fielddata is disabled

##### [question]:

```markdown
? RequestError(400, 'search_phase_execution_exception', 'Fielddata is disabled on text fields by default. Set fielddata=true on [update_time] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead.')
```

##### [season]:

```markdown
1. Elasticsearch 5.x版本以后，对排序和聚合等操作，用单独的数据结构(fielddata)缓存到内存里了，默认是不开启的，需要单独开启。
2. 对于 type:"text" 的，进行排序聚合等操作时，若fielddata=false,则会报错；其他类型的、或者type:text但是fielddata=true的则不然
```

##### [solve]:

```markdown
# solve
# 待开启开启字段  "update_time"

PUT my_index/_mapping
{
    "properties":{
        "update_time":{
            "type":"text",
            "fielddata":true
        }
    }
}

```

***[官方文档    fielddata](./es-err.md)***