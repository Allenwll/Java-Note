mysql 定时备份



## 以mysql用户执行

1. 以mysql用户设置定时本分脚本schedulerbackup.sh
```
vi schedulerbackup.sh

rq=`date +"%m%d"`  

mysqldump -u用户名 -p密码 -h主机 数据库  --lock-all-tables > /var/mysqlbackup/$rq.sql
```

2. 以root用户创建备份目录/var/mysqlbackup
```
cd /var
mkdir mysqlbackup
chown -R mysql:mysql mysqlbackup
```
3. 以mysql用户创建定时执行
```
crontab -e
50 23 * * * sh /home/mysql/schedulerbackup.sh
```

查看定时执行


    crontab -l
    
    
## 以root用户执行

1. 以root用户设置定时本分脚本schedulerbackup.sh
```
vi schedulerbackup.sh

rq=`date +"%m%d"`  

mysqldump -u用户名 -p密码 -h主机 数据库  --lock-all-tables > /var/mysqlbackup/$rq.sql
```

2. 以root用户创建备份目录/var/mysqlbackup
```
cd /var
mkdir mysqlbackup
```
3. 以root用户创建定时执行
```
crontab -e
50 23 * * * sh /root/schedulerbackup.sh
```

查看定时执行


    crontab -l