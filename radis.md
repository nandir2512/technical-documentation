### 1. Download and Run the Redis in docker Container
``` docker run -d --name my-redis -p 6379:6379 redis:latest ```

What this command does:
• -d: Runs the container in detached mode (in the background).
• --name my-redis: Assigns the name my-redis to your container so it is easy to reference.
• -p 6379:6379: Maps port 6379 of the container to port 6379 on your Mac, allowing local applications to connect to it.
• redis:latest: Tells Docker to use the latest official Redis Open Source image.

### 2. Verify the Container is Running
``` docker ps ```

### 3. Connect to Redis via CLI 
``` docker exec -it my-redis redis-cli ```

Once the prompt changes to 127.0.0.1:6379>, test it by running a quick ping-pong command:
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set test "Hello Docker"
OK
127.0.0.1:6379> get test
"Hello Docker"
127.0.0.1:6379> exit



| Feature | redis:latest (Standard) | redis/redis-stack:latest (Stack) |
| -------- | -------- | -------- |
| Purpose  | Pure caching & basic data structures. | Complex modern application development. |
| Footprint  | Extremely lightweight, low memory usage.  | Heavier image due to preloaded extensions. |
| Core | Structures	Strings, Hashes, Lists, Sets, Streams. | All standard structures + extensions.
| Full-Text & Index Search | ❌ No (limited to simple key lookups). | Yes (RediSearch / Vector Search).


Why Redis Stack is Required for AI & RAG

1. **Native Vector Search (Vector Database): **
  To build a RAG system, you must convert text (like PDF documents or website data) into mathematical arrays called embeddings (vectors). Redis Stack includes the RediSearch engine, which allows it to act as a high-performance Vector Database. It supports algorithms like HNSW and FLAT to perform KNN (K-Nearest Neighbor) semantic search instantly.
2. **Context & Chat History Management: **
  AI agents require strict memory management. Redis Stack allows you to store entire user session histories as structured JSON using RedisJSON, while simultaneously indexing them for quick semantic retrieval.
3. **Hybrid Search Capabilities: **
  With Redis Stack, you can combine semantic vector searches with traditional full-text keyword filters (e.g., searching for "find documents about tax compliance and filter only for files created in 2026"). Standard Redis cannot do this.


### 4. 2. Run the Redis Stack Container
``` docker run -d --name my-redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest ```


### 5. Verify the Container is Running
``` docker ps ```

### 3. Connect to Redis via CLI 
```
    rahulnandi@Rahuls-MacBook-Air ~ %  docker exec -it <CONTAINER ID> bash 
    root@6a66365ca3a0:/# redis-cli ping
    PONG
    root@6a66365ca3a0:/# redis-cli     
    127.0.0.1:6379> 
```

### Getting Started with Redis Iris

https://redis.io/tutorials/getting-started-with-redis-iris/

