# AFNetWorking详解

## Foundation框架提供的网络请求类NSURLSession
**使用demo**
```
    NSString *urlString = @"https://static001.geekbang.org/univer/classes/ios_dev/lession/45/toutiao.json";
    NSURL *url = [NSURL URLWithString:urlString];
    NSURLSession *session = [NSURLSession sharedSession];
    __weak typeof(self) weakSelf = self;
    NSURLSessionDataTask *dataTask = [session dataTaskWithURL:url completionHandler:^(NSData *_Nullable data, NSURLResponse *_Nullable response, NSError *_Nullable error) {
        __strong typeof(weakSelf) strongSelf = weakSelf;
        
        //to convert binary data as JSON data
        NSError *jsonError;
        id jsonObj = [NSJSONSerialization JSONObjectWithData:data options:0 error:&jsonError];
        //typecasting twice to convert json data as NSArray
        NSArray *dataArray =  [((NSDictionary *)[((NSDictionary *)jsonObj) objectForKey:@"result"]) objectForKey:@"data"];
        NSMutableArray *listItemArray = @[].mutableCopy;
        for (NSDictionary *info in dataArray) {
            ListItem *listItem = [[ListItem alloc] init];
            [listItem configWithDictionary:info];
            [listItemArray addObject:listItem];
        }
        //block is useful
        dispatch_async(dispatch_get_main_queue(), ^{
            if (finishBlock) {
                finishBlock(error == nil, listItemArray.copy);
            }
        });
    }];
    [dataTask resume];
```
>[NSURLSession优缺点和详解](https://www.jianshu.com/p/b0ddadd34037)

## 

## 参考文献
[参考文献-1](https://www.jianshu.com/p/d94cb6b58573)
[官网](https://github.com/AFNetworking/AFNetworking)
[请求详解-1](http://www.skyfox.org/ios-request-unacceptable-content-type.html)