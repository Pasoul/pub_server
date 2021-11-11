
### 客户端使用

## 注意: 如果你平时会上传package到google三方库上,直接从step 2开始看,相信你肯定知道怎么设置terminal走代理

```
打开命令行输入:

dart --version
```

查看你当前版本,并去 https://github.com/jiang111/pub/tree/master/version 这个地址找对应的pub.dart.snapshot文件
下载下来,如果没有,可以提issue,最低支持2.14.0

> 1. 将pub.dart.snapshot 拷贝到 flutter sdk目录/bin/cache/dart-sdk/bin/snapshots 文件夹下,替换源文件


```desc
为什么要替换, 因为默认情况下发布仓库需要走 google auth 授权, 需要科学上网, 替换的 snapshots 则把授权的逻辑给去掉 
```

> 2. 在pubspec.yaml文件添加: publish_to: http://私服ip:port, 然后发布仓库, (仓库名称最好不要和pub.dev上的重名)在项目根目录执行 pub publish 等待提示, 操作按y上传

> 3. 如何查看仓库是否发布成功, 查看部署的web地址,或者调用 http://ip:port/api/getAllPackages 如果能查询到你的仓库和对应的版本则代表发布成功

> 4. 上传成功之后如何依赖:

```
  package_name:
    hosted:
      name: plugin_name
      url: http://ip:port
    version: ^lastedVersion
```
> 5. 常用API:

```
http://ip:port/api/getAllPackages  //获取所有可用库的名称, 以及最新版本

http://ip:port/api/packages/<package-name> //获取 package_name 库所有历史版本, 以及相关信息

http://ip:port/api/packages/<package-name>/versions/<version-name> //获取 package_name 库 version-name 版本的下载地址, 以及相关信息

http://ip:port/packages/<package-name>/versions/<version-name>.tar.gz //下载指定库的指定版本
```

> 6. GUI页面部署参考https://github.com/jiang111/flutter_pub_web

### 服务端部署

> 0. 注意点: 务必保证服务器编码为utf8 编译前最好对系统依赖库执行一下update

> 1. 下载最新版dart的sdk, 并配置环境变量, 保证在任意目录可以执行dart, 添加大陆地区的镜像源(自行google) 
```shell script
1. https://dart.dev/tools/sdk/archive 选择对应版本
2. wget https://storage.googleapis.com/dart-archive/channels/stable/release/2.13.1/sdk/dartsdk-linux-x64-release.zip
3. unzip dartsdk-linux-x64-release.zip
4. export PATH=$PATH:./dart-sdk/bin
5. export PUB_HOSTED_URL=https://pub.flutter-io.cn

```

> 2. clone 源码, cd 到源码根目录, 执行 pub get

> 3. 修改 ./example/example.dart 中的
```dart
ArgParser argsParser() {
  var parser = ArgParser();

  //pub_server-repository-data修改成你需要的路径
  parser.addOption('directory',
      abbr: 'd', defaultsTo: 'pub_server-repository-data');

  //为保证外部访问,修改成服务器的ip
  parser.addOption('host', abbr: 'h', defaultsTo: 'localhost');

  //修改成自己想要的端口
  parser.addOption('port', abbr: 'p', defaultsTo: '6453');
  parser.addFlag('standalone', abbr: 's', defaultsTo: false);
  return parser;
}

```

> 4. terimal执行 ./pub_server_run.sh 即可完成部署

> 5. 运行后日志会保存在当前目录下的log.log文件,可自行查看

### web端部署
查看 https://github.com/jiang111/flutter_pub_web 导出静态文件,然后部署在web服务器即可, 具体操作参考flutter web相关构建方法


[![Stargazers over time](https://starchart.cc/jiang111/pub_server.svg)](https://starchart.cc/jiang111/pub_server)
