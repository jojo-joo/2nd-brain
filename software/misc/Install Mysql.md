## Install mysql server in ubuntu 20.04
1. Install Mysql Server 
    ``` bash
    sudo apt-get install mysql-server
    mysql -V
    sudo cat /etc/mysql/debian.cnf
    mysql -u debian-sys-maint -p    # using the password in debian.cnf
    ```
2. Setup root password
    ``` sql
    USE mysql;
    FLUSH privileges;
    ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'your_password';
    FLUSH privileges;
    quit;
    ```

3. Start mysql service
    ``` bash
    service mysql restart
    ```
