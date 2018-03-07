## 1.NSOperation

- `NSOperation`是一个抽象类，一般通过继承或者使用`Foundation`框架提供的两个子类`NSBlockOperation`、`NSInvocationOperation`

### 依赖
```
- addDependency:
```
- 添加一对一的约束，当前Operation需要在target Operation结束之后才能开始执行
- 依赖是由Operation管理的，不依赖于任何Queue，即两个不同队列的Operation也可以添加依赖
- 当一个Operation的所有依赖都执行完之后，Operation处于ready状态，可以重写isReady方法，实现自定义决定Operation的ready时机

### 优先级
```
- setQueuePriority:
```
- 对于添加进队列的Operation，执行顺序，首先取决于任务是否处于ready状态（取决于依赖），其次是Operation的优先级

### service levels
```
@property NSQualityOfService quali†yOfService;
```
#### enum
- NSQualityOfServiceUserInteractive
> UserInteractive QoS is used for work directly involved in providing an interactive UI such as processing events or drawing to the screen. 
- NSQualityOfServiceUserInitiated
> UserInitiated QoS is used for performing work that has been explicitly requested by the user and for which results must be immediately presented in order to allow for further user interaction.  For example, loading an email after a user has selected it in a message list.
- NSQualityOfServiceUtility
> Utility QoS is used for performing work which the user is unlikely to be immediately waiting for the results.  This work may have been requested by the user or initiated automatically, does not prevent the user from further interaction, often operates at user-visible timescales and may have its progress indicated to the user by a non-modal progress indicator.  This work will run in an energy-efficient manner, in deference to higher QoS work when resources are constrained.  For example, periodic content updates or bulk file operations such as media import.
- NSQualityOfServiceBackground
> Background QoS is used for work that is not user initiated or visible.  In general, a user is unaware that this work is even happening and it will run in the most efficient manner while giving the most deference to higher QoS work.  For example, pre-fetching content, search indexing, backups, and syncing of data with external systems. 
- NSQualityOfServiceBackground
> Background QoS is used for work that is not user initiated or visible.  In general, a user is unaware that this work is even happening and it will run in the most efficient manner while giving the most deference to higher QoS work.  For example, pre-fetching content, search indexing, backups, and syncing of data with external systems.
- NSQualityOfServiceBackground
> Default QoS indicates the absence of QoS information.  Whenever possible QoS information will be inferred from other sources.  If such inference is not possible, a QoS between UserInitiated and Utility will be used.

### Completion Block
```
- setCompletionBlock:
```


## 2.自定义Opertaion
- 一般情况下Opertation不需要并发，除非通过调用`start`执行一个Operation而不是添加到一个Queue中

### 2.1 非并发Operation
- 实现一个自定义初始化方法
- 实现`main`方法

### 2.2 并发Operation
- start（必选）
- main（可选）
- isExceuting（必选）
- isFinisheed（必选）
- isConcurrent（必选）
