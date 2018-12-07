ntp时间服务配置


## 主机信息

### master:
```
[hadoop@master ~]$ cat /etc/hosts
127.0.0.1       localhost.localdomain   localhost.localdomain   localhost4      localhost4.localdomain4 localhost
::1     localhost.localdomain   localhost.localdomain   localhost6      localhost6.localdomain6 localhost

192.168.163.177 master.hadoop master
192.168.163.178 slave3.hadoop slave3
```

### slave3:
```
[hadoop@slave3 .ssh]$ cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.163.177 master.hadoop master
192.168.163.178 slave3.hadoop slave3
```


## 配置
### 1. master安装ntp服务

```
chkconfig ntpd on
service ntpd restart
```
修改/etc/ntp.conf 添加
```
restrict 192.168.163.0 mask 255.255.255.0 nomodify notrap
```



#### 2. slave3 安装ntp服务
```
修改 /etc/ntp.conf 添加
server 192.168.163.177 prefer

注释
#server 0.centos.pool.ntp.org iburst
#server 1.centos.pool.ntp.org iburst
#server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst

chkconfig ntpd on
service ntpd restart

```

查看同步结果
ntpq -p