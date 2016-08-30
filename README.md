Web Image
=========

这段时间有一些空闲时间， 所以就仔细阅读了源码。读完之后觉得受益匪浅。
有几个重要点：

枚举的多用法 (在SDWebImageManager中就大量的用到类型的判断)
----------
```objective-c
//枚举定义
typedef NS_OPTIONS(NSUInteger, AKDirection) {
    AKDirectionT  = 1<<0,
    AKDirectionB  = 1<<1,
    AKDirectionL  = 1<<2   ,
    AKDirectionR  = 1<<3

};

  AKDirection dir2= AKDirectionT;
        AKDirection dir3 = dir2 & AKDirectionL; //0
        AKDirection dir4 = dir2 & AKDirectionT; //AKDirectionT。约等于 dir2 == AKDirectionT

        //addDir 可以保存 多种类型。可以添加 移除对应类型
        AKDirection addDir = 0;
        //addDir添加AKDirectionL类型
        addDir |=AKDirectionL;
        addDir |=AKDirectionT;

        if (addDir & AKDirectionL) {
            NSLog(@"addDir include AKDirectionL");
        }
        if (!(addDir & AKDirectionB)) {
            NSLog(@"addDir NOT include AKDirectionB");
        }

        //addDir 移除AKDirectionL
        addDir &= ~AKDirectionL;
        if (!(addDir & AKDirectionL)) {
            NSLog(@"addDir remove AKDirectionL");
        }
```


自定义NSOperation
----------
SDWebImageDownloader中维护了一个并发队列downloadQueue，队列中是自定义SDWebImageDownloaderOperation。
SDWebImageDownloaderOperation 负责了单个图片的下载（sessiontask）和各种下载完成回调。


缓存模块SDImageCache
----------
其中通过系统NSCache处理app在运行中的缓存管理。文件本地缓存，则是通过对Cache文件目录下添加对应对目录和图片文件。
请求缓存，则是通过NSURLRequest的缓存策略来判断是否NSURLRequestUseProtocolCachePolicy 还是NSURLRequestReloadIgnoringLocalCacheData。

-NSURLRequestUseProtocolCachePolicy = 0, 默认的缓存策略， 如果缓存不存在，直接从服务端获取。如果缓存存在，会根据response中的Cache-Control字段判断下一步操作
-NSURLRequestReloadIgnoringLocalCacheData 忽略本地缓存数据，直接请求服务端.




阅读过程中，并在源码中添加了的注释，欢迎纠错！










  
