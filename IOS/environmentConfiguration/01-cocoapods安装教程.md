# cocoapod 安装使用教程
1. 安装Homebrew
```
1. /usr/bin/ruby -e "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install)"
2. brew -v 查看是否安装成功
```

2. 安装rvm
```
1. curl -L get.rvm.io | bash -s stable
2. 重启终端
3. 产看rvm版本
4. rvm -v 
```

3. 安装ruby
```
0. echo "ruby_url=https://cache.ruby-china.com/pub/ruby" > ~/.rvm/user/db 安装前更换rvm源
0. rvm install 2.7.0 --disable-binary # 忽略二进制，否则可能会请求 rubies.travis-ci.org
1. rvm install ruby --head 安装ruby最新版本或者指定版本
2. rvm list 查询已经安装ruby版本
3. rvm use 2.6.3 --default 指定安装的版本的ruby
4. ruby -v gem -v 安装版本检测
5. gem sources -l 查看gem源
6. gem source -r https://rubygems.org/
   gem source -a https://gems.ruby-china.org
   更换gem源
7. sudo gem update --system 先升级gem到最新版本防止出错
8. sudo gem install -n /usr/local/bin cocoapods 正式安装cocoapods
9. pod --version 查看pod版本
10. pod search AsyncSocket 验证搜索功能
11. git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git  ~/.cocoapods/repos/trunk 想要search需要继续下载这一步。
```

# cocoapods使用教程

1. pod init  生成Podfile文件
2. pod ‘AFNetworking’ 引入对应的类库，不写版本默认拉去最新的版本
3. pod install 拉去文件中的第三方库
















