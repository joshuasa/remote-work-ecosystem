## Debian DevBox

* [Secure DevBox](https://github.com/joshuasa/remote-work-ecosystem/blob/main/content/debian-devbox.md#secure-devbox)
* [Git](https://github.com/joshuasa/remote-work-ecosystem/blob/main/content/debian-devbox.md#git)
* [Python Custom Builds for Virtual Environments](https://github.com/joshuasa/remote-work-ecosystem/blob/main/content/debian-devbox.md#python-custom-builds-for-virtual-environments)
* [HTTPie](https://github.com/joshuasa/remote-work-ecosystem/blob/main/content/debian-devbox.md#httpie)
* [Azure CLI](https://github.com/joshuasa/remote-work-ecosystem/blob/main/content/debian-devbox.md#azure-cli)
* [Azure Functions Core Tools](https://github.com/joshuasa/remote-work-ecosystem/blob/main/content/debian-devbox.md#azure-functions-core-tools)

## Secure DevBox

Follow steps below when installing Debian locally on a bare-metal NUC for example. When installing Debian on Azure VM setup can be done via portal config and only SSH port needs to be updated in `sshd_config`.

**Note:** On Azure consider using [Bastion](https://azure.microsoft.com/en-us/services/azure-bastion/).

### Add User

If only `root` account provided after clean installation.

```bash
$ apt install sudo
$ adduser joshua
```

Add user to sudoers.

```bash
$ vim /etc/sudoers
```
```bash
# User privilege specification
root    ALL=(ALL:ALL) ALL
joshua  ALL=(ALL:ALL) NOPASSWD:ALL
```

Restart SSH server.

```bash
$ /etc/init.d/ssh restart
```

###  Add User SSH key to DevBox

Add public key to `.ssh/authorized_keys` and set folder and file permissions.

```bash
$ chmod 700 .ssh
$ chmod 600 .ssh/authorized_keys
```

### Update SSH Server settings

Open `sshd_config` file.

```bash
$ sudo vim /etc/ssh/sshd_config
```

Update the following:

```bash
Port xxxx
PasswordAuthentication no
PermitRootLogin no
```

Restart SSH server.

```bash
$ sudo /etc/init.d/ssh restart
```

Now access host using ssh key and newly assigned port.<br>
**Note:** Remember to allow newly assigned SSH port traffic on firewall.

```bash
$ ssh -p xxxx joshua@dbox-azu-za1 -i ssh/joshua
```

## Git

### Install Git

```bash
$ sudo apt install git
```

### Config Git

```bash
$ git config --global user.name joshuasa
$ git config --global user.email joshua.sa@yman.me

$ git config --list
$ cat ~/.gitconfig

$ git config --global credential.helper 'store'
```

## Python Custom Builds for Virtual Environments

Install multiple versions of Python in `~/.pythonbuilds`<br>
Use these custom builds when creating virtual environments requiring specific Python version.

### Install build dependencies

```bash
$ sudo apt install build-essential
$ sudo apt install libreadline-gplv2-dev libncursesw5-dev libssl-dev
$ sudo apt install libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
$ sudo apt install libffi-dev
```

### Download Python source code and build

python.org source code [download page](https://www.python.org/downloads/)

```bash
$ mkdir ~/.pythonbuilds/python-3.9.1

$ cd ~/downloads
$ wget https://www.python.org/ftp/python/3.9.1/Python-3.9.1.tgz

$ tar -zxvf Python-3.9.1.tgz
$ cd Python-3.9.1

$ ./configure --prefix=/home/joshua/.pythonbuilds/python-3.9.1
$ make
$ make install
```

### Install pip and virtualenv

```bash
$ sudo apt install python3-pip
$ pip3 --version

$ sudo pip3 install --upgrade virtualenv
$ virtualenv --version
```

### Install and activate Python virtual environment

Go to Python project folder.

```bash
$ virtualenv --python=/home/joshua/.pythonbuilds/python-3.9.1/bin/python3.9 venv
$ source venv/bin/activate
(venv) $ python --version
Python 3.9.1
```

## HTTPie

### Create Python Virtual Environment

```bash
$ virtualenv --python=/home/joshua/.pythonbuilds/python-3.8.6/bin/python3.8 httpie-venv
```

### Install HTTPie

```bash
$ source httpie-venv/bin/activate
$ python --version

$ pip install --upgrade pip setuptools
$ pip install --upgrade httpie
$ http --version
```

## Azure CLI

### Install Azure CLI

Get packages needed for the install process.

```bash
$ sudo apt update
$ sudo apt install ca-certificates curl apt-transport-https lsb-release gnupg
```

Download and install the Microsoft signing key.

```bash
$ curl -sL https://packages.microsoft.com/keys/microsoft.asc |
    gpg --dearmor |
    sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
```

Add the Azure CLI software repository.

```bash
$ AZ_REPO=$(lsb_release -cs)
$ echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
    sudo tee /etc/apt/sources.list.d/azure-cli.list
```

Update repository information and install the `azure-cli` package.

```bash
$ sudo apt update
$ sudo apt install azure-cli

$ az --version
$ az version
```

![az version](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/debian-devbox_02.png)

### Login

Run the `login` command.

```bash
$ az login
```

Open browser page https://aka.ms/devicelogin and enter the authorization code displayed in terminal.

### Update

Update Azure CLI with `apt upgrade` or `az upgrade`.

## Azure Functions Core Tools

**Note:** Microsoft signing key already downloaded and installed with Azure CLI installation.

Set up the APT source list before doing an APT update.

```bash
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/debian/$(lsb_release -rs | cut -d'.' -f 1)/prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
```

Install the Core Tools package.

```bash
$ sudo apt update
$ sudo apt install azure-functions-core-tools-3

$ func
```

![func](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/debian-devbox_01.png)