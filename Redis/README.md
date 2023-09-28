# Redis - Installation & Configuration on Ubuntu (Linux)

* at first you nedd to update your apt remote repository, and then install redis-server app using apt repository

  * (refer to below command)
  
```shell
apt update
apt install redis-server
```

and I thnk you should know where the redis configuration file is! there we go

ubuntu  **`/etc/redis/redis.config`** 

CentOS **`/etc/redis.config`**

<br><br/>

once you install redis-server, you'll recognize that ##'redis.config' is belong to redis user. as like below

```shell
2369114 -rw-r-----   1 redis redis 85844  3월  4  2022 redis.conf
```

so, yo have to switch user from your ID to root or redis. but at the first time you try to change user to redis, you will face this problem that user can't be changed. 

this is because **'redis:x:132:139::/var/lib/redis:/usr/sbin/nologin', shell (bash, zsh etc) is not configured to your redis ID. I recommend you switch user to root ID


after switching your user, you can access redis.config. and you can find part of content of configuration as below. 

```shell
#Redis access IP
bind [IP]

#Redis access Port
port [Port]

#redis access Password
requirepass [Password]

#log level 
loglevel notice

logfile /var/log/redis/redis-server.log
```

