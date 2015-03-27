---
layout: post
category : 搜索
tagline: "elasticsearch中的一些小细节"
tags : [fuzzy_like_this, elasticsearch, more_like_this]
---
>more_like_this和fuzzy_like_this是es当中比较容易混淆的两个概念，下面介绍了这两种查询语句的具体文档及实际内容
> TODO 介绍两者底层的实现方式

##结论
more_like_this和fuzzy_like_this查询的本质区别是：funzzy_like_this是模糊匹配，意思是，如果进行适当的设置，已从fat匹配到fit这样的关键词，但是，more_like_this是精确匹配，不会出现这样的情况，匹配到得文档中一定和查询当中有相同的关键词。


## 构建more_like_this和fuzzy_like_this的基本测试环境
搭建基本的环境
先启动elasticsearch

	curl -XPUT localhost:9200/test
	curl -XPUT localhost:9200/test/test/1 -d '{"hello":"a fox run"}'
	curl -XPUT localhost:9200/test/test/2 -d '{"hello":"an apple drop, i ran"}'

##more_like_this

more_like_this查询得到与所提供文本相似的文档，实现的方式可以这么理解
对查询的匹配串进行分词，过滤得到一个词向量，比如，我要找和下面这句话相似的文档: 
	"a fox run"
“a fox run” => ["a", "fox", "run"]
实际在匹配时，是去文档里面找包含以上这些词向量的内容。

more_like_this可以使用额外的参数
field: 查询执行所作用的字段的数组。默认是_all字段。
percent_terms_to_match: 指明一个文档必须匹配多大比例的词项才被视为相似。默认是0.30。对于上面那个例子来说，一共3个词，3 * 0.3 约等于1。（这里的取舍只是一个示意，并不保证实际es采用的取舍过程是这样）。这样只要有一个词匹配到就认定是相似文档。
min_term_freq: 指明文档中词项出现的最低频次，低于该频次的将被忽略，默认为2。
max_query_terms: 指明在生成的查询中查询词项的最大数目，默认为25。
stop_words: 指明停词集合。
min_doc_freq: 指明词项至少在多少个文档中出现才不会被忽略。默认是5。
max_doc_freq: 指明词项在在多少个文档中出现的次数上限，高于这个次数会被忽略（比如a, an, is这些高频词）。
analyzer:  指定分析查询使用的分词器。

###more_like_this的查询用例

	{
	    "query": {
	        "more_like_this": {
	            "like_text": "run",
	            "min_term_freq": 1,
	            "min_doc_freq": 1
	        }
	    }
	}

得到下面的结果

	{
	"_index" : "test",
	"_type" : "test",
	"_id" : "1",
	"_score" : 0.15342641,
	"_source" : {
		"hello" : "a fox run"
		}
	}

##funzzy_lke_this
fuzzy_like_this查询基于模糊串，并选择其产生的最好的区分词项。例如，在title和otitle字段上进行fuzzy_like_this查询，并得到与crime publishment相似的所有的查询语句如下:
{
    "query": {
        "fuzzy_like_this": {
            "fields": [
                "hello"
            ],
            "like_text": "app"
        }
    }
}



支持以下参数:

* fields: 指定在哪些字段上执行查询。默认是_all
* like_text: 该参数是必须得，指明文档应比较的内容
* ignore_tf: 指定是否忽略词项的频次，默认为false
* max_query_terms: 指明在查询中查询词项的最大数目，默认是25.
* min_similarity: 指明区分词项应该有的最小相似度，默认为0.5
* prefix_length: 指明区分词项的共同前缀的长度，默认为0.
* boost: 指定boost取值。
* analyzer: 指定分词器。

查到下面的:
	{
	"_index" : "test",
	"_type" : "test",
	"_id" : "2",
	"_score" : 0.15342641,
	"_source" : {
		"hello" : "an apple drop, i ran"
		}
	}


