# Devboxes

This repository contains ansible roles to setup my development machines and helps to keep them in sync.
The ansible playbook will install software which i need for my day to day work as developer.
It will also install my set of configuration for those tools.
The playbook will install and configure the following tools:

* zsh (configured with [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh))
* modularized zshrc config via a dotfiles repo (e.g. [schnatterer/dotfiles](https://github.com/schnatterer/dotfiles/), inspired by [holman](https://github.com/holman/dotfiles))
* tmux (inspired from [.tmux](https://github.com/gpakosz/.tmux))
* vim (configured with [vimrc](https://github.com/amix/vimrc))
* docker
* vagrant
* virtual box
* packer
* terraform via [tfenv](https://github.com/tfutils/tfenv)
* gcloud, kubectl and kubectx
* gpg
* git
* mercurial
* java, groovy, maven, gradle via [sdkman](https://sdkman.io/)
* node.js via [nvm](https://github.com/nvm-sh/nvm)
* golang
* vscode
* Intellij
* bin, pipx and topgrade for installing and upgrading everything. 
  Hints:
    * Run `topgrade` to upgrade everything regularly. 
    * Use `pipx` to install python-base apps. Other than `pip`, apps installed using `pipx` are also upgraded using topgrade.
* and many more

## Supported Operating Systems

* Ubuntu (tested on 20.04)
* Manjaro Linux
* Mac OS X

## Prerequisites

* Ansible >= 2.6

### MacOSX

* Install [homebrew](https://brew.sh/)
* Install ansible `brew install ansible`

### Ubuntu

Install ansible

```bash
sudo add-apt-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

### Manjaro

Install ansible `sudo pacman -S ansible`

## Getting started

Just run the `devbox` script, on the first run it will ask some question to personalize the installed configurations.
After that it will ask for your sudo password and then apply the ansible playbook. 

You can also test the whole bunch in an ephemeral docker container.

```shell
docker build -t devboxes .
docker run -v $(pwd):/home/dev/devboxes \
    -it \
    --tmpfs /run \
    --tmpfs /run/lock \
    --privileged \
    -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
    -v /lib/modules:/lib/modules:ro \
   --name devbox \
    ghcr.io/schnatterer/devboxes

#login with: dev / box
devboxes/devbox -e ansible_become_pass='box' 

# Once ansible is done, explore the container as you like.
# After you're done, you'll have to kill it from another terminal:
docker stop devbox

# If you need the container again later just
docker start devbox
#login with: dev / box
docker attach devbox

# If you want to get rid of it for good 
docker rm -f devbox
```

Please be aware that the `devbox` scripts loads several GB worth of data from the internet and that as a result the 
container will have take several GB on your harddrive (7GB at the time of writing).

Note that it's not a sandboxed container, because devboxes relies on snap, which relies on systemd, which both are not
made for containers.
In general, less privileges than `--privileged` flag would suffice (see [snpad-docker](https://github.com/ogra1/snapd-docker/blob/3a38d17a30d8295f6099b4e5769f54763e92ad4a/build.sh#L110-L113)).
But, devboxes installs and runs docker (in this case in docker), which is what `--privileged` was made for, so its an obvious choice here.
Use it only for testing and development!

## Tags

Its also possible to only apply certain tags, e.g.

```shell
# exclusive tag
devbox --tags zsh
# excluded tag
devbox --skip-tags zsh
```

See [playbook](playbook.yml) for existing tags.

## Todo

- [ ] Automatically Git Updates
- [ ] Role chooser
