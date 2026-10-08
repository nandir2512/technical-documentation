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
