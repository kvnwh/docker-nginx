# Mount Volumes on Windows Subsystem for Linux (WSL)
There is a [blog post](https://medium.com/software-development-stories/developing-a-dockerized-web-app-on-windows-subsystem-for-linux-wsl-61efec965080) about how to set it up, but not quite working for me. Following is the guide that works on my windows machine.

## Install docker (If you already have docker set up in windows and bash, skip to step 4 )

1. install [docker for windows](https://docs.docker.com/docker-for-windows/).
2. install docker client on ubuntu
    ```sh
    sudo apt-get install apt-transport-https ca-certificates
    sudo apt-key adv \
        --keyserver hkp://ha.pool.sks-keyservers.net:80 \
        --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee /etc/apt/sources.list.d/docker.list
    sudo apt-get update
    sudo apt-get install docker-engine
    ```
3. add env variable to `.bashrc` file `export DOCKER_HOST=tcp://127.0.0.1:2375`. Check if docker is running by `docker info`
4. turn on shared Drives on docker settings. 
    
    **NOTE: If it is already on, click on Reset credentials to reset it (I couldn't get the mounted volume work until I reset)**
    
    ![image](https://cdn-images-1.medium.com/max/1600/1*nlJtgI5TxsNPqntOVFEAiw.jpeg)
5. Make sure your projects are in your `user` directory, i.e. `C:\Users\your-name\*`, the mount will not work if it is outside `C:\Users`
6. Open up bash terminal.
6. symlink `/mnt/c` as `/c` 
    
    `sudo ln -s /mnt/c /c`
7. go into the project directory by using `/c` (not `/mnt/c`), i.e. `cd /c/Users/your-name/dev/platform`. Recommended to add an alias in .bashrc file: 

    `alias dev="cd /c/Users/your-name/dev"`
6. now you can use `$PWD` in docker-compose file, so it will look like:
    ```
    web:
    volumes:
      - $PWD:/app
    ```