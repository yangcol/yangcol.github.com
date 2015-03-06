---
layout: post
category : 搜索
tagline: "elasticsearch中的一些小细节"
tags : [fuzzy_like_this, elasticsearch, more_like_this]
---

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

curl -XPUT localhost:9200/test
curl -XPUT localhost:9200/test/test/1 -d '{"hello":"a fox run"}'
curl -XPUT localhost:9200/test/test/2 -d '{"hello":"an apple drop, i ran"}'

## more_like_this的查询用例
{
    "query": {
        "more_like_this": {
            "like_text": "run",
            "min_term_freq": 1,
            "min_doc_freq": 1
        }
    }
}