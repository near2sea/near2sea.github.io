---
layout: post
title: "EasyReact初识"
date: 2018-10-25 10:03:04 +0800
comments: true
categories: 
---
## 美团客户端响应式框架 EasyReact 
### 前言
EasyReact 是一款基于响应式编程范式的客户端开发框架，开发者可以使用此框架轻松地解决客户端的异步问题。

目前 EasyReact 已在美团和大众点评客户端的部分业务中实践，并且持续迭代了一年多的时间。近日，我们决定开源这个项目的 iOS Objective-C 语言部分，希望能够帮助更多的开发者不断探索更广泛的业务场景，也欢迎更多的社区的开发者跟我们一起加强 EasyReact 的功能。Github 的项目地址，参见 https://github.com/meituan/EasyReact 。
<!-- more -->

## 响应式编程的学习门槛
前面已经分析过，单纯的响应式编程并不是特别的难以理解，而函数式编程才是造成高学习门槛的原因。因此 EasyReact 采用大家都熟知的面向对象编程进行设计，

想要了解代码，相对于函数式编程变得容易很多。

另外响应式编程基于数据流动，流动就会产生一个有向的流动网络图。在函数式编程中，网络图是使用闭包捕获来建立的，这样做非常不利于图的查找和遍历。而 EasyReact 选择在框架中使用图的数据结构，将数据流动的有向网络图抽象成有向有环图的节点和边。这样使得框架在运行过程中可以随时查询到节点和边的关系，详细内容可以参见 框架概述 。

另外对于已经熟悉了 ReactiveCocoa 的同学来说，我们也在数据的流动操作上基本实现了 ReactiveCocoa API。详细内容可以参见 基本操作 。更多的功能可以向我们提功能的 ISSUE ，也欢迎大家能够提 Pull Request 来共同建设 EasyReact。

## EasyReact的最佳实践

通常我们创建一个类，里面会包含很多的属性。在使用 EasyReact 时，我们通常会把这些属性包装为 `EZRNode` 并加上一个泛型。如：

``` objc SearchService.h
@interface SearchService : NSObject

@property (nonatomic, readonly, strong) EZRMutableNode *param;
@property (nonatomic, readonly, strong) EZRNode *result;
@property (nonatomic, readonly, strong) EZRNode *error;

@end
```

这段代码展示了如何创建一个 WiKi 查询服务，该服务接收一个 `param` 参数，查询后会返回 `result` 或者 `error`。以下是实现部分：

```objc SearchService.m
@implementation SearchService

- (instancetype)init {
    if (self = [super init]) {
        _param = [EZRMutableNode new];
        EZRNode *resultNode = [_param flattenMap:^EZRNode * _Nullable(NSString * _Nullable searchParam) {
            NSString *queryKeyWord = [searchParam stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet  URLQueryAllowedCharacterSet]];
            NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"https://en.wikipedia.org/w/api.php?action=query&titles=%@∝=revisions&rvprop=content&format=json&formatversion=2", queryKeyWord]];
            EZRMutableNode *returnedNode = [EZRMutableNode new];
            [[NSURLSession sharedSession] dataTaskWithURL:url completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
                if (error) {
                    returnedNode.value = error;
                } else {
                    NSError *serializationError;
                    NSDictionary *resultDictionary = [NSJSONSerialization JSONObjectWithData:data options:0 error:&serializationError];
                    if (serializationError) {
                        returnedNode.value = serializationError;
                    } else if (!([resultDictionary[@"query"][@"pages"] count] && !resultDictionary[@"query"][@"pages"][0][@"missing"])) {
                        NSError *notFoundError = [NSError errorWithDomain:@"com.example.service.wiki" code:100 userInfo:@{NSLocalizedDescriptionKey: [NSString stringWithFormat:@"keyword '%@' not found.", searchParam]}];
                        returnedNode.value = notFoundError;
                    } else {
                        returnedNode.value = resultDictionary;
                    }
                }
            }];
            return returnedNode;
        }];
        EZRIFResult *resultAnalysedNode = [resultNode if:^BOOL(id  _Nullable next) {
            return [next isKindOfClass:NSDictionary.class];
        }];
        _result = resultAnalysedNode.thenNode;
        _error = resultAnalysedNode.elseNode;
    }
    return self;
}

@end

```
在调用时，我们只需要通过 `listenedBy` 方法关注节点的变化：

```objc ViewController.m
self.service = [SearchService new];
[[self.service.result listenedBy:self] withBlock:^(NSDictionary * _Nullable next) {
    NSLog(@"Result: %@", next);
}];
[[self.service.error listenedBy:self] withBlock:^(NSError * _Nullable next) {
    NSLog(@"Error: %@", next);
}];

self.service.param.value = @"mipmap"; //should print search result
self.service.param.value = @"420v"; // should print error, keyword not found.
```

使用 EasyReact 后，网络请求的参数、结果和错误可以很好地被分离。不需要像命令式的写法那样在网络请求返回的回调中写一堆判断来分离结果和错误。

因为节点的存在先于结果，我们能对暂时还没有得到的结果构建连接关系，完成整个响应链的构建。响应链构建之后，一旦有了数据，数据便会自动按照我们预期的构建来传递。

在这个例子中，我们不需要显式地来调用网络请求，只需要给响应链中的 param 节点赋值，框架就会主动触发网络请求，并且请求完成之后会根据网络返回结果来分离出 result 和 error 供上层业务直接使用。

