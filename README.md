# VM Setup
Documentation for [Vultr servers](https://www.vultr.com/?ref=7096195) based on [Digital Ocean's tutorial for hardening servers](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04) and thoughtbot's guide to creating a [remote development machine](https://robots.thoughtbot.com/remote-development-machine).

## [Create VM on Vultr](https://www.vultr.com/?ref=7096195)
Pick your region, size, latest version of Ubuntu, enable IPV 6, block storage, and enable SSH keys.

## Create User
1. Sign in as root and create a new user: 
```
adduser [name]
```
2. Give sudo access to new user: 
```
usermod -aG sudo [name]
```

## Disable Password Authentication
3. Copy ssh public key from Vultr control panel to remove machine. ([details](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04))
4. As root or your new sudo user, open the SSH daemon configuration:
```
sudo nano /etc/ssh/sshd_config
```
5. Update the following settings:
```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
```
6. Type this to reload the SSH daemon:
```
sudo systemctl reload sshd
```
7. Exit and try to login without a password:
```
ssh name@ip address
```

## Configuration
8. Sign into your vm and run this command: 
```
bash <(wget -qO- https://raw.githubusercontent.com/hirefrank/init/master/init.sh)
2>&1 | tee ~/init.log
```
9. Clone the dotfiles repo:
```
git clone git://github.com/hirefrank/dotfiles.git ~/dotfiles
```
10. Clone the dotfiles-local repo:
```
git clone git://github.com/hirefrank/dotfiles-local.git ~/dotfiles-local
```
11. Initialize:
```
env RCRC=$HOME/dotfiles/rcrc rcup
rcup
```

## Set Up a Basic Firewall
12. View ufw:
```
sudo ufw app list
```
13. Allow mosh:
```
sudo ufw allow mosh
```
14. Allow OpenSSH:
```
sudo ufw allow OpenSSH
```
15. Enable the firewall by typing:
```
sudo ufw enable
```
16. Type "y" and press ENTER to proceed. You can see that SSH connections are still allowed by typing:
```
sudo ufw status
```
