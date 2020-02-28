A collection of dockerfiles for either development work or just for reference.

Example usage script:

#!/bin/bash
docker build . -t dotnet_dev_env:latest
docker system prune -f
docker run -it -v [local directory]:/home/docker/code dotnet_dev_env:latest
