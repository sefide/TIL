
_CompletableFuture#join_
이벤트가 완료되거나 혹은 exception이 던져졌을 때 결과가 반환된다. 
일반적인 함수 형식의 사용을 보다 잘 준수하기 위해서, CompletableFuture의 완료와 관련된 계산에서 예외가 발생하면 이 메서드는 기본 예외를 원인으로 하는 CompletionException을 던진다. 
``` java
public T join() {
    Object r;
    return reportJoin((r = result) == null ? waitingGet(false) : r);
}
```


_ForkJoinPool.managedBlock(q)_ <br>
주어진 동기 태스트를 수행한다. ForkJoinPool에 태스크가 수행되고 있고, 만약 현 스레드가 블락되는 동안 충분한 병렬성이 보장되어야 한다면 이 메소드는 활성화되기 위해 여분의 스레드로 재배열 된다. 

_CompletableFuture#supplyAsync(Supplier<U> supplier, Executor executor)_
  
``` java
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier,
                                                   Executor executor) {
    return asyncSupplyStage(screenExecutor(executor), supplier);
}
```


_CompletableFuture#supplyAsync(Supplier<U> supplier)_
  
``` java
private static final Executor asyncPool = useCommonPool ?
    ForkJoinPool.commonPool() : new ThreadPerTaskExecutor();

public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier) {
    return asyncSupplyStage(asyncPool, supplier);
}
```

Exceutors#newFixedThreadPool
```
public static ExecutorService newFixedThreadPool(int nThreads) { 
	return new ThreadPoolExecutor(nThreads, nThreads,
		 0L, TimeUnit.MILLISECONDS, 
		new LinkedBlockingQueue<Runnable>()); 
}
```

---
**참고** <br>
https://hamait.tistory.com/937
