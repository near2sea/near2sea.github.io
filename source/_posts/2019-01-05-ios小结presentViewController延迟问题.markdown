---
title: ios小结presentViewController延迟问题
tags: ios
categories:
  - 前端
abbrlink: 21826
date: 2019-01-05 17:17:23
---

# iOS小结(解决presentViewController延迟问题)

在 iOS 中，当使用

```
-(void)presentViewController:(UIViewController*)viewControllerToPresent animated:(BOOL)flag completion:(void (^__nullable)(void))completion
```

方法进行界面跳转的时候，有时候会出现延迟，这个延迟有时候会有好几秒的时间才会执行 completion，有时候干脆就一直不会跳转。

* 由于某种原因，`presentViewController`跳转时completion的内容并不会真的马上触发执行，除非有一个主线程事件触发这种消费。比如在弹出慢的时候，你随便点击一下屏幕，马上就能弹出来

所以得出相应的解决方法：

1.在主线程中执行跳转：

```
dispatch_async(dispatch_get_main_queue(), ^(void){
            [currentVC presentViewController:presentVC animated:YES completion:^{
            }];
        });
```

2.在执行跳转前唤醒主线程,`WakeUpMainThread`方法它的作用只是唤醒主线程

```
[self performSelectorOnMainThread:@selector(WakeUpMainThread) withObject:nil waitUntilDone:NO];
```