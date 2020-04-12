# symfony5-angular9-docker-compose
Docker-compose configuration for projects using Symfony 5 + Angular 9 + MySQL

Prerequisites & Notes
---------------------
This configuration was only tested on Windows 10 Pro.

It supposes that you have already installed [Docker for Windows 10](https://docs.docker.com/docker-for-windows/install/) and [Docker-compose for Windows 10](https://docs.docker.com/compose/install/).


Installation and configuration
------------------------------
1) Clone this repository inside your project folder
```bash
$ git clone https://github.com/bastienvanderplaetse/symfony5-angular9-docker-compose.git .
```
2) Complete `.env` file with your project values:
- `MY_SQL_ROOT_PASSWORD`: password for root account of the MySQL database
- `MYSQL_DATABASE`: name of the MySQL database that should be used for your project
- `MYSQL_USER`: username for the user account of the MySQL database
- `MYSQL_PASSWORD`: username's password to access the MySQL database
- `SYMFONY_APP`: absolute path to the root of your Symfony 5 project
- `ANGULAR_APP`: absolute path to the root of your Angular 9 project

Setup for Angular 9 project
---------------------------
If you want to create a new Angular 9 project, follow the following steps. Otherwise, just put the absolute path to your project inside the `ANGULAR_APP` environment variable inside the `.env` file.

New Angular 9 project creation steps:
1) Move into the `angular-app` directory
```bash
$ cd angular-app
```
2) Execute the following Docker command to generate a new Angular 9 project (N.B. this step was adapted from [https://github.com/talohana/angular-docker-dev-environment](https://github.com/talohana/angular-docker-dev-environment))
```bash
$ docker-compose -f docker-compose.seed.yml run seed
```
You will be prompted to choose:
* the name of your application (put whatever you want),
* if you want to add or not Angular routing (follow what you need for your application),
* the stylesheet format you want to use.
3) In the `package.json` file inside the Angular project directory, replace 
```json
"start": "ng serve"
```
by 
```json
"start": "ng serve --host 0.0.0.0"
```
4) Change the value of the `ANGULAR_APP`environment variable inside the `.env` file to the absolute path to the `angular-app` directory.

N.B. You can change the location of the Angular project by copy-pasting the content of ```angular-app``` directory into the desired location and by changing the ```ANGULAR_APP``` environment variable with the correct absolute path.

Your Angular 9 project is ready to use !

Setup for Symfony 5 project
---------------------------
If you want to create a new Symfony 5 project, follow the following steps. Otherwise, just put the absolute path to your project inside the `SYMFONY_APP` environment variable inside the `.env` file.

New Symfony 5 project creation steps:
1) Change the value of the `SYMFONY_APP`environment variable inside the `.env` file to the absolute path of the location where you want to put your Symfony project.
2) Build the docker images inside the folder cloned with this repository
```bash
$ docker-compose build
```
3) Start up the docker containers
```bash
$ docker-compose up -d
```
4) Access to the `app_php` container
```bash
$ docker exec -it -u www-data app_php bash
```
5) Create a symfony project
```bash
$ composer create-project symfony/skeleton my-temp-folder
```
6) Copy the project to the right location (the files will appear in the location set in the `.env` file)
```bash
$ cp -Rf /home/www/my-temp-folder/. /var/www/app/.
$ rm -Rf /home/www/my-temp-folder
```
7) To exit the `app_php` container, type the following command and press enter
```bash
$ exit
```
8) To stop the containers use this command
```bash
$ docker-compose down
```

Your Symfony 5 project is ready to use !

Usage
-----
Build Docker images and containers (only the first time you want to use them)
```bash
$ docker-compose build
```

Run development environment (may take a long time, specifically for the node container). The container are ready to use when the Angular Live Development server is listening on 0.0.0.0:4200 and the compilation is successfully completed (these informations will be prompted).
```bash
$ docker-compose up
```

Run development environment in background (faster than the previous one)
```bash
$ docker-compose up -d
```

Stop Docker images and containers (not running in background): press `CTRL+C`

Stop Docker images and containers (running in background)
```bash
$ docker-compose down
```

Access to project
-----------------
Symfony project: http://localhost
Angular project: http://localhost:4200
Phpmyadmin : http://localhost:3306

Hacks
------
If you want to work on another project, you can have multiple `.env` files (rename the unused ones) with the correct parameters.

When you want to run containers (specifically for another project), if you are prompted that container names are already used, use the following command:
```bash
$ docker rm -f $(docker ps -a -q)
```