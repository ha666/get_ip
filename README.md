# get-ip

#### 介绍
自动获取内网IP并修改域名解析

#### 应用场景
公司里有一台测试机，没有固定IP，IP是有可能变化的，而我需要无感知地用ssh连接这台服务器

#### 解决方案
1. 自动启动，如果被关了，就自动拉起来
2. 定时（比如说每3分钟）监控一下本地IP地址有没有变化
3. 如果IP有变化，就调用dnspod的接口及时更新域名解析
4. ssh连接的时候直接用域名

#### 使用步骤

##### 准备工作
1. 在dnspod上面有一个域名
2. 创建好二级域名
3. <https://support.dnspod.cn/Kb/showarticle/tsid/227/> 按文档获取到id和token

##### 添加环境变量:主域名(例)
```shell
export domain_name=abc.red
```
##### 添加环境变量:二级域名(例)
```shell
export sub_domain_name=test
```
##### 添加环境变量:dnspod的访问编号(例)
```shell
export dnspod_id=12345
```
##### 添加环境变量:dnspod的访问令牌(例)
```shell
export dnspod_token=12345678901234567890123456789012
```
##### 添加环境变量:是否启用调试模式(可选,1启动,其它不启动)
```shell
export is_debug=1
```
##### 运行程序
```go
windows上直接双击启动
linux上面用nohup或其它方式启动
```
