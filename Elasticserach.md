#Elasticserach

Spark从入门到精通
链接：https://pan.baidu.com/s/166CoB8YoZcR_61ZnLk1SFg 
提取码：cwo2 


Elasticsearch顶尖高手系列课程
链接：https://pan.baidu.com/s/1troauC1YKF98da7e5BvdcA 
提取码：smf6 


亿级流量电商详情页系统实战（第二版）：缓存架构+高可用服务架构+微服务架构
链接：https://pan.baidu.com/s/1OLpxTKDjfVstVIY8VxYpwA 

提取码：mgyg 





docker exec -it 镜像名 bash      ---可以进入镜像的目录

@Document注解里面的几个属性，类比mysql的话是这样： 
index –> DB 
type –> Table 
Document –> row 

@Id注解加上后，在Elasticsearch里相应于该列就是主键了，在查询时就可以直接用主键查询，后面一篇会讲到。其实和mysql非常类似，基本就是一个数据库。

```
curl 'localhost:9200/_cat/indices?v'
curl -XGET 'http://localhost:9200/_mapping?pretty=true'
```




~~~java
（1）统计某个字段的数量  
  ValueCountBuilder vcb=  AggregationBuilders.count("count_uid").field("uid");  
（2）去重统计某个字段的数量（有少量误差）  
 CardinalityBuilder cb= AggregationBuilders.cardinality("distinct_count_uid").field("uid");  
（3）聚合过滤  
FilterAggregationBuilder fab= AggregationBuilders.filter("uid_filter").filter(QueryBuilders.queryStringQuery("uid:001"));  
（4）按某个字段分组  
TermsBuilder tb=  AggregationBuilders.terms("group_name").field("name");  
（5）求和  
SumBuilder  sumBuilder= AggregationBuilders.sum("sum_price").field("price");  
（6）求平均  
AvgBuilder ab= AggregationBuilders.avg("avg_price").field("price");  
（7）求最大值  
MaxBuilder mb= AggregationBuilders.max("max_price").field("price");   
（8）求最小值  
MinBuilder min= AggregationBuilders.min("min_price").field("price");  
（9）按日期间隔分组  
DateHistogramBuilder dhb= AggregationBuilders.dateHistogram("dh").field("date");  
（10）获取聚合里面的结果  
TopHitsBuilder thb=  AggregationBuilders.topHits("top_result");  
（11）嵌套的聚合  
NestedBuilder nb= AggregationBuilders.nested("negsted_path").path("quests");  
（12）反转嵌套  
AggregationBuilders.reverseNested("res_negsted").path("kps ");  
~~~





### ES的CURL

es中所有的索引库名称必须小写，不能以下划线开头，也不能包含逗号



###### 创建一个索引库（PUT创建索引库）：

~~~
curl -XPUT http://uplooking01:9200/bigdata
返回值{"acknowledged":"true"}
~~~

###### 查看一个索引库的信息（GET查看）：

~~~
curl -XGET http://uplooking01:9200/bigdata
返回值一个json字符串
如果想要格式良好的结果
curl -XGET http://uplooking01:9200/bigdata?pretty
~~~

###### 在索引库中添加若干索引信息（POST添加信息）：

~~~
curl -XPOST http://uplooking01:9200/bigdata/product/1 -d'{
    "name":"hadoop",
    "author":"Dong Couting",
    "version":"2.9.4"
}'  
curl -XPOST http://uplooking01:9200/bigdata/product/2 -d'{
    "name":"hive",
    "author":"facebook",
    "version":"2.1.0",
    "url":"http://hive.apache.org"
}'
curl -XPUT http://uplooking01:9200/bigdata/product/3 -d'{
    "name":"hbase",
    "author":"apache",
    "version":"1.1.5",
    "url":"http://hbase.apache.org"
}'  
~~~

###### 查询一个索引库下面的所有数据（GET查看）：

