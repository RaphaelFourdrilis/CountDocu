% Gisgraphy setup

Prerequisites
=============

Oracle JDK 8
------------

```zsh
wget --no-cookies --no-check-certificate --header \
"Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.rpm"
 
sudo yum localinstall jdk-8u161-linux-x64.rpm
rm ~/jdk-8u161-linux-x64.rpm

# Test if correctly installed:
java -version
```

Postgres 10
-----------

```zsh
rpm -Uvh https://yum.postgresql.org/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm
yum install postgresql10-server postgresql10
/usr/pgsql-10/bin/postgresql-10-setup initdb
systemctl enable postgresql-10.service
systemctl start postgresql-10.service
```
    
Postgis 2
---------

```zsh
yum install epel-release
yum install postgis2_10
```
    
Gisgraphy
---------

Installation instructions: https://www.gisgraphy.com/documentation/installation/setuplinux.php

1. Connect to postgres user then

    ```zsh
    createdb gisgraphy
    createuser --interactive
    # Inside the new prompt, type username:
    gisgraphy
    # Then accept as dbadmin
    y
    
    psql -d gisgraphy -f /usr/share/pgsql/contrib/postgis/2.4/postgis.sql
    psql -d gisgraphy -f /usr/share/pgsql/contrib/postgis/2.4/spatial_ref_sys.sql
    ```
     
2. Create new gisgraphy user (linux)
    
3. While still connected as postgres:
    
    ```zsh
    psql
    
    # Then inside psql prompt:
    ```
    
    ```sqlpostgresql
    ALTER ROLE gisgraphy WITH ENCRYPTED PASSWORD 'password';
    GRANT ALL PRIVILEGES ON DATABASE gisgraphy TO gisgraphy;
    ```
   
4. Open pg_hba.conf in postgres install dir and add these before all other similar lines:
    
    ```ini
    local  all     all                                        password
    host   all     all    127.0.0.1         255.255.255.255   password
    ```

5. De-acivate firewalld 
	
	```zsh
    systemctl disable firewalld
    systemctl stop firewalld
	```
	
Installation
============

Get the latest release
----------------------

First, connect as the `gisgraphy` user and
[download the latest Gisgraphy release](https://www.gisgraphy.com/download/index.php).

```zsh
# Direct link to 5.0-beta2
wget http://download.gisgraphy.com/releases/gisgraphy-5.0-beta2.zip

# Extract wherever you want
unzip gisgraphy-5.0-beta2.zip
rm gisgraphy-5.0-beta2.zip
```

Fix erroneous jdbc driver
-------------------------

Then, until the Gisgraphy developpers change this themselves, you have
to replace some dependencies in
`gisgraphy_install_dir/webapps/ROOT/WEB-INF/lib`.

- Go to [this website](https://jar-download.com/?detail_search=g:%22org.springframework%22%20AND%20a:%22spring-jdbc%22&search_type=av&a=spring-jdbc)
and download version `5.0.X`.

- Then, go to [this website](https://jar-download.com/?detail_search=g:%22org.postgresql%22%20AND%20a:%22postgresql%22&search_type=av&a=postgresql)
and download version `42.2.X` for JRE 8+ (__not__ for JRE 6 or 7)

You will probably have to use WinSCP or any equivalent, since the
website doesn't provide direct links; the urls are re-generated every time ...

Initialize the databse
----------------------

```zsh
# From inside the extracted gisgraphy release
psql -d gisgraphy -f sql/create_tables.sql
psql -d gisgraphy -f sql/insert_users.sql
```

Once this is done, open `webapps/ROOT/WEB-INF/classes/jdbc.properties`
in your favorite editor and replace the existing user and password in
both places it is required in this file by the once you chose when
creating the gisgraphy user.

> Note that both the postgresql gisgraphy user and the unix one must
have the same password

Start Gisgraphy
---------------

Go back to the root of the Gisgraphy distribution and start the service

```zsh
chmod +x *.sh
./launch.sh
```

