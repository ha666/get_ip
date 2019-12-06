# get-ip

#### 介绍
自动获取内网IP并修改域名解析

#### 应用场景
公司里有一台测试机，没有固定IP，IP是有可能变化的，而我需要无感知地用ssh连接这台服务器

#### 解决方案
1. 自动启动，如果被关了，就自动拉起来
2. 定时（比如说每1分钟）监控一下本地IP地址有没有变化
3. 如果IP有变化，就调用aliyun dns的接口及时更新域名解析
4. ssh连接的时候直接用域名

#### 使用步骤

##### 准备工作
1. 在aliyun上面有一个域名
2. 创建好二级域名
3. <https://usercenter.console.aliyun.com/#/manage/ak> 获取到AccessKey和Access Key Secret

##### 添加环境变量:主域名(例)
```shell
export domain_name=abc.red
```
##### 添加环境变量:二级域名(例)
```shell
export sub_domain_name=test
```
##### 添加环境变量:aliyn dns的AccessKeyId(例)
```shell
export aliyun_dns_key=7324hjksd8y9f3jk
```
##### 添加环境变量:aliyn dns的AccessKeySecret(例)
```shell
export aliyun_dns_secret=u8a9q2j3nklfcasu82hi3ry8as9d2h3
```
##### 运行程序
```shell
在Supervisor中配置成服务，自动监控，如果程序挂了就自动拉起来

[program:get_ip]
command=/root/get_ip/get_ip
priority=999
autostart=true
autorestart=true
startsecs=10
startretries=3
exitcodes=0,2
stopsignal=QUIT
stopwaitsecs=10
user=root
log_stdout=true
log_stderr=true
logfile=/root/get_ip/get_ip.log
logfile_maxbytes=1MB
logfile_backups=10
stdout_logfile_maxbytes=20MB
stdout_logfile_backups=20
stdout_logfile=/root/get_ip/get_ip.stdout.log

supervisorctl reload

```
