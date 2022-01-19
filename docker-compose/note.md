# 1. Example 
- in file

# 2. Command

```bash
docker-compose build  # prepare
docker-compose images # list 
docker-compose run    
docker-compose up     # = build + run 
docker-compose ps 
docker-compose down   #
docker-compose start  #  fail if there is no image available. 
```

# 3. Mutilple Dockerfile 

```
version: '3'
services:
  web:
    # Path to dockerfile.
    # '.' represents the current directory in which
    # docker-compose.yml is present.
    build: 
        context: ./db
        dockerfile: Dockerfile-db

```

# 4. Env

```
env_file:
      - ./.env
```

or

```
- environment:
```

- Priority of .env variables: từ cao nhất tới thấp nhất : 
> - Compose file
> - Shell environment variables
> - Environment file
> - Dockerfile
> - Variable is not defined
