# Airflow  Deployment on AWS EC2 Instance

Requirements

Airflow version (2.0.0)

Python versions: 3.5, 3.6, 3.7
MySQL DB: 5.6, 5.7
Sqlite - latest stable 

Additional notes on Python version requirements

Stable version requires at least Python 3.5.3 when using Python 3
Both versions are currently incompatible with Python 3.8 due to a known compatibility issue with a dependent library


Getting started

Please visit the Airflow Platform documentation (latest stable release) for help with installing Airflow, getting a quick start, or a more complete tutorial.

Installing Docker on Ubuntu 18.04

apt-get update

apt-get install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
apt-get update

apt-get install docker-ce docker-ce-cli containerd.io -y

Installing MySQL DB

sudo apt update

sudo apt install mysql-server

Configuring MySQL

sudo mysql_secure_installation

Adjusting User Authentication and Privileges

sudo mysql

SELECT user,authentication_string,plugin,host FROM mysql.user;

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

FLUSH PRIVILEGES;

SELECT user,authentication_string,plugin,host FROM mysql.user;

exit

Login inot mysql with root using password


mysql -u root -p

Note: Create a password with 1 Capital letter 1 special charecter 1 number and length 8 

Create a new user and give it a strong password:

CREATE USER 'sammy'@'localhost' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;

exit

Testing MySQL

systemctl status mysql.service

MYSQl version Check

sudo mysqladmin -p -u root version


How to connect Airflow to MySQL:

Change the config file in the Airflow repo 

sql_alchemy_conn = mysql://{USERNAME}:{PASSWORD}@{MYSQL_HOST}:3306/airflow

Login in MYSQl DB

CREATE DATABASE airflow CHARACTER SET utf8 COLLATE utf8_unicode_ci; 
CREATE USER 'airflow'@'34.68.35.27' IDENTIFIED BY '<StrongPassword>';
grant all on airflow.* TO 'airflow'@'<airflowinstanceip>' IDENTIFIED BY '<StrongPassword>';


Installing Airflow Stable version 

Clone github branch into ec2 instance

git clone https://github.com/gnishanth4/airflow-dev.git 

cd airflow-dev 

docker build -t airflow .

docker run -p 8080:8080 -d --name airflow-dev airflow


Ports need to open in Security groups

Airflow Port 8080
MySql Port   3306







