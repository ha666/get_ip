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

##### 添加配置文件 get-ip.yaml
```yaml
domain_name: ha666.com
sub_domain_name: pi
aliyun_dns_key: abc
aliyun_dns_secret: def
is_debug: true
record_id: ""
```
##### 运行程序
```shell

# 在systemd中配置成服务，自动监控，如果程序挂了就自动拉起来

[Unit]
Description=get-ip
After=network.target

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=root
Group=root
Restart=always

# Prevent writes to /usr, /boot, and /etc
ProtectSystem=full

# Doesn't yet work properly with SELinux enabled
# NoNewPrivileges=true

PrivateDevices=true

WorkingDirectory=/home/pi/proj/get-ip
ExecStart=/home/pi/proj/get-ip/get-ip

KillMode=process
KillSignal=SIGTERM

# Don't want to see an automated SIGKILL ever
SendSIGKILL=yes

RestartSec=20s
UMask=007
```
```shell
sudo systemctl enable get-ip.service
sudo systemctl daemon-reload
sudo systemctl start get-ip.service
```
