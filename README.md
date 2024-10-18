# docker-redis-stack
Full Redis Stack Using Docker Compose

Update the redis-stack.conf file esepcially look at creting a very strong passowrd.

Maps the Redis port (6379) and RedisInsight port (8001) to the host  

## To Start
- docker compose up -d

## To Update
- docker compose pull
- docker compose down
- docker compose up -d

## Set Up Security
There are 2 options for setting up redis security using a global password or setting up an acl file that has the default user, admin and restricted user  
Open the redis-stack.conf file and find these lines:  
```
# Security
# requirepass set-complex-passwword
protected-mode no
aclfile /redis-acl.conf
```

To set up basic security comment out aaclfile line and un comment requirepassword. Please use a very complex password I normally use 16-32 alpha numeric passwords  

If you want to use ACL edit the redis.acl.conf file by changing the passwords after the > in each line  
```
user default on +@all ~* >defaultpassword
user admin on +@all ~* &* >adminpassword123
user restricted on +@all -@dangerous ~* &* >restrictedpassword456
```

You should also disable commands in production by uncommenting out some of these lines in redis-stack.conf    
```
# Disable dangerous commands
#rename-command FLUSHALL ""
#rename-command FLUSHDB ""
#rename-command CONFIG ""
#rename-command SHUTDOWN ""
#rename-command KEYS ""
#rename-command DEBUG ""
#rename-command SAVE ""
#rename-command BGSAVE ""
#rename-command BGREWRITEAOF ""
#rename-command MIGRATE ""
#rename-command RESTORE ""
#rename-command SORT ""
#rename-command MONITOR ""
#rename-command SYNC ""
#rename-command PSYNC ""
```

## Redis Cli
We have added redis-cli as part of the install but it has been compented out, either install redis-tools on server or you can install it via docker composer, if installed via composer please use  

```
docker compose run --rm redis-cli

redis-cli -h 127.0.0.1 -p 6379
```
