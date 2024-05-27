## Install mysql server in ubuntu 20.04
1. Install Mysql Server `sudo apt-get install mysql-server`
2. Check installation `mysql -V`
3. Display the password by `sudo cat /etc/mysql/debian.cnf`, you will see something
    ```
    [client]
    host     = localhost
    user     = debian-sys-maint
    password = DKtfCGDb0Fn1GlRn
    socket   = /var/run/mysqld/mysqld.sock
    [mysql_upgrade]
    host     = localhost
    user     = debian-sys-maint
    password = DKtfCGDb0Fn1GlRn
    socket   = /var/run/mysqld/mysqld.sock
    ```
4. Remember the password `DKtfCGDb0Fn1GlRn` and type it in the folloing query `mysql -u debian-sys-maint -p`

5. Setup root password
    ``` sql
    USE mysql;
    FLUSH privileges;
    ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'your_password';
    FLUSH privileges;
    quit;
    ```
6. Retart mysql service
    ``` bash
    service mysql restart
    ```
