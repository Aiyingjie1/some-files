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
