# docker-redis-stack
Full Redis Stack Using Docker Compose

Update the redis-stack.conf file esepcially look at creting a very strong passowrd.

## To Start
- docker compose up -d

## To Update
- docker compose pull
- docker compose down
- docker compose up -d

## Redis Cli
We have added redis-cli as part of the install but it has been compented out, either install redis-tools on server or you can install it via composer, if installed via composer please use  

```
docker compose run --rm redis-cli
```

## To Connect using passowrd
Please change the password in the conf file this is best practice   
```
redis-cli -h 127.0.0.1 -p 6379 -a 'your_strong_password' 
```