[TOC]

## 文本类型系列[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

文本系列包括以下字段类型：

-   [`text`](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html#text-field-type)，用于全文内容的传统字段类型，例如电子邮件正文或产品描述。
-   [`match_only_text`](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html#match-only-text-field-type)，它的空间优化变体`text`禁用评分并在需要位置的查询上执行较慢。它最适合索引日志消息。

### 文本字段类型[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

用于索引全文值的字段，例如电子邮件正文或产品描述。这些字段是`analyzed`，也就是说，它们在被索引之前通过 [分析器](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis.html)将字符串转换为单个术语的列表。分析过程允许 Elasticsearch*在*每个全文字段中搜索单个单词。文本字段不用于排序，也很少用于聚合（尽管 [重要的文本聚合](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-significanttext-aggregation.html) 是一个明显的例外）。

`text`字段最适合非结构化但人类可读的内容。如果您需要索引机器生成的非结构化内容，请参阅 [映射非结构化内容](https://www.elastic.co/guide/en/elasticsearch/reference/current/keyword.html#mapping-unstructured-content)。

如果您需要索引结构化内容，例如电子邮件地址、主机名、状态代码或标签，您可能应该使用[`keyword`](https://www.elastic.co/guide/en/elasticsearch/reference/current/keyword.html)字段。

下面是一个文本字段的映射示例：

```json
PUT my-index-000001 
{ 
    "mappings" : { 
        "properties" : { 
            "full_name" : { 
                "type" : "text" 
            } 
        } 
    } 
} 

------
curl -X PUT "localhost:9200/logs?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "message": {
        "type": "match_only_text"
      }
    }
  }
}
'

```



### 将字段用作文本和关键字[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

有时同时拥有同一字段的全文 ( `text`) 和关键字 ( `keyword`) 版本很有用：一个用于全文搜索，另一个用于聚合和排序。这可以通过[多字段](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html)来实现 。

### 文本字段的参数[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

`text`字段接受以下参数：

| [`analyzer`](https://www.elastic.co/guide/en/elasticsearch/reference/current/analyzer.html) | 应该在索引时和搜索时用于该字段 的[分析器](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis.html)`text`（除非被 覆盖 [`search_analyzer`](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-analyzer.html)）。默认为默认索引分析器，或 [`standard`分析器](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-analyzer.html)。 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`boost`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-boost.html) | 映射字段级查询时间提升。接受浮点数，默认为`1.0`。            |
| [`eager_global_ordinals`](https://www.elastic.co/guide/en/elasticsearch/reference/current/eager-global-ordinals.html) | 是否应该在刷新时急切地加载全局序数？接受`true`或`false` （默认）。对于经常用于（重要）术语聚合的字段，启用此功能是一个好主意。 |
| [`fielddata`](https://www.elastic.co/guide/en/elasticsearch/reference/current/fielddata.html) | 该字段是否可以使用内存中的字段数据进行排序、聚合或脚本编写？接受`true`或`false`（默认）。 |
| [`fielddata_frequency_filter`](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html#field-data-filtering) | 允许决定在`fielddata` 启用时将哪些值加载到内存中的专家设置。默认情况下加载所有值。 |
| [`fields`](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html) | 多字段允许为不同目的以多种方式索引相同的字符串值，例如一个字段用于搜索和一个多字段用于排序和聚合，或者由不同的分析器分析相同的字符串值。 |
| [`index`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-index.html) | 该字段应该是可搜索的吗？接受`true`（默认）或`false`。        |
| [`index_options`](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-options.html) | 什么信息应该存储在索引中，用于搜索和突出显示的目的。默认为`positions`. |
| [`index_prefixes`](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-prefixes.html) | 如果启用，2 到 5 个字符之间的术语前缀将被索引到一个单独的字段中。这允许前缀搜索以更大的索引为代价更有效地运行。 |
| [`index_phrases`](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-phrases.html) | 如果启用，两个词组合 ( *shingles* ) 将被索引到一个单独的字段中。这允许以更大的索引为代价更有效地运行精确的短语查询（无溢出）。请注意，这在不删除停用词时效果最佳，因为包含停用词的短语将不使用辅助字段，并且会回退到标准短语查询。接受`true`或`false`（默认）。 |
| [`norms`](https://www.elastic.co/guide/en/elasticsearch/reference/current/norms.html) | 对查询进行评分时是否应考虑字段长度。接受`true`（默认）或`false`。 |
| [`position_increment_gap`](https://www.elastic.co/guide/en/elasticsearch/reference/current/position-increment-gap.html) | 应该在字符串数组的每个元素之间插入的假术语位置的数量。默认为`position_increment_gap` 分析器上配置的默认为`100`. `100`被选择是因为它可以防止具有相当大的斜率（小于 100）的短语查询跨字段值匹配术语。 |
| [`store`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-store.html) | 字段值是否应与[`_source`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-source-field.html)字段分开存储和检索。接受`true`或`false` （默认）。 |
| [`search_analyzer`](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-analyzer.html) | 本[`analyzer`](https://www.elastic.co/guide/en/elasticsearch/reference/current/analyzer.html)应该在对搜索时使用`text`领域。默认为`analyzer`设置。 |
| [`search_quote_analyzer`](https://www.elastic.co/guide/en/elasticsearch/reference/current/analyzer.html#search-quote-analyzer) | 本[`analyzer`](https://www.elastic.co/guide/en/elasticsearch/reference/current/analyzer.html)应在搜索时遇到一个短语时使用。默认为`search_analyzer`设置。 |
| [`similarity`](https://www.elastic.co/guide/en/elasticsearch/reference/current/similarity.html) | 应该使用 哪种评分算法或*相似性*。默认为`BM25`.               |
| [`term_vector`](https://www.elastic.co/guide/en/elasticsearch/reference/current/term-vector.html) | 是否应为字段存储术语向量。默认为`no`.                        |
| [`meta`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-field-meta.html) | 有关该字段的元数据。                                         |

### `fielddata` 映射参数[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

`text`默认情况下，字段可搜索，但默认情况下不可用于聚合、排序或脚本编写。如果您尝试对`text`字段上的脚本中的值进行排序、聚合或访问，您将看到以下异常：

默认情况下，在文本字段上禁用 Fielddata。设置`fielddata=true`on `your_field_name`以通过反转倒排索引将 fielddata 加载到内存中。请注意，这可能会占用大量内存。

字段数据是从聚合、排序或脚本中的全文字段访问已分析令牌的唯一方法。例如，像`New York` 这样的全文字段会被分析为`new`and `york`。聚合这些令牌需要字段数据。

### 启用 fielddata 之前[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

在文本字段上启用 fielddata 通常没有意义。字段数据与[字段数据缓存](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-fielddata.html)一起存储在堆中，因为计算成本很高。计算字段数据可能会导致延迟峰值，而增加堆使用量是导致集群性能问题的一个原因。

大多数想要对文本字段执行更多操作的用户都使用[多字段映射](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html) ，其中既有`text`用于全文搜索的字段，也有[`keyword`](https://www.elastic.co/guide/en/elasticsearch/reference/current/keyword.html)用于聚合的未分析字段，如下所示：

```json
PUT my-index-000001
{
  "mappings": {
    "properties": {
      "my_field": { 
        "type": "text",
        "fields": {
          "keyword": { 
            "type": "keyword"
          }
        }
      }
    }
  }
}     
      
    
curl -X PUT "localhost:9200/my-index-000001?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "my_field": { 
        "type": "text",
        "fields": {
          "keyword": { 
            "type": "keyword"
          }
        }
      }
    }
  }
}
'

```



|      | 使用该`my_field`字段进行搜索。                       |
| ---- | ---------------------------------------------------- |
|      | 将该`my_field.keyword`字段用于聚合、排序或在脚本中。 |

### 在`text`字段上启用 fielddata[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

您可以`text`使用[更新映射 API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html)在现有字段 上启用 fielddata ，如下所示：

```json
PUT my-index-000001/_mapping
{
  "properties": {
    "my_field": { 
      "type":     "text",
      "fielddata": true
    }
  }
}

curl -X PUT "localhost:9200/my-index-000001/_mapping?pretty" -H 'Content-Type: application/json' -d'
{
  "properties": {
    "my_field": { 
      "type":     "text",
      "fielddata": true
    }
  }
}
'

```



|      | 您指定的映射`my_field`应包含该字段的现有映射以及`fielddata`参数。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### `fielddata_frequency_filter` 映射参数[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/text.asciidoc)

Fielddata 过滤可用于减少加载到内存中的术语数量，从而减少内存使用。术语可以按*频率*过滤：

频率过滤器允许您仅加载文档频率介于 a`min`和`max`value之间的术语，这可以表示为绝对数（当数字大于 1.0 时）或百分比（例如`0.01`is`1%`和`1.0`is `100%`）。频率是**按段**计算的 。百分比基于具有字段值的文档数量，而不是细分中的所有文档。

通过指定段应包含的最小文档数，可以完全排除小段`min_segment_size`：

```json
PUT my-index-000001
{
  "mappings": {
    "properties": {
      "tag": {
        "type": "text",
        "fielddata": true,
        "fielddata_frequency_filter": {
          "min": 0.001,
          "max": 0.1,
          "min_segment_size": 500
        }
      }
    }
  }
}


curl -X PUT "localhost:9200/my-index-000001?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "tag": {
        "type": "text",
        "fielddata": true,
        "fielddata_frequency_filter": {
          "min": 0.001,
          "max": 0.1,
          "min_segment_size": 500
        }
      }
    }
  }
}
'

```



### 仅匹配文本字段类型[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/match-only-text.asciidoc)

它的一个变体用[`text`](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html#text-field-type)位置查询的评分和效率来换取空间效率。该字段以与`text`仅索引文档 ( `index_options: docs`) 并禁用规范 ( `norms: false`)的字段相同的方式有效地存储数据。术语查询的执行速度与在`text`字段上的执行速度一样快，但需要诸如[`match_phrase`查询之类的](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html)位置的 [查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html)执行速度较慢，因为它们需要查看`_source`文档以验证短语是否匹配。所有查询都返回等于 1.0 的常量分数。

分析不可配置：始终使用[默认分析器](https://www.elastic.co/guide/en/elasticsearch/reference/current/specify-analyzer.html#specify-index-time-default-analyzer) （[`standard`](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-analyzer.html)默认情况下）分析文本 。

此字段不支持[跨度查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/span-queries.html)，请改用[间隔查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-intervals-query.html)，或者 [`text`](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html#text-field-type)如果您绝对需要跨度查询，则使用 字段类型。

除此之外，`match_only_text`支持与`text`. 就像 一样`text`，它不支持排序，并且对聚合的支持有限。

```json
PUT logs
{
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "message": {
        "type": "match_only_text"
      }
    }
  }
}

curl -X PUT "localhost:9200/logs?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "message": {
        "type": "match_only_text"
      }
    }
  }
}
'

```



#### 仅匹配文本字段的参数[编辑](https://github.com/elastic/elasticsearch/edit/7.14/docs/reference/mapping/types/match-only-text.asciidoc)

接受以下映射参数：

| [`fields`](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html) | 多字段允许为不同目的以多种方式索引相同的字符串值，例如一个字段用于搜索和一个多字段用于排序和聚合，或者由不同的分析器分析相同的字符串值。 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`meta`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-field-meta.html) | 有关该字段的元数据。                                         |