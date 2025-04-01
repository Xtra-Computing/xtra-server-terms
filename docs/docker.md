# How to install rootless docker

Please run the following commands:

1. `curl -sSL https://get.docker.com/rootless | sh`
2. open the `.bashrc` file or `.zshrc` file. (depends on your shell)
3. Add the following two environmental variables to the end of the file.

```shell
export PATH=/home/$USER/bin:$PATH
export DOCKER_HOST=unix:///run/user/$UID/docker.sock
```

4. Start rootless docker

```bash
systemctl --$USER start docker
```


# How to uninstall rootless docker

```shell
rm -rf ~/.local/share/docker
cd ~/bin
rm -f *docker* *containerd* *runc* *rootlesskit* *vpnkit*
```