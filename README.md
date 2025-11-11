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

## Redis CLI Usage

### Option 1: Use Redis CLI from the running container
Connect to the Redis CLI using the running Redis container:
```bash
docker exec -it redis redis-cli
```

### Option 2: Use Redis CLI with authentication
If you have authentication enabled, connect with credentials:
```bash
# For basic password authentication
docker exec -it redis redis-cli -a your_password

# For ACL authentication
docker exec -it redis redis-cli --user admin --pass your_admin_password
```

### Option 3: Install Redis CLI separately
You can also install redis-tools on your host system:
```bash
# Ubuntu/Debian
sudo apt-get install redis-tools

# macOS with Homebrew
brew install redis

# Then connect from host
redis-cli -h 127.0.0.1 -p 6379
```

### Basic Redis CLI Commands
Once connected, try these basic commands:
```bash
# Test connection
PING

# Set and get a key
SET mykey "Hello Redis"
GET mykey

# List all keys (use carefully in production)
KEYS *

# Get server info
INFO server
```
