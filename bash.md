[back to overwiev](/../..)

# Bash Cheatsheet

Note that this CheatSheet is biased towards MacOS (as this is my working device)

##### Table of Contents

[Misc](#misc)  
[File management](#file-management)  
[Server](#server)

## Misc

### Kill open port in OSX

```bash
sudo lsof -i :<port>
sudo kill -9 <port PID>
```

## File management

### OSX Bash script not executed "command not found".

```
sudo chmod a+x ./app/build.sh
```

## Server

### Install nodejs on ubuntu

https://github.com/nodesource/distributions/blob/master/README.md#debinstall

```bash
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### SSH

- Generate

```bash
ssh-keygen -R
```

- Copy

```bash
cat ~/.ssh/id_rsa.pub
```

### Connect to a server

```bash
ssh root@<IP>
```

### Transfer files

```bash
scp <source> <destination>
```

To copy remotely

```bash
scp /path/to/file username@a:/path/to/destination
```

To copy from remote

```bash
scp username@b:/path/to/file /path/to/destination
```

To copy a folder use `-r`

```bash
scp -r /path/to/file username@a:/path/to/destination
```

### My Startup resources

```bash
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g pnpm
npm install pm2 -g
apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