~~~
curl -XGET http://uplooking01:9200/bigdata/product/_search
格式良好的写法：
curl -XGET http://uplooking01:9200/bigdata/product/_search?pretty
{
  "took" : 11,
  "timed_out" : false,  ---->是否超时
  "_shards" : {         ---->分片个数(就是kafka中的partition，一个索引库有几个分区)
    "total" : 5,        ---->默认分片有5个
    "successful" : 5,   ---->能够正常提供服务的有5个
    "failed" : 0        ----> (total-successful)=failed
  },
  "hits" : {            ---->查询结果集
    "total" : 2,        ---->查询到了几条记录
    "max_score" : 1.0,  ---->记录中最大的得分
    "hits" : [ {        ---->具体的结果集数组
      "_index" : "bigdata", ----->结果所在索引库
      "_type" : "product",  ----->结果所在索引库中的某一个类型type
      "_id" : "2",          ----->结果索引id
      "_score" : 1.0,       ----->索引得分
      "_source" : {         ----->索引具体内容_source
        "name" : "hive",        
        "author" : "facebook",
        "version" : "2.1.0",
        "url" : "http://hive.apache.org"
      }
    }, {
      "_index" : "bigdata",
      "_type" : "product",
      "_id" : "1",
      "_score" : 1.0,
      "_source" : {
        "name" : "hadoop",
        "author" : "Dong Couting",
        "version" : "2.9.4"
      }
    } ]
  }
}


使用POST方式查询多条数据（注意不是使用get，注意''）
curl -XPOST 'http://localhost:9200/agentserve/agentserve/_search?pretty' -d' 
{"query":{"bool":{"must":[{"match_all":{}}],"must_not":[],"should":[]}},"from":0,"size":50,"sort":[],"aggs":{}}'
~~~

​	PUT和DELETE都是幂等操作。幂等操作就是只不管重复进行多少次，结果都是一样的。POST不是幂等操作。

​	创建操作可以使用POST或者使用PUT，POST作用在一个集合资源上，而PUT操作是作用在具体的一个资源上，很多资源使用数据库自增主键作为标识信息，这个时候就需要使用PUT了。而创建的资源的标识信息到底是什么，只能由服务端提供时，这个时候就必须使用POST。

###### 只查询source中的个别字段（只查了name，author）：

~~~
curl -XGET 'http://uplooking01:9200/bigdata/product/_search?_source=name,author&pretty'
~~~

###### 返回结果结果只查询source：

~~~
 curl -XGET 'http://uplooking01:9200/bigdata/product/1?_source&pretty'
~~~

###### 条件查询：

~~~
curl -XGET 'http://uplooking01:9200/bigdata/product/_search?q=name:hbase&pretty'
        查询name为hbase的结果
    curl -XGET 'http://uplooking01:9200/bigdata/product/_search?q=name:h*&pretty'
        查询name以h打头的结果
~~~

###### 分页查询：

~~~
curl -XGET 'http://uplooking01:9200/bank/acount/_search?pretty&from=0&size=5'
~~~

​	ES可以使用PUT或者POST对文档进行更新，如果指定ID的文档已经存在，则执行更新操作。
​	注意：执行更新操作的时候，ES首先将旧的文档标记为删除状态，然后添加新的文档，旧的文档不会立即消失，但是你也无法访问，**ES会继续添加更多数据的时候在后台清理已经标记为删除状态的文档。**

###### **普通删除，根据主键删除：**

~~~
 curl -XDELETE http://localhost:9200/bigdata/product/3/
 说明：如果文档存在，es属性found：true，successful:1，_version属性的值+1。
   如果文档不存在，es属性found为false，但是版本值version依然会+1，这个就是内部
管理的一部分，有点像svn版本号，它保证了我们在多个节点间的不同操作的顺序被正确标记了。
   注意：一个文档被删除之后，不会立即生效，他只是被标记为已删除。ES将会在你之后添加
更多索引的时候才会在后台进行删除。
~~~

###### 批量操作bulk:

~~~
格式：
   action:[index|create|update|delete]
   metadata:_index,_type,_id
   request body:_source(删除操作不需要)
   {action:{metadata}}\n
   {request body}\n
   
   {action:{metadata}}\n
   {request body}\n
   
   
例如（JSON脚本文件）：
{"index":{"_id":"1"}}   {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}

{"index":{"_id":"6"}}
{"account_number":6,"balance":5686,"firstname":"Hattie","lastname":"Bond","age":36,"gender":"M","address":"671 Bristol Street","employer":"Netagy","email":"hattiebond@netagy.com","city":"Dante","state":"TN"}

使用文件的方式
curl -XPOST http://uplooking01:9200/bank/acount/_bulk --data-binary @/home/uplooking/data/accounts.json
~~~

######  create和index的区别：

​	如果数据存在，使用create操作失败，会提示文档已经存在，使用index则可以成功执行。



Bulk会把将要处理的数据载入内存中，所以数据量是有限制的。最佳的数据量不是一个确定的数值，它取决于你的硬件，你的文档大小以及复杂性，你的索引以及搜索的负载。一般建议是1000~5000个文档，如果你的文档很大，可以适当减少队列，大小建议是5~15MB，默认不能超过100M，可以在es的配置文件中修改这个值：
**http.max_content_length:100mb**

