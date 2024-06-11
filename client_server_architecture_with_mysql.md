### CLIENT SERVER ARCHITECTURE WITH MYSQL

#### Understanding client server architecture

client server architecture refers to an architecture in which two or more computers are connected together
over a network to send and receive requests amongst each other.

each computer has its own role, the machine sending the request is usually referred to as the client, 
while the one receiving it is called the server.



#### TASK: IMPLEMENT A CLIENT-SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM(DBMS)

1) create and two linux based virtual servers (EC2 instances AWS)

    server A name = 'myssql server'
    server B name = 'mysql client'

2) on mysql linux server install mysql server software

    sudo apt update
    sudo apt install mysql-server
    sudo mysql_secure_installation

verify mysql server is running
    sudo systemctl status mysql






3) do same thing on my client server

4) by default both EC2 instance's are located in same local virtual network,so they can communicate with each other using local ip-addresses,
use mysql servers local ip address to connect from mysql client. mysql server uses TCP port 3306 by default , so you will have to open it by creating a new entry in inbound rules,
in mysql server. For extra security do not allow all ip addresses to reach your 'mysql server' allow access to only specific local ip address of your 'mysql client'.


5) we might need to configure mysql server to allow connection from remote host.

    sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

    Replace '127.0.0.1' with '0.0.0.0'



6) from mysql client linux server connect remotely to mysql server database engine without using ssh. we will use mysql utility to perform this action:

    #### Configure the MySQL Server

        Edit MySQL Configuration:
        On the MySQL server instance, edit the MySQL configuration file to allow connections from any IP address. The configuration file is usually located at /etc/mysql/mysql.conf.d/mysqld.cnf or /etc/my.cnf.

        sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

        Look for the line:
        bind-address = 127.0.0.1

        Change it to:
        bind-address = 0.0.0.0




    Restart MySQL Service:
    Restart the MySQL service to apply the changes.

        sudo systemctl restart mysql


    Create a Remote User:
    Log in to the MySQL server and create a user that can connect remotely. Replace username, password, and client-ip with your preferred values. If you want to allow access from any IP address, use % instead of client-ip.

        sudo mysql -u root -p

    Then, in the MySQL shell, execute:

        CREATE USER 'username'@'client-ip' IDENTIFIED BY 'password';
        GRANT ALL PRIVILEGES ON *.* TO 'username'@'client-ip' WITH GRANT OPTION;
        FLUSH PRIVILEGES;



    #### Connect from MySQL Client

    Connect to MySQL Server:
    Use the MySQL client utility to connect to the MySQL server. Replace server-ip, username, and password with the appropriate values.

        mysql -h server-ip -u username -p

