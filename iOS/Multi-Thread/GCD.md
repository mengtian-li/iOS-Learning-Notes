## 1. 参考
[Grand Central Dispatch Tutorial for Swift: Part 1/2](https://www.raywenderlich.com/79149/grand-central-dispatch-tutorial-swift-part-1)

## 2. Serial vs. Concurrent
- Serial 任务串行执行
- Concurrent 任务并行执行 

## 3. Synchronous vs. Asynchronous
- Syschronous 同步，GCD方法，等待任务执行完成才返回
- Asynchronous 异步，不等待任务完成，立即返回，不会阻塞GCD方法的线程

## 4. Critical Section
临界区，atomic 只能保证property的setter getter方法为原子操作   
临界区一般需要加锁:
- @synchronized(token)
- NSLock
- dispatch_semaphore_t

## 5. Queues
- FIFO 

### 5.1 Serial Queue
- 任务一个接一个完成

### 5.2 Concurrent Queue
- 任务开始顺序按照添加进队列的顺序，任务完成顺序不一定，并行执行

### 5.3 Queue Tyeps

#### 5.3.1 Main Queue
```
dispatch_queue_t dispatch_get_main_queue();
```
- Serial Queue
- tasks excute on main thread (UI thread)

#### 5.3.2 Global Queue
```
dispatch_queue_t dispatch_get_global_queue(long identifier, unsigned long flags);
```
- Concurrent Queue
##### identifier
- QOS_CLASS_USER_INTERACTIVE
> The user interactive class represents tasks that need to be done immediately in order to provide a nice user experience. Use it for UI updates, event handling and small workloads that require low latency. The total amount of work done in this class during the execution of your app should be small.
- QOS_CLASS_USER_INITIATED
> The user initiated class represents tasks that are initiated from the UI and can be performed asynchronously. It should be used when the user is waiting for immediate results, and for tasks required to continue user interaction.
- QOS_CLASS_UTILITY
> The utility class represents long-running tasks, typically with a user-visible progress indicator. Use it for `computations, I/O, networking, continous data feeds and similar tasks`. This class is designed to be `energy efficient`.
- QOS_CLASS_BACKGROUND
> The background class represents tasks that the user is not directly aware of. Use it for prefetching, maintenance, and other tasks that don’t require user interaction and aren’t time-sensitive.

##### flags
预留，填0

#### 5.3.3 Custom Queue
```
dispatch_queue_t dispatch_queue_create(const char *label, dispatch_queue_attr_t attr);
```

##### label 
一半反域名，为队列设置一个标签
##### attr
- DISPATCH_QUEUE_SERIAL
- DISPATCH_QUEUE_CONCURRENT

## 6. Dispatch 方法

### 6.1 dispatch_async

### 6.2 dispatch_sync

### 6.3 dispatch_after
```
 void dispatch_after(dispatch_time_t when, dispatch_queue_t queue, dispatch_block_t block);
```
- `when` `dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delayInSeconds * NSEC_PER_SEC))`
- `queue` `最好是主队列`

### 6.4 dispatch_once
- Executes a block object once and only once for the lifetime of an application.
```
+ (instancetype)sharedInstance {
    static MTSharedInstance *sharedInsatnce = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInsatnce = [[MTSharedInstance alloc] init];
    });
    
    return sharedInsatnce;
}
```


### 6.5 dispatch_barrier
> Dispatch barriers are a group of functions acting as a serial-style bottleneck when working with concurrent queues. Using GCD’s barrier API ensures that the submitted block is the only item executed on the specified queue for that particular time. This means that all items submitted to the queue prior to the dispatch barrier must complete before the block will execute.
When the block’s turn arrives, the barrier executes the block and ensures that the queue does not execute any other blocks during that time. Once finished, the queue returns to its default implementation.
- 最好用于自定义并行队列，系统并行队列是全局的，可能会影响其他的队列

#### 6.5.1 dispatch_barrier_sync
等闭包执行完之后返回

#### 6.5.1 dispatch_barrier_async
立即返回

## 7. Dispatch Group

### 7.1 创建dispatch group
```
dispatch_group_t group = dispatch_group_create();
```
### 7.2 dispatch_group_enter
- 任务进入group
- dispatch_group_enter 的数目与dispatch_group_leave 必须要匹配上

### 7.3 dispatch_group_leave
- 通知group 任务完成

### 7.4 dispatch_group_wait
```

```
