## Python Custom Build for Virtual Environments on Debian

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