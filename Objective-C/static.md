
### 1. `.m` 文件里定义static 常量
```
// .m

static const int kConstant = xxx;

@implementation ...

@end

```
- 该文件里全局可见 (准确说应该是compilation unit，而不是文件)
- 外部文件不能通过`extern`引用

### 2. `.m` 文件里定义static 变量
```
// .m

static int intVar = xxx;

@implementation ...

@end

// .h
//实现类似类变量的机制
+ (int)intVar;
+ (void)setIntVar:(int)intVar;

```
- OC里没有类变量的概念，但通过静态的全局变量，可以实现类似类变量的东西
    - 因为一般情况下，OC里的类以文件为单位，划分。通过给类添加，get 和 set的类方法，去实际操作这个全局变量，就可以实现类似类变量的机制。
- 外部文件不能通过extern引用




### 3. `.h` 里定义static 常量
```
// .h

static const int kConstatnt = 1;

```

[Variable declarations in header files - static or not?](http://stackoverflow.com/a/92641/4635964)

> 这里static，意味着，每一个包含这个头文件的源文件里，会包含一个这个常量的拷贝，但与此同时，链接时，并不会出现符号冲定义的错误。

- 外部文件，可以通过引用该头文件，使用这个全局常量，但这里获得的是该常量的一份，拷贝，因为常量并不能修改，所以获取的是拷贝，也没有太多问题
- 如果这里，去掉这个static，外部文件引用这个头文件，就会出现符号重定义的链接错误
- 但是这种实现全局常量的方式不太好


### 4. `.h` 里定义static 变量
```
// .h

static int intVar;


```

- 这里同在`.h`里定义static 常量一样，每一个引用该头文件的源文件，获取到的是这个变量的一份拷贝，也就是说你在不同的文件里，修改这个变量，都是修改当前文件里的一份拷贝，只会在当前这个文件范围里生效

```
// a.h 

static int a = 0;  //定义了一个全局变量
 
@interface A: NSObject

+ (void)printA; //打印a的值

@end

// a.m

@implementation 

+ (void)printA {
    NSLog("%d",a);
}

@end


// b.h

@interface B: NSObject

+ (void)printA; //打印a的值

@end


// b.m 
#import "a.h" //引入了全局变量 a

@implementation 

+ (void)printA {
    a ++;
    NSLog("%d",a);
}

@end

// main.m

[A printA];  // 1 
[B printA];  // 2
[A printA];  // 1
[B printA];  // 3
[A printA];  // 1
[B printA];  // 4


```

- 所以这种方式，定义的变量，并不是真正的全局变量。


### 5.更优雅的方式，定义全局变量和全局常量

- 在`.m`文件里，定义全局变量，和全局常量，不用static修饰
- 在`.h`文件里，用extern声明这些全局变量和全局
- 其他文件引用这个`.h`来使用相应的常量和变量。
```
// .m
int a;

// .h
extern int a;

```

