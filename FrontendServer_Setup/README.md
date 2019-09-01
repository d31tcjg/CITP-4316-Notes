# Frontend Server Setup and Deployment

## Create Github Account

1. Create a [GitHub](https://github.com) Account
2. Add new Repository
3. Select Private
4. Command Promt or Terminal go to Application Folder
5. Configure Git global user
   5.1 `git config --global user.name "Your Name"`
   5.2 `git config --global user.email "Your Email"`
6. Add Github as Origin with `git remote add origin GITHUB_REPO_URL`
7. Push code to Github with `git push -u origin master`

## Create Server

[Digital Ocean](https://digitalocean.com/) Welcome to the developer cloud. They make it simple to launch in the cloud and scale up as you grow—whether you’re running one virtual machine or ten thousand.

1. Go to Digital Ocean with the previus link and create an account or [Click this link](https://m.do.co/c/ce761b3417ec) for a \$50 credit for 30 days (pretty much a month free)
2. Create a \$5 Droptlet with Ubuntu 18.04, selecting a temporary password option.
3. Copy the IP Address when the server is created.
4. Login to the server with [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) on Windows or ssh command on mac
   4.1 ssh root@SERVER_IP_ADDRESS

## Update Server

1. Update server components list with `apt update`
2. Upgrade server components with `apt upgrade`

## Add Normal Sudo User and disable Root remote Login

1. `adduser username` Where username is the name that you want to use
   1.1 Type password and information needed
2. `usermod -aG sudo username` Where username is the user created in previous step
3. Test user conenction exits the server and acces now with `username` instead of `root`
4. Log back with root and execute `nano /etc/ssh/sshd_config`
5. Change `PermitRootLogin yes` to `PermitRootLogin no` and `#PermitEmptyPasswords no` to `PermitEmptyPasswords no`
6. Exit and save with `Ctrl+x` and then `Y` and finally `Enter`
7. Restart ssh with `/etc/init.d/ssh restart`
8. Exit the server

## Add Firewall

1. Login with your username account (not root)
2. Type `sudo ufw status` to view firewall status
3. Install Firewall with `sudo apt-get install ufw`
4. Enable Ports on Firewall
   4.1 `sudo ufw allow ssh`
   4.2 `sudo ufw allow http`
   4.3 `sudo ufw allow https`
5. Enable Firewall `sudo ufw enable`
6. Type `sudo ufw status` to view firewall status

## Install Nginx (HTTP Server)

1. Install with `sudo apt install nginx`
2. Start nginx `systemctl start nginx`

## Install Node

1. Install NVM with `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
2. Install Node.js with this command: `nvm install node`

## Install Yarn

1. `curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -`
2. `echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list`
3. `sudo apt-get update && sudo apt-get install --no-install-recommends yarn`

## Build Website

1. Clone Github repo with `clone GITHUB_REPO_URL`
2. Install project dependencies goinc inside the folder and type `yarn`
3. Build with `yarn build`
4. Give user permissions to Nginx Folder `sudo chown -R /var/www/html/`
5. Remove content of Nginx folder with `rm /var/www/html/*`
6. Copy dist folder to nginx folder with `cp dist/* /var/www/html/`

## Add domain to Digital Ocean

1. DO Console click on DNS
2. Add Site and point @ to server
3. On your domain registrar (Probably [GoDaddy](https://www.godaddy.com/))
4. Change Name Servers to the One that DO Have

## Add Certbot to have Https on teh server

```bash
    sudo apt-get update
    sudo apt-get install software-properties-common
    sudo add-apt-repository universe
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get update
    sudo apt-get install certbot python-certbot-nginx
    sudo certbot --nginx
    sudo certbot certonly --nginx
```