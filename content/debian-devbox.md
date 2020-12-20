## Debian DevBox

Assuming clean Debian 10 (Buster) installation.

## Secure DevBox

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