# docker-redis-8
Redis 8 Using Docker Compose

Update the redis.conf file especially look at creating a very strong password.

Maps the Redis port (6379) to the host  

## To Start
- docker compose up -d

## To Update
- docker compose pull
- docker compose down
- docker compose up -d

## Set Up Security
There are 2 options for setting up Redis security using a global password or setting up an ACL file with multiple users.

Open the redis.conf file and find these lines:  
```
# Security
# requirepass YOUR_SECURE_PASSWORD_HERE_32_CHARS_MIN
protected-mode yes
aclfile /usr/local/etc/redis/redis-acl.conf
```

### Option 1: Basic Password Authentication
Comment out the `aclfile` line and uncomment `requirepass`. Use a very complex password (32+ characters recommended):
```
requirepass your_very_secure_password_here_at_least_32_characters
# aclfile /usr/local/etc/redis/redis-acl.conf
```

### Option 2: ACL-based Authentication (Recommended)
Edit the redis-acl.conf file and change all the placeholder passwords:
```
user default off
user admin on +@all ~* &* >CHANGE_THIS_ADMIN_PASSWORD_32_CHARS_MIN
user app on +@read +@write +@list +@set +@hash +@stream +@geo +@hyperloglog +@bitmap -@dangerous ~* >CHANGE_THIS_APP_PASSWORD_32_CHARS_MIN
user readonly on +@read ~* >CHANGE_THIS_READONLY_PASSWORD_32_CHARS_MIN
```

### Production Security Hardening
You should also disable dangerous commands in production by uncommenting some of these lines in redis.conf    
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
