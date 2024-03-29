# Generic LAMP local dev for Docker

This is a lihgtweight LAMP stack for local development on Docker for Mac. Total download size of the images is less than 200 MB. There's no need to build any container images locally as all images are build in Docker Hub. Fast and simple set up.

## Requrements

- Docker for Mac 2.0+

**Recommended:** Install [Homebrew](https://brew.sh)

## Getting started

MySQL credentials are in `conf/mysql/credentials.env`. Usually there's no need to change them.

To make sure you've got latest images:
- `docker-compose pull`

To start LAMP stack:
- `docker-compose up -d`

Then you can browse to http://127.0.0.1:8080/

### MySQL database access

You can use MySQL clients like mysqldump, mysql CLI and Sequel Pro on `127.0.0.1:3306`.

If your workstation is missing MySQL CLI tools you can install them from Homebrew:

```
brew update
brew install mysql-client
brew link --force mysql-client
```

In order to use WP-CLI and Drush on you local Terminal you need to add `127.0.0.1 mysql` line to */etc/hosts* file:

```
sudo nano /etc/hosts
```

Then you can call MySQL CLI for example like this: `mysql -hmysql -umydbuser -pmydbpwd mydb`

### Example WordPress project bootstrapping

Place your project files under `projects` directory.

Get Composer from Homebrew:

```
brew update
brew install composer
```

For example to initialize [WP Bedrock](https://roots.io/bedrock/docs/installing-bedrock/) project:
1. `cd projects`
1. `composer create-project roots/bedrock myproject`
1. `cd web`
1. `ln -s ../myproject/web myproject`
1. Create .env file to `projects/myproject/.env`:

```
DB_NAME='mydb'
DB_USER='mydbuser'
DB_PASSWORD='mydbpwd'
DB_HOST='mysql'
WP_ENV='development'
WP_HOME='http://127.0.0.1:8080/myproject'
WP_SITEURL='http://127.0.0.1:8080/myproject/wp'
AUTH_KEY='d*i|rmW`c.sA|4kJ<:!h)NgR7,_IAKY-%dypa)nt93aQ-b1lYvumzTU$V!3>bG?l'
SECURE_AUTH_KEY='m3}cITN3[Pzv/cs6FtR344#Ujctmu}#RNOX6Ens93ndL+j&An4(h2;_{7WMFR:3n'
LOGGED_IN_KEY='0!4aoZOK?&0@Nv+O8vAM,/6QmfZV-H}M@{#HXSA/[E(D^)r>Nhx/r;$-rRbhuSk#'
NONCE_KEY='>FakHf1H$%{8>OPLhez?`z;IkVE0wB=[[uF!Z*[rOoo&I:gY/MN5|#G#TDrvEYTq'
AUTH_SALT='[t@dDPX4!VSV=>zt&d;2iohAOGnYH4]Av/&IFW<pc:-%9R/8nYFK`.3[m.4O]nD]'
SECURE_AUTH_SALT='+o,wb9EQbz<L5<q2w&.%<6%;yL)CzPa,]p}XpM/keLdZd/vm$9`^.]5PTU@DIF*{'
LOGGED_IN_SALT='8]He1IS8UdD(v+g30ujx9;u3QRkQy*bP!kz5jUkEaG9R)|Bd1D9f51h0+`wqLr!5'
NONCE_SALT='U/GkE@dX@`+k6gS?nlM!YHBEAG;YX5N;S6`h)y4j9{ta<w[-t!Xm;[+/e|dof,{1'
```

Then open http://127.0.0.1:8080/myproject/ to launch WordPress install wizard.

**WP-CLI**

```
brew update
brew install wp-cli
```

### Example Drupal 8 project bootstrapping

Place your project files under `projects` directory.

Get Composer from Homebrew:

```
brew update
brew install composer
```

Install Drupal 8 core:

1. `cd projects`
1. `composer create-project drupal-composer/drupal-project:8.x-dev myproject`
1. `cd web`
1. `ln -s ../myproject/web myproject`

Then open http://127.0.0.1:8080/myproject/ to launch Drupal install wizard.


**Drush**

Install [Drush Launcher](https://github.com/drush-ops/drush-launcher) so that you can use project spesific Drush releases.

```
curl -OL https://github.com/drush-ops/drush-launcher/releases/download/0.6.0/drush.phar
chmod +x drush.phar
sudo mv drush.phar /usr/local/bin/drush
```

Now you should be able to call `drush` command in every directory. You can for example run site install command:

```
cd projects
cd myproject
drush si standard --db-url=mysql://mydbuser:mydbpwd@mysql/mydb
```

### Logs

Open a new Terminal window/tab and start real-time log watch: `docker-compose logs -f`

### Xdebug

Xdebug PHP extension is enabled by default. Remote debugging is disabled by default.

To enable remote debugging edit file `conf/php/xdebug.ini` and restart PHP container: `docker-compse restart php`

## Removing environment from Docker

To stop and remove containers:
- `docker-compose rm -s`

To stop and remove containers and MySQL data volume:
- `docker-compose rm -s -v`

