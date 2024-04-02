# 将当前线程的Request上下文传递到子线程中

[java - How to enable request scope in async task executor - Stack Overflow](https://stackoverflow.com/questions/23732089/how-to-enable-request-scope-in-async-task-executor?newreg=cef88ba81efc416babcb819b532e4592)

Executor:

```java
public class ContextAwarePoolExecutor extends ThreadPoolTaskExecutor {
    @Override
    public <T> Future<T> submit(Callable<T> task) {
        return super.submit(new ContextAwareCallable(task, RequestContextHolder.currentRequestAttributes()));
    }

    @Override
    public <T> ListenableFuture<T> submitListenable(Callable<T> task) {
        return super.submitListenable(new ContextAwareCallable(task, RequestContextHolder.currentRequestAttributes()));
    }
}
```

Callable:

```java
public class ContextAwareCallable<T> implements Callable<T> {
    private Callable<T> task;
    private RequestAttributes context;

    public ContextAwareCallable(Callable<T> task, RequestAttributes context) {
        this.task = task;
        this.context = context;
    }

    @Override
    public T call() throws Exception {
        if (context != null) {
            RequestContextHolder.setRequestAttributes(context);
        }

        try {
            return task.call();
        } finally {
            RequestContextHolder.resetRequestAttributes();
        }
    }
}
```

Configuration:

```java
@Configuration
public class ExecutorConfig extends AsyncConfigurerSupport {
    @Override
    @Bean
    public Executor getAsyncExecutor() {
        return new ContextAwarePoolExecutor();
    }
}
```
