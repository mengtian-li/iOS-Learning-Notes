```
@interface MTA : NSObject

- (void)method1;
- (void)method2;
- (void)method3;

@end

@implementation MTA

- (void)method1 {
    NSLog(@"Class A method1 .");
}

- (void)method2 {
    NSLog(@"Class A method2 .");
}

- (void)method3 {
    NSLog(@"Class A method3 .");
}

@end

@interface MTB : MTA

@end

@implementation MTB

- (void)method1 {
    NSLog(@"Class B method1 .");
}

- (void)method3 {
    ///// self
//    [self method1];
//    [self method2];
    ///// super
    [super method1];
    [super method2];
}

@end

@interface MTC : MTB

@end

@implementation MTC

- (void)method2 {
    NSLog(@"Class C method2 .");
}

@end

```

- self指的是运行时，当前接收消息的对象
- super是字啊编译时，类的继承关系
