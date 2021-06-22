
# docker pull
- pulls an image from docker hub (hub.docker.com is default)
- docker pull <image-name>

# docker run
- run an image as a container
- steps behind
  1. checks local image cache, or downloads from docker hub and stores in local cache
  2. creates a container object and loads it into memory to run