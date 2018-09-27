# System Admin

##AWS , apt-get , Bash , NodeJS , Nginx , Git

### Index

- [Flow to Create New Instance](#Simple flow for a new project in a new instance)
- [Remote Access](#Remote Access)
- [In Remote Machine](#In Remote Machine)
  - [Users / Password](#Add new user / pw)
  - [Update & Upgrade](#Update & Upgrade)
  - [Reboot](#Reboot)
- [Packages](#Install necessary packages)
  - [Node.js](#Install Node.js)
  - [Other packages](#Install Packages)
  - [Common Commands](#Common system admin commands)
- [Setup a Node.JS app](#Setup NodeJS app)
  - ["Forever"](#Set-up-server-to-run-forever)
  - [Nginx](#Nginx - reverse proxy)
  - [HTTPS](#Set up HTTPS)
  - [Cron job](#Cron job)
- [Git Deployment](#Git Deployment)
- [Command not found](#Command not found)

---

#### Deploy

Domain : a pretty address to tell the world

DNS : tell the world which public IP to go to for certain domain

Machine / Server : place actually store stuff to serve, its address on Internet is public IP

ssh : remote access to Server , see below

---

#### Simple flow for a new project in a new instance

[1. Create new instance , assign elastic IP , setup DNS](#)

[2. Move pem key into `~/.ssh` and set config](#Remote Access)

[3. Set-up EC2 environment](#In Remote Machine)

[4. Install packages](#Install Packages)

[5. Set-up Nginx](#Nginx - reverse proxy)

[6. Use Certbot to set up HTTPS>](#Set up HTTPS)

[7. Set-up Git](#Git Deployment)

---

###Remote Access

```ubuntu
// @local
cd ~
cd .ssh
ls -lrt
// create a config file if do not have one
touch config
chmod 400 config
// modify config
nano config
// inside config:
Host <your elastic IP>
   User ubuntu
   IdentityFile <location of your private key .pem file>
// remote access
ssh ubuntu@<your elastic IP>
```

---

#### SSH (Secure Shell) to access remote machine

```ubuntu
// 2 ways of authentication : 
// A : 
ssh <username>@<ip>
<pw>
// B :
// inside ~/.ssh/ contains file authorized_keys
// local machine has key ~/.ssh/id_rsa.pub matches remote machine's authorized_keys = done
ssh <username>@<ip>
```

---

### In Remote Machine

####Add new user / pw

```ubuntu
// create temp user
$ sudo su
# passwd root
enter pw: <ur pw> (pw)
re-enter: <ur pw>

// del pw when u r done
# passwd –d root
```

---

#### Update & Upgrade

```ubuntu
sudo apt-get update
sudo apt-get upgrade
```

---

#### Reboot instance

1. aws webpage > Instance > Actions > Instance State > Reboot > Yes

2. aws ec2 reboot-instances --instance-ids i-1234567890abcdef5

---

#### Reboot system

```
sudo reboot
```

---

###Install necessary packages

#### Install Node.js

https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html

```ubuntu
// in ssh
// install nvm :
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
// activate nvm
. ~/.nvm/nvm.sh
// nvm install node.js
nvm install 8
// or install latest
nvm install --lts
// test/check ver using :
node -e "console.log('Running Node.js ' + process.version)"

// another way to do so
// add the NodeSource repository to your server
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

// install the latest NodeJS
sudo apt-get install -y nodejs
    
// Also install build essential for native addon
sudo apt-get install -y build-essential

// check ver
node --version
```

---

####Install Packages

install the following : `build-essential`,`nginx`,`htop`,`git` , `curl`, `postgresql` and `postgresql-contrib`.

```ubuntu
// install
sudo apt-get install <package-name>
// remove package only
sudo apt-get remove <package-name>
// remove package n config
sudo apt-get purge <package-name>
// list all pkg installed
dpkg -l
// search a pkg
apt-cache search <package-name>
// install non-ubuntu-offical software
sudo add-apt-repository ppa:<repository-name>
```

---

#### Common system admin commands

```ubuntu
// colorful command process manager htop (fancier than `ps`)
htop

// service
sudo service nginx start
sudo service nginx stop
sudo service nginx restart

// SCP secure copy
// file
scp my_local_files <username>@<hostname>:
// folder
scp -r my_folder <username>@<hostname>:
```

---

### Setup NodeJS app

```ubuntu
// Navigate to your home directory by cd ~, make a new file
nano hello.js

// in hello.js
#!/usr/bin/nodejs

var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World\n');
}).listen(8080,'localhost');
console.log("Server running at http://localhost:8080/");
// save n exit

// grant execution permission
chmod +x  ./hello.js

// start server by running hello.js
./hello.js
> Server running at http://localhost:8080/
```

Go to another terminal

```ubuntu
// ssh to server
// test the response
curl localhost:8080
> Hello World

// check if nodejs is running
ps -ef | grep hello.js
> <user_name>   <Process_id> <Parent_Process_id>  1 <time> pts/1    00:00:00 /usr/bin/nodejs ./hello.js

// stop server
kill -9 <process_id>
// or ctrl + c
```

---

#### Set up server to run forever

so that server wont shut down after SSH connection is terminated

```ubuntu
// install process manager 'forever'
sudo npm install -g forever

// to stop
forever stop app.js

// to restart
forever restart app.js
forever restart <process id>

// details
forever list
> info:    Forever processes running
> data:        uid  command       script forever pid  id logfile                        uptime       
> data:    [0] _1bx /usr/bin/node app.js 7877    7883    /home/gordon/.forever/_1bx.log 0:0:3:24.379 

// check log
forever list
nano <logfile path>	// or
tail <logfile path>
```

---

####Nginx - reverse proxy

- load-balance over multiple application server
- cache & compress static content to speed up loading time
- provide SSL/TLS encryption for the connection
- provide basic access authentication

```ubuntu
// start service
sudo service nginx start

// set up reverse proxy config
// edit default config for nginx
sudo nano /etc/nginx/sites-available/default
// find the location blk and replace with
location / {
    proxy_pass http://localhost:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}

// save n exit , run cmd to test config validity
sudo nginx -t

// restart if ok
sudo service nginx restart
```

---

#### Set up HTTPS

- Free CA cert by 'Let's Encrypt' 
- expire every 90 days
- visit certbot website (https://certbot.eff.org/#ubuntuxenial-nginx) for more info

```ubuntu
// edit nginx config
sudo nano /etc/nginx/sites-available/default

// change `server_name _;` to
server_name mysite.com;

// install certbot on virtual machine
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx

// obtain n install cert
sudo certbot --nginx

// try this if failed
sudo certbot --authenticator standalone --installer nginx --pre-hook "service nginx stop" --post-hook "service nginx start"

// to re-new
certbot renew

// restart nginx
sudo service nginx restart
```

---

#### Cron job

```ubuntu
// start cron job
crontab -e

// check for renew daily
//  Certbot is only going to renew it if the certificate is going to expire in the future 30 days
0 0 */1 * * certbot renew
```

---

https://github.com/foreverjs/forever

https://nginx.org/en/

https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/

---

### Git Deployment

##### Git Hook

```ubuntu
// server-siode
# Create an empty folder
mkdir your_app_name.git
cd your_app_name.git 

# Initialize Reporsitory
git init --bare
# --bare means bare repository
```

hooks are fired when diff actions occur , important ones are as follow :

- `pre-receive`  : runs right before the server started to receive the `git push` from the client
- `update`  : runs once for each push on each branch , if a push is updating multiple branches, `pre-receive` runs once only. `update` runs multiple times
- `post-receive` : runs once when the `git push` is completed.

##### Setup Post-receive Script

```ubuntu
// create / mod post-receive
sudo nano post-receive

// paste content :
#!/bin/bash -l 
GIT_REPO=$HOME/your_app_name.git
PROJECT_DEST=$HOME/your_app

if [ -d $PROJECT_DEST ];then
    git --work-tree=$PROJECT_DEST --git-dir=$GIT_REPO checkout -f 
else
    rm -rf $PROJECT_DEST
    git clone $GIT_REPO $PROJECT_DEST
fi
cd $PROJECT_DEST/
npm install # if it is a npm repository
forever stop ./app.js
forever start ./app.js

exit
// save n exit

// change it to executable
sudo chmod +x post-receive
```

##### Setup remote in local repository

```
// local dev side
// set up remote
git remote add production ubuntu@<your elastic IP>:your_app_name.git

// cmd for del
git remote rm production

// push
git push production master
```

---

### Command not found

some modules are installed in project but not on the machine globally, like knex.js

when you try `knex -V`

the console will say "knex" command not found, do you mean .......

so, navigate to project folder and type :

```bash
$ node_modules/knex/bin/cli.js -h
```

it will be `-h` or `-V` or `migrate:latest` or whatever options available 