# Rootless Docker



## 📲 Install rootless docker

Please run the following commands:

1. `curl -sSL https://get.docker.com/rootless | sh`
2. Follow the instructions showed after the command

    a. open the `.bashrc` file or `.zshrc` file. (depends on your shell)

    b. Add the following two environmental variables to the end of the `.*rc` file.

    ```shell
    export PATH=/home/$USER/bin:$PATH
    export DOCKER_HOST=unix:///run/user/$UID/docker.sock
    ```

    c. source the rc file

    ```shell
    source ~/.bashrc
    # or
    # source ~/.zshrc
    ```

4. Start rootless docker

    ```bash
    systemctl --user start docker
    ```

5. Test if docker was installed

    ```bash
    docker run hello-world
    ```
    
6. Install docker compose plugin (optional)

    ```bash
    DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
    mkdir -p $DOCKER_CONFIG/cli-plugins
    curl -SL https://github.com/docker/compose/releases/download/v2.34.0/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
    ```
    
    ```bash
    chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
    ```
    
    ```bash
    docker compose version
    ```

## 🗑️ Uninstall rootless docker

```shell
rm -rf ~/.local/share/docker
cd ~/bin
rm -f *docker* *containerd* *runc* *rootlesskit* *vpnkit*
```
