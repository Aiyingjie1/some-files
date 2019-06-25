# JDK1.8

#### List转MAP，并按照一个字段分组

~~~java
Map<Long, AgentQueueSessionCount> agentQueueSessionWaitCountMap =
            agentQueueSessionWaitCountList.stream().collect(Collectors.toMap(AgentQueueSessionCount::getQueueId, agentQueueSessionWaitCount -> agentQueueSessionWaitCount));
        
~~~



#### No identifier specified for entity 错误

Entity 类中加@Id；