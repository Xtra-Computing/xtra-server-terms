# Rootless Docker



## How to install rootless docker

Please run the following commands:

1. `curl -sSL https://get.docker.com/rootless | sh`
2. Follow the instructions showed after the command

    a. open the `.bashrc` file or `.zshrc` file. (depends on your shell)

    b. Add the following two environmental variables to the end of the `.*rc` file.

    ```shell
    export PATH=/home/$USER/bin:$PATH
    export DOCKER_HOST=unix:///run/user/$UID/docker.sock
    ```

3. Start rootless docker

    ```bash
    systemctl --$USER start docker
    ```

4. Test if docker was installed

    ```bash
    docker run hello-world
    ```

## How to uninstall rootless docker

```shell
rm -rf ~/.local/share/docker
cd ~/bin
rm -f *docker* *containerd* *runc* *rootlesskit* *vpnkit*
```