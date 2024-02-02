# Lab 1 - Docker


## Pre-reqs

- Docker should be preinstalled:
   ```
   > docker info
   Client:
    Version:    24.0.1
    Context:    rootless
    Debug Mode: false
   
   Server:
    Containers: 15
     Running: 3
     Paused: 0
     Stopped: 12
    Images: 496
    Server Version: 24.0.1
   ...
   ```

- We will be using the Docker image `josecastillolema/api` hosted on [Docker Hub](https://hub.docker.com/r/josecastillolema/api) during this class.

  The image `josecastillolema/api` cointains the [following files](lab01-docker):
   - [**`api.py`**](lab01-docker/api.py): A simple API written in Python that leverages the [Flask web server library](https://flask.palletsprojects.com/en/1.1.x/)
   - [**`requirements.txt`**](lab01-docker/requirements.txt): Application dependencies. They can be installed using `pip`, Python's dependency manager
   - [**`Dockerfile`**](lab01-docker/Dockerfile): ​The file used by `docker` to create the image
 
## Docker images

2. List the images in the local repository (the catalog should be empty, as we haven't yet downloaded any images):
    ```
    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    ```
    
3. Search images within the [DockerHub](https://hub.docker.com/) catalog:
    ```
    $ docker search mongodb
    NAME                                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
    mongo                               MongoDB document databases provide high avai…   7041                [OK]                
    mongo-express                       Web-based MongoDB admin interface, written w…   735                 [OK]                
    tutum/mongodb                       MongoDB Docker image – listens in port 27017…   229                                     [OK]
    bitnami/mongodb                     Bitnami MongoDB Docker Image                    123                                     [OK]
    frodenas/mongodb                    A Docker Image for MongoDB                      18                                      [OK]
    centos/mongodb-32-centos7           MongoDB NoSQL database server                   8                                       
    webhippie/mongodb                   Docker images for MongoDB                       7                                       [OK]
    centos/mongodb-26-centos7           MongoDB NoSQL database server                   5                                       
    centos/mongodb-36-centos7           MongoDB NoSQL database server                   5                                       
    eses/mongodb_exporter               mongodb exporter for prometheus                 4                                       [OK]
    neowaylabs/mongodb-mms-agent        This Docker image with MongoDB Monitoring Ag…   4                                       [OK]
    centos/mongodb-34-centos7           MongoDB NoSQL database server                   3                                       
    quadstingray/mongodb                MongoDB with Memory and User Settings           3                                       [OK]
    tozd/mongodb                        Base image for MongoDB server.                  2                                       [OK]
    mongodbsap/mongodbdocker                                                            2                                       
    zadki3l/mongodb-oplog               Simple mongodb image with single-node replic…   2                                       [OK]
    ssalaues/mongodb-exporter           MongoDB Replicaset Prometheus Compatible Met…   2                                       
    xogroup/mongodb_backup_gdrive       Docker image to create a MongoDB database ba…   1                                       [OK]
    bitnami/mongodb-exporter                                                            1                                       
    openshift/mongodb-24-centos7        DEPRECATED: A Centos7 based MongoDB v2.4 ima…   1                                       
    ansibleplaybookbundle/mongodb-apb   An APB to deploy MongoDB.                       1                                       [OK]
    targetprocess/mongodb_exporter      MongoDB exporter for prometheus                 0                                       [OK]
    gebele/mongodb                      mongodb                                         0                                       [OK]
    phenompeople/mongodb                 MongoDB is an open-source, document databas…   0                                       [OK]
    astronomerio/mongodb-source         Mongodb source.                                 0                                       [OK]
    ```
 
4. Download (`pull`) the image `josecastillolema/api` into the local repository:
    ```
    $ docker pull josecastillolema/api
    Using default tag: latest
    latest: Pulling from josecastillolema/api
    741437d97401: Pull complete 
    34d8874714d7: Pull complete 
    0a108aa26679: Pull complete 
    7f0334c36886: Pull complete 
    65c95cb8b3be: Pull complete 
    d6518cb8a076: Pull complete 
    978af0c41163: Pull complete 
    2279c641f679: Pull complete 
    0adc21e4cdc7: Pull complete 
    44e1126d38c8: Pull complete 
    24c75b074ef0: Pull complete 
    Digest: sha256:3da8f411cec4ffa960543c9260b193badde97397eb16f35bcac5e8e320bd5393
    Status: Downloaded newer image for josecastillolema/api:latest
    docker.io/josecastillolema/api:latest
    ```
    
5. List the images again, confirm that the image `josecastillolema/api` exists:
    ```
    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    josecastillolema/api   latest              ad8814253c1b        2 years ago         933MB
    ```

## Playing around with Docker

6. Run an example command (`hostname`) into the container:
    ```
    $ docker run josecastillolema/api hostname
    c293c1989a56
    ```

7. Measure the time of the previous command:
    ```
    $ time docker run josecastillolema/api hostname
    7aa02808ccfc

    real	0m0.812s
    user	0m0.023s
    sys	0m0.027s
    ```
    Note that in less than a second:
     - Docker created the container
     - Ran the `hostname` command on it
     - Printed the output
     - Deleted the container
    
8. Check that both host and container share the same Kernel:
    ```
    $ uname -a
    Linux ip-172-31-60-180 5.3.0-1023-aws #25~18.04.1-Ubuntu SMP Fri Jun 5 15:18:30 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

    $ docker run josecastillolema/api uname -a
    Linux de0407ee790f 5.3.0-1023-aws #25~18.04.1-Ubuntu SMP Fri Jun 5 15:18:30 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
    ```

9. Run the `josecastillolema/api` image in interactive mode. Note that the prompt changes when we log into the container: user `root` with hostname `d8924e5138b3` (the container ID in this particular run).
    ```
    $ docker run -it josecastillolema/api bash
    root@d8924e5138b3:/#
    ```

10. Create a file while still inside the container and then exit the container:
    ```
    root@d8924e5138b3:/# touch myFile
    
    root@d8924e5138b3:/# ls
    bin   dev  home  lib32  libx32  myFile      opt   root  sbin  sys  usr
    boot  etc  lib   lib64  media   mnt         proc  run   srv   tmp  var

    root@d8924e5138b3:/# exit
    ```
    
11. Check that the container is no longer running:
    ```
    $ docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```
    
12. Run the container again, and confirm that the created file no longer exists. **Containers are ephemeral**, they do not store any type of change, be it files, data, installed *software*, etc. Any changes to the container should be done through the [`Dockerfile`](lab01-docker/Dockerfile):
    ```
    $ docker run -it josecastillolema/api bash
    root@5b83d8b5b521:/# ls
    bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
    boot  etc  lib   lib64  media   opt  root  sbin  sys  usr

    root@5b83d8b5b521:/# ls myFile
    ls: meuArquivo: No such file or directory
    
    root@5b83d8b5b521:/# exit
    ```

## Container execution

13. Let's add the `-p` parameter to the `docker run` command, responsible for port mapping. It takes an argument of the type ***x:y***, where ***x*** is the host-side port and ***y*** the container-side port. The `-d` parameter configures `docker` to run the container in the *background*:
   ```
   $ docker run -d -p 5000:5000 josecastillolema/api
   ```

14. Check that the container is running (note the new `PORTS` information: `0.0.0.0:5000->5000/tcp`):
   ```
   $ docker ps
   CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS                            PORTS                    NAMES
   e691f0fd676b        josecastillolema/api   "./api.py"          9 seconds ago       Up 8 seconds (health: starting)   0.0.0.0:5000->5000/tcp   funny_roentgen
   ```
   
15. After 30 seconds of its creation, the container should transition to the `healthy` state (`STATUS` column):
   ```
   $ docker ps
   CONTAINER ID        IMAGE                  COMMAND             CREATED              STATUS                        PORTS                    NAMES
   e691f0fd676b        josecastillolema/api   "./api.py"          About a minute ago   Up About a minute (healthy)   0.0.0.0:5000->5000/tcp   funny_roentgen
   ```
   
16. Test locally the API (or via browser if you prefer):
   ```
   $ curl localhost:5000
   Benvido a API FIAP!
   ```
