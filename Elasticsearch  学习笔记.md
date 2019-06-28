# Elasticsearch  学习笔记

## 基本概念

[curl全解](https://www.jianshu.com/p/07c4dddae43a)

**节点(node)**是一个运行着的Elasticsearch实例。

**集群(cluster)**是一组具有相同`cluster.name`的节点集合，他们协同工作，共享数据并提供故障转移和扩展功能，当然一个节点也可以组成一个集群。

### 文档（Document）

这里我理解的文档，就是原来在关系型数据库的情况下，一条数据存储时会被分在不同的数据库里，查询时会从不同的数据库聚合数据。而在ES中，一条数据不需要被拆分，直接可以储存，相同的数据被存储到一个文档下面，可以方便被检索、排序、过滤。

通常对象和文档在某种程度上是等价的，只是表现的形式不同。文档特指最顶层结构或者**根对象(root object)**序列化成的JSON数据（以唯一ID标识并存储于Elasticsearch中。

一个文档不仅包括数据这个主题，还包括一些 `metadata `:

 - _index:索引
   1. 名字必须是全部小写
   2. 不能以下划线开头
   3. 不能包含逗号
 - _type:每个**类型(type)**都有自己的**映射(mapping)**或者结构定义，就像传统数据库表中的列一样。所有类型下的文档被存储在同一个索引下，但是类型的**映射(mapping)**会告诉Elasticsearch不同的文档如何被索引
   1. 名字可以是大写或小写
   2. 不能包含下划线或逗号
	- _id:**id**仅仅是一个字符串，它与`_index`和`_type`组合时，就可以在Elasticsearch中唯一标识一个文档。当创建一个文档，你可以自定义`_id`，也可以让Elasticsearch帮你自动生成

如下例子展示了文档的本质：

```
-x
```

## 分词插件安装

[权威指南](https://es.xiaoleilu.com/010_Intro/20_Document.html)

[阮一峰](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)

```
./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.0/elasticsearch-analysis-ik-6.8.0.zip
```

## 关闭ES

- ctrl+c
- `curl -XPOST 'http://localhost:9200/_shutdown' `

## 查询

```
curl -H "Content-Type: application/json" -XGET 'http://localhost:9200/_count?pretty' -d '
{
    "query": {
        "match_all": {}
    }
}
```

注意：上述中6.X版本开始需要加入头信息，否则会报不识别格式的错误，这是新版ES的更加安全的考虑所致。