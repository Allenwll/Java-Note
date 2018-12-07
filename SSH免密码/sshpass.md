sshpass

# 从命令行方式传递密码 -p指定密码

$ sshpass -p '123456' ssh user_name@host_ip
$ sshpass -p '123456' scp root@host_ip:/home/test/t ./tmp/