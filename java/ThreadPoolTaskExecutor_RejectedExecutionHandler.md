# ThreadPoolTaskExecution RejectedExecutionHandler


### what
요청이 큐의 크기보다 많이 발생한 경우 정해둔 reject policy에 따라 task를 거절할지 어떻게 수행할지 정해진다. 

<br>

#### ThreadPoolExecutor에서 기본적으로 제공하는 RejectedExecutionHandler
- **AbortPolicy** : 기본 설정값으로, TaskRejectException을 반환하며 Task를 수행을 중단한다.
```
public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
    throw new RejectedExecutionException("Task " + r.toString() +
                                         " rejected from " +
                                         e.toString());
}
```

- **CallerRunsPolicy** : TaskExecutor가 아닌 호출한 Thread에서 해당 Task를 직접 수행한다.
```
public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
    if (!e.isShutdown()) {
        r.run();
    }
}
```

- **DiscardPolicy** : Exception을 던지지 않으며 요청이 새로 들어온 Task를 거절한다.
```
public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
}
```

- **DiscardOldestPolicy** : 오래 기다리고 있던 Task를 Queue에서 제거하고 새로운 Task를 큐에 넣는다.
```
public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
    if (!e.isShutdown()) {
        e.getQueue().poll();
        e.execute(r);
    }
}
```

- **커스텀 RejectedExecutionHandler** : RejectedExecutionHandler를 상속하고 rejectedExecution 메서드에 Task를 거절할 때 동작할 수행 내용을 선언해주면 된다.

<br>


### how

- corePoolSize : 기본 thread 개수
- maxPoolSize : 최대 thread 개수
- queueCapacity : 큐 개수
- rejectExecutionHandler : 요청이 큐 이상 발생 시 task 거절 시 수행 동작 핸들러 설정

<br>

java
```java 
 @Bean(name = "threadPoolTaskExecutor")
    public Executor threadPoolTaskExecutor() {
        ThreadPoolTaskExecutor threadPoolExecutor = new ThreadPoolTaskExecutor();
        threadPoolExecutor.setCorePoolSize(10);
        threadPoolExecutor.setMaxPoolSize(20);
        threadPoolExecutor.setQueueCapacity(10);
        threadPoolExecutor.setThreadNamePrefix("Executor-");
        threadPoolExecutor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        threadPoolExecutor.initialize();

        return threadPoolExecutor;
    }
```


xml
```xml
<bean id="threadPoolTaskExecutor" class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor"> 
  <property name="corePoolSize" value="1"/> 
  <property name="maxPoolSize" value="5"/> 
  <property name="queueCapacity" value="100"/> 
  <property name="threadNamePrefix" value="Executor-"/> 
  <property name="rejectExecutionHandler">
    <bean class="java.util.concurrent.ThreadPoolExecutor$DiscardPolicy>
  </property>
</bean>
```
