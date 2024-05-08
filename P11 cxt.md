# Crear db mysql
```
create user 'mmuser'@'%' identified by 'mmuser-password';
create databasse mattermost;
grant all privileges on mattermost.* to 'mmuser'@'%';
wget https://releases.mattermost.com/9.7.3/mattermost-9.7.3-linux-amd64.tar.gz
tar -xvzf mattermost...gz
mv mattermost /opt
 "DataSource": "mmuser:mmuser-password@tcp(127.0.0.1:3306)/mattermost?charset=utf8mb4,utf8\u0026writeTimeout=30s",

```
usuario y contra
admin
cxt-g06-admin@maildrop.cc
administrator
nombre empresa : cxt_g06