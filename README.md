# Redis Sentinel

## Description 

 Repository contains code to run Redis Sentinel using Docker Compose.


## Usage

 1. Install Docker
 2. Install Docker Compose
 3. Clone repository code
 4. Run Docker compose
 5. Add hosts to your host file
    ```bash
    127.0.0.1 redis-1
    127.0.0.1 redis-2
    127.0.0.1 redis-3
    127.0.0.1 sentinel-1
    127.0.0.1 sentinel-2
    127.0.0.1 sentinel-3
    ```

 It will run 3 Redis and 3  Sentinels

 **Redis**
  ```
  redis-1:6379    # Master
  redis-2:7379    # Replica 1
  redis-3:8379    # Replica 2
  ```
 **Sentinel**
  ```
  sentinel-1:26379
  sentinel-2:36379
  sentinel-3:46379
  ```


## Check

 1. Ask Sentinel for the current Master
    ```bash
    redis-cli \
        -h sentinel-1 \
        -p 26379 \
        -a localpass \
        sentinel get-master-addr-by-name mymaster
    ```
    ```
    1) "redis-1"
    2) "6379"
    ```

 2. Contact Redis Master
    ```bash
    redis-cli \
        -h redis-1 \
        -p 6379 \
        -a localpass \
        info replication
    ```

 3. Ask Sentinel for Replicas
    ```bash
    redis-cli \
        -h sentinel-1 \
        -p 26379 \
        -a localpass \
        sentinel replicas mymaster
    ```
    ```
    1)  1) "name"
        2) "redis-3:8379"
        3) "ip"
        4) "redis-3"
        5) "port"
        6) "8379"
        7) "runid"
        8) "4b795a8e8a9e9495ab5195d234927a040328525d"
        9) "flags"
    10) "slave"
    ```


 4. Contact Replica
    ```bash
    redis-cli \
        -h redis-3 \
        -p 8379 \
        -a localpass \
        info replication
    ```
