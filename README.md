# Docker-Swarm-Practice

## Task: Create a multi-service multi-node cat/dog voiting app 
## Diagram: ![architecture](https://user-images.githubusercontent.com/70330884/221368569-910c3f72-213b-4534-a722-d8973c99114f.png)

## Solution:
### 1.Create two overlay networks - "backand" and "frontend" 
```
docker network create -d overlay --attachable backend   
docker network create -d overlay --attachable frontend  
```
Check network using ``` docker network ls```

![Networks](https://user-images.githubusercontent.com/70330884/221368835-4f5a68ae-9e23-46b0-b5ef-6993bda91a2b.png)

### 2. Create "vote" service
```docker service create --name vote --network frontend -p 80:80 --replicas 2 bretfisher/examplevotingapp_vote```

### 3. Create "redis" service 
```docker service create --name redis --network frontend redis:3.2```

### 4. Create "worker" service
```docker service create --name worker --network frontend --network backend bretfisher/examplevotingapp_worker```

### 5. Create "db" service 
```docker service create --name db --mount type=volume,source=db-data,target=/var/lib/postgresql/data -e POSTGRES_HOST_AUTH_METHOD=trust --network backend postgres:9.4```

### 6. Create "network" service
```docker service create --name result -p 5001:80 --network backend bretfisher/examplevotingapp_result```

## Result
Voiting page - [https://localhost:80](https://localhost:80)
<br/><img width="426" alt="Voting app" src="https://user-images.githubusercontent.com/70330884/221369316-198e1664-0544-4beb-ada9-01ce222d36ff.png">

Result page - [https://localhost:5001](https://localhost:5001)
<br/><img width="897" alt="Result page src="https://user-images.githubusercontent.com/70330884/221369691-4eba3328-b1d0-48de-a61a-c8cab7ac6519.png">


