# HELPFUL DOCKER COMMANDS

- NOTE: We're going to name containers using your username, because using names is easier than using ID's, but we need to avoid naming conflicts.

- OTHER NOTE: We assume your environment already contains the image ```sg2```, and also that the specified image has a tag at ```1.0```.


<hr>
<br>

TO START A LONG-RUNNING PROCESS INSIDE A CONTAINER SO THAT YOU CAN ENTER OR EXIT THE CONTAINER AT WILL, WHILE KEEPING THE CONTAINER (AND ITS PROCESS) RUNNING:
``` bash

docker run --detach --tty --name $USER --user $(id -u):$(id -g) sg2:1.0 {LONG}/{RUNNING}/{PROCESS}

# EX: docker run --detach --tty --name $USER --user $(id -u):$(id -g) sg2:1.0 python3 /src/some_script.py

# EX: docker run --detach --tty --name $USER --user $(id -u):$(id -g) sg2:1.0 bash -c 'echo "Process started." && echo "Sleeping..." && sleep 300'

```

<br>

TO VIEW THE OUTPUT OF A RUNNING BUT DETACHED CONTAINER'S MAIN PROCESS:
(The main process is the entrypoint specified when you started the container)
``` bash
docker logs --follow $USER

# To exit and return to the host shell, press CTRL+C
```

<br>

TO "ATTACH" TO A DETACHED BUT RUNNING CONTAINER, AND ALSO START BASH IN YOUR TTY:
``` bash
docker exec --interactive --tty $USER /bin/bash

# To exit and return to the host shell, type 'exit' and press enter, or press CTRL+C
```

<br>

TO STOP A RUNNING CONTAINER:
``` bash
docker stop $USER
```

<br>

TO RESTART A STOPPED CONTAINER:
``` bash
docker restart $USER
```

<br>

TO PAUSE A RUNNING CONTAINER (a paused container will not survive a reboot):
``` bash
docker pause $USER

# To un-pause a paused container
# docker unpause $USER
```

<br>

STARTING A CONTAINER WITH READONLY ACCESS TO A DATASET (and access to host GPUs):
``` bash
cd {DIR}/{WITH}/{CODE}

# Note that /{PATH/{TO}/{DATA} will be created if it doesn't exist. 
docker run --detach --tty --name $USER --user $(id -u):$(id -g) --gpus all -v $(pwd):/src -v /{PATH/{TO}/{DATA}:/data:ro sg2:1.
```

<br>

TO SAVE DOCKER IMAGE TO TAR FILE:
``` bash
# See the docs: https://docs.docker.com/engine/reference/commandline/save/#description
```

<br>

TO REMOVE ALL STOPPED CONTAINERS:
``` bash
# Note that this will not free significant disk space, 
# as the image itself is the primary contributor to disk usage.
docker container prune
```

<br>

TO REMOVE ALL UNUSED IMAGES:
``` bash
docker image prune
```
