## VS Code Remote Development using SSH

Local-quality development experience with development environment & code hosted on remote bare-metal or VM based server. Examples are related to my remote work development ecosystem - VS Code running locally on Windows 10 PCs connecting to Debian DevBoxes (bare-metal servers and VMs in my Cape Town home office as well as data centers in Europe, United States and South Africa).

![Remote-SSH](https://code.visualstudio.com/assets/docs/remote/ssh/architecture-ssh.png)

## System Requirements

### Local SSH client

OpenSSH included in Windows 10:

```powershell
PS C:\> ssh -V
OpenSSH_for_Windows_7.7p1, LibreSSL 2.6.5
```

### Remote SSH host

Installing secure Debian DevBox gist *coming soon.*<br>
OpenSSH server on Debian:

```bash
$ ssh -V
OpenSSH_7.9p1 Debian-10+deb10u2, OpenSSL 1.1.1d  10 Sep 2019

$ sudo systemctl status sshd
ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since...
```

### Visual Studio Code & Remote SSH Extension

Search for Remote SSH extension by Microsoft in VS Code side bar and install.

## Connect to Remote Host and Open Remote Project

### Update local hosts file

Add host names to `C:\Windows\System32\drivers\etc\hosts` for example:

```
192.168.1.80 pbox-nuc
13.244.119.138 dbox-aws-ct1
```

### Verify connection

Verify you can connect to the SSH host from Windows Terminal.

```powershell
PS C:\> ssh -p xxxx joshua@dbox-nuc -i ssh/joshua
Enter passphrase for key 'ssh/joshua':

joshua@dbox-nuc:~$
```

### Add local ssh_config file and update extension settings

Add hosts to local file `C:\Users\joshua\ssh\ssh_config` that follows the [SSH config file format](https://man7.org/linux/man-pages/man5/ssh_config.5.html).

```
Host dbox-nuc
      User joshua
	   HostName dbox-nuc
	   Port xxxx
   IdentityFile C:\Users\joshua\ssh\joshua

Host dbox-aws-ct1
      User joshua
      HostName dbox-aws-ct1
      Port xxxx
   IdentityFile C:\Users\joshua\ssh\joshua

Host dbox-ovh-uk2
      User joshua
      HostName dbox-ovh-uk2
      Port xxxx
   IdentityFile C:\Users\joshua\ssh\joshua

Host dbox-gcp-bg1
      User joshua
      HostName dbox-gcp-bg1
      Port xxxx
   IdentityFile C:\Users\joshua\ssh\joshua
```

Update Remote SSH Extension config file setting.

![Update extension setting](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_02.png)

### Connect to Remote Host

Open command palette (`Ctrl`+`Shift`+`P`) and select **Remote-SSH: Connect to Host...**<br>Select from list of remote hosts.

![List of remote hosts](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_03.png)

Enter passphrase for ssh key.

![Enter passpharse](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_04.png)

VS Code connects to remote host and displays connection information in status bar.

![Status bar](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_05.png)

You can also confirm that you're now working on Debian host by opening VS Code Terminal (`Ctrl`+`Shift`+`).

![VS Code Terminal](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_06.png)

### Open Remote Project

Next **Open Folder** or **Clone Repository** on Debian host.<br>
**Open Folder** will display remote host file system.

![Open Folder](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_07.png)

After opening remote project for the first time it will show up in Recent Workspaces list (`Ctrl`+`R`) with `[SSH: dbox-nuc]` label after project name.

![Recent Workspaces](https://raw.githubusercontent.com/joshuasa/remote-work-ecosystem/main/images/vscode-remote-ssh_08.png)