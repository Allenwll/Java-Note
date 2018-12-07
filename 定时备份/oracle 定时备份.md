oracle 定时备份

1. 以oracle用户设置定时本分脚本schedulerbackup.sh
```
vi schedulerbackup.sh

export ORACLE_SID=实例名
export ORACLE_BASE=/opt/app/oracle
export ORACLE_HOME=/opt/app/oracle/product/11g
export PATH=$ORACLE_HOME/bin:$PATH
export NLS_LANG="AMERICAN_AMERICA.AL32UTF8"

rq=`date +"%m%d"`  
exp 用户/密码 owner=用户 file=/var/orabackup/$rq.dmp log=/var/orabackup/$rq.log
```

2. 以root用户创建备份目录/var/orabackup
```
cd /var
mkdir orabackup
chown -R oracle:oinstall orabackup
```
3. 以oracle用户创建定时执行
```
crontab -e
50 23 * * * sh /home/oracle/schedulerbackup.sh
```

查看定时执行


    crontab -l