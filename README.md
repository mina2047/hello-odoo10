# Quick install

Installing Odoo 10 with one command.

(Supports multiple Odoo instances on one server)

Install [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/) yourself, then run:

``` bash
curl -s https://raw.githubusercontent.com/mina2047/Hello-odoo10/master/run.sh | sudo bash -s odoo-ten 10010 20010
```

to set up first Odoo instance @ `localhost:10010` (default master password: `mostafa@1234`)

and

``` bash
curl -s https://raw.githubusercontent.com/mina2047/Hello-odoo10/master/run.sh | sudo bash -s odoo-two 11010 21010
```

to set up another Odoo instance @ `localhost:11010` (default master password: `mostafa@1234`)

Some arguments:
* First argument (**odoo10-ten**): Odoo deploy folder
* Second argument (**10010**): Odoo port
* Third argument (**20010**): live chat port

If `curl` is not found, install it:

``` bash
$ sudo apt-get install curl
# or
$ sudo yum install curl
```

# Usage

Start the container:
``` sh
docker-compose up
```

* Then open `localhost:10010` to access Odoo 10.0. If you want to start the server with a different port, change **10010** to another value in **docker-compose.yml**:

```
ports:
 - "10010:8069"
```

Run Odoo container in detached mode (be able to close terminal without stopping Odoo):

```
docker-compose up -d
```

**If you get the permission issue**, change the folder permission to make sure that the container is able to access the directory:

``` sh
$ git clone https://github.com/mina2047/Hello-odoo10.git
$ sudo chmod -R 777 addons
$ sudo chmod -R 777 etc
$ mkdir -p postgresql
$ sudo chmod -R 777 postgresql
```

Increase maximum number of files watching from 8192 (default) to **524288**. In order to avoid error when we run multiple Odoo instances. This is an *optional step*. These commands are for Ubuntu user:

```
$ if grep -qF "fs.inotify.max_user_watches" /etc/sysctl.conf; then echo $(grep -F "fs.inotify.max_user_watches" /etc/sysctl.conf); else echo "fs.inotify.max_user_watches = 524288" | sudo tee -a /etc/sysctl.conf; fi
$ sudo sysctl -p    # apply new config immediately
```

# Custom addons

The **addons/** folder contains custom addons. Just put your custom addons if you have any.

# Odoo configuration & log

* To change Odoo configuration, edit file: **etc/odoo.conf**.
* Log file: **etc/odoo-server.log**
* Default database password (**admin_passwd**) is `mostafa@1234`, please change it @ [etc/odoo.conf#L60](/etc/odoo.conf#L60)

# Odoo container management

**Run Odoo**:

``` bash
docker-compose up -d
```

**Restart Odoo**:

``` bash
docker-compose restart
```

**Stop Odoo**:

``` bash
docker-compose down
```

# Live chat

In [docker-compose.yml#L21](docker-compose.yml#L21), we exposed port **20010** for live-chat on host.

Configuring **nginx** to activate live chat feature (in production):

``` conf
#...
server {
    #...
    location /longpolling/ {
        proxy_pass http://0.0.0.0:20010/longpolling/;
    }
    #...
}
#...
```

# docker-compose.yml

* odoo:10.0
* postgres:12

# Odoo 10 screenshots

<img src="screenshots/2022-10-17_22h16_21.png" width="50%">

<img src="screenshots/2022-10-17_22h16_30.png" width="100%">

<
