---
title: Guide To Install SonarQube Server
date: "2019-11-15T23:16:32.169Z"
description: SonarQube is a popular static code analyze tool and in this post we will cover how to install SonarQube server on Ubuntu.
---

SonarQube is a popular static code analyze tool and in this post we will cover how to install SonarQube server on Ubuntu.

As I already mentioned, SonarQube is one of the most popular static code analyzer tool and it's installation and usage is very simple. In this guide we will use PostgreSQL as database solution.

## PostgresSQL Installation and Configuration

### Install PostgreSQL

Let's start with database installation. PostgreSQL installation is also very simple, we can execute following commands to install postgres:

```sh
# Add PostgreSQL apt repository
echo 'deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main' >> /etc/apt/sources.list.d/pgdg.list

# Import the repository signing key, and update the package lists
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - 

# Install PostgreSQL
sudo apt-get update
sudo apt-get install postgresql-10
```
   
### Change postgres user password and authentication type

After PostgreSQL installation we will connect PostgreSQL and create database with the name SonarQube. But before that, we should change PostgreSQL authentication type from IDENT to MD5. In order to do that, we should first change our user(we connected as root) to postgres and then connect to PostgreSQL: 

```sh
su - postgres psql
``` 

then change password with the 

```sh
\password
``` 

command. This command prompt you a new password and confirmation inputs. After that you can quit from PostgreSQL with: 

```sh
\q
```

After password change, we should check authentication type by looking at pg_hba.conf file as shown in below:

```sh
cat /var/lib/pgsql/10/data/pg_hba.conf
```

This file's content should have a part like below (the important part is md5 column):

```properties
# IPv4 local connections:
host    all              all             127.0.0.1/32             md5
# IPv6 local connections:
host    all              all             ::1/128                  md5</pre>
```

If the file contains configuration like below you should update to md5 instead.

```properties
# IPv4 local connections:
host    all              all             127.0.0.1/32             ident
# IPv6 local connections:
host    all              all             ::1/128                  ident
```

### Create database for Sonarqube

To use PostgreSQL for database solution for SonarQube we should create sonarqube database using following command:
            
```sql
CREATE DATABASE sonarqube;
```

### Create sonarqube User

The next step coming after postgres configuration is creating unix user for sonarqube. 

```sh
adduser sonarqube
```

This command will ask for password and then prompt to set new user informations. After typing desired inputs, our user will be created.

After sonarqube user creation, let's add our user into sudo group
```sh
usermod -aG sudo sonarqube
```

## Download & Configure SonarQube

Download latest version of sonarqube from <a href="https://www.sonarqube.org/downloads/" target="_blank">https://www.sonarqube.org/downloads/</a> and extract it to folder as you desire.

Edit `$SONARQUBE-HOME/conf/sonar.properties` file to configure postgresql access.

```properties
sonar.jdbc.username=sonarqube
sonar.jdbc.password=mypassword
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
```

And it is highly recommended to update Elasticsearch storage paths in the same file(`$SONARQUBE-HOME/conf/sonar.properties`).

```properties
sonar.path.data=/var/sonarqube/data
sonar.path.temp=/var/sonarqube/temp
```
   
### Start SonarQube Server

Execute the following script to start the server:

```sh
$SONARQUBE-HOME/bin/sonar.sh start
```

Default port for sonarqube is 9000 and default System administrator credentials are admin/admin


### Solution for elastic-search related vm.max_map_count error

After initial start of server first check if there is any error from log files stored in `$SONARQUBE-HOME/logs` directory. If you see the `max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]` error you can resolve this by executing following command as root:

```
sysctl -w vm.max_map_count=262144
```


---

references:


- <a href="http://yallalabs.com/linux/how-to-install-and-use-postgresql-10-on-ubuntu-16-04/" target="_blank">http://yallalabs.com/linux/how-to-install-and-use-postgresql-10-on-ubuntu-16-04/</a>
- <a href="https://www.liquidweb.com/kb/change-postgresql-authentication-method-from-ident-to-md5/" target="_blank">https://www.liquidweb.com/kb/change-postgresql-authentication-method-from-ident-to-md5/</a>
- <a href="https://www.guru99.com/postgresql-create-database.html" target="_blank">https://www.guru99.com/postgresql-create-database.html</a>
- <a href="https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart" target="_blank">https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart</a>
- <a href="https://docs.sonarqube.org/latest/setup/install-server/" target="_blank">https://docs.sonarqube.org/latest/setup/install-server/</a>
- <a href="https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html" target="_blank">https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html</a>