# CLIENT-SERVER ARCHITECTURE WITH MYSQL

Understanding Client-Server Architecture
As you proceed with your journey into the world of IT, you will begin to realise that certain concepts apply to many other areas. One of such concepts is – Client-Server architecture.
Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.
In their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".
A simple diagram of Web Client-Server architecture is presented below:
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/P5S1.png)

In the example above, a machine that is trying to access a Web site using a Web browser or simply ‘curl’ command is a client and it sends HTTP requests to a Web server (Apache, Nginx, IIS or any other) over the Internet.
If we extend this concept further and add a Database Server to our architecture, we can get this picture:
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/P5S1.1.png)

In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be an Internet connection, but it is a common practice to place Web Server and DB Server close to each other in a local network).
The setup on the diagram above is a typical generic Web Stack architecture that you have already implemented in previous projects (LAMP, LEMP, MEAN, MERN), this architecture can be implemented with many other technologies – various Web and DB servers, from small Single-page applications SPA to large and complex portals.

This LAMP website server(s) can be located anywhere in the world and you can reach it also from any part of the globe over global network – Interthe net.
Assuming that you go on your browser, and typed in there www.propitixhomes.com. It means that your browser is considered the "Client". Essentially, it is sending requests to the remote server, and in turn, would be expecting some kind of response from the remote server.
Let’s take a very quick example and see Client-Server communicatation in action.
Open up your Ubuntu or Windows terminal and run the curl command:
```
curl -Iv www.propitixhomes.com
```

Note: If your Ubuntu does not have ‘curl’, you can install it by running sudo apt install curl
In this example, your terminal will be the client, while www.propitixhomes.com will be the server.

See the response from the remote server in the below output. You can also see that the requests from the URL are being served by a computer with an IP address 160.153.133.153 on port 80. More on IP addresses and ports when we get to Networking related projects
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%202022-12-20%20at%2022.36.10.png)

Another simple way to get a server’s IP address is to use a simple diagnostic tool like ‘ping’, it will also show round-trip time – time for packets to go to and back from the server, this tool uses ICMP protocol.
#### Side Self Study
1. Read about ping and traceroute network diagnostic utilities. Be able to make sense of the results of using these tools.
2. Refresh your knowledge of basic SQL commands, and be able to perform simple SHOW, CREATE, DROP, SELECT and INSERT SQL queries.

## IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).
### TASK – Implement a Client-Server Architecture using MySQL Database Management System (DBMS).
To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions
**step 1.** Create and configure two Linux-based virtual servers (EC2 instances in AWS).

Server A name - `mysql server`

Server B name - `mysql client`

![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(310).png)

connect your mysql_server

then Update ubuntu
```
sudo apt update
```
Upgrade ubuntu
```
sudo apt upgrade
```
**step 2** - On mysql server Linux Server, we will install MySQL Server software. 
```
sudo apt install mysql-server -y sudo systemctl start mysql.service

sudo systemctl status mysql.service
```

Finally, we will check the status by running 
```
sudo systemctl status mysql 
```
This is what the end result should look like 
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(310).png)

**step 3** -  On **mysql client Linux Server**, we will update ubuntu by running 

```
sudo apt update
```
and Upgrade ubuntu
```
sudo apt upgrade
``` 

afterward, install MySQL Client software with ```sudo apt install mysql-client -y```.

**step 4** By default, both of our EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Or, we can add them to the same subnets.

Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so we will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach our ‘mysql server’ – allow access only to the specific local IP address of our ‘mysql client’.
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(313).png)

**step 5** For MySQL secure installation run the following in your MySQL server : 
```
sudo mysql
```
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
Afterward, type `exit`. 
**Note** that 'PassWord.1' should be replaced with your sql password.
```
sudo mysql_secure_installation
```
When prompted for the password enter the PassWord.1 (or whatever you set when you ran the above SQL query)

![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(314).png)

**step 6** On MySQL server, we will create a database
```
sudo mysql -p
```
```
CREATE DATABASE db_db;
```

**step 7** Now we can create a new user and grant him full privileges on the database we have just created. 

```
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%202023-01-09%20at%2011.27.27.png)

The following command above creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1, but you should replace this value with a secure password of your own choosing.

Now we need to give this user permission over the example_database database: 
```
GRANT ALL ON db_db.* TO 'example_user'@'%';
```

This will give the example_user user full privileges over the db_db database, while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with: `exit`
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%202023-01-09%20at%2011.42.45.png)

**step 8** Lets test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials: 
```
mysql -u example_user -p
```
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(316).png)

Notice the -p flag in this command, which will prompt us for the password used when creating the example_user user. After logging in to the MySQL console, confirm that we have access to the db_db database by running:
```
SHOW DATABASES;
```
This will give you the following output:
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(317).png)

**step 9** Exit MySQL and restart the mySQL service using
```
sudo systemctl restart mysql
```

```
sudo systemctl status mysql.service
```

**step 10** e need to configure MySQL server to allow connections from remote hosts.
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

Replace the value of bind_address from `127.0.0.1` to `0.0.0.0` like this: 
*BEFORE*
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(318).png)

*AFTER*
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%202023-01-08%20at%2012.39.09.png)

Afterward, 
```
run sudo systemctl restart mysql
```
**Step 11** From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action. Check that you have successfully connected to a remote MySQL server and can perform SQL queries:
**NB: make sure you have updated and upgraded your ubuntu, installed mysql software on your mysql_client server**

```
Sudo mysql -u example_user -h <mysql_server ip address> -p
```
Type this in and the database should be visible: 
```
Show databases;
```
![](https://github.com/beorel/CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL/blob/main/images/Screenshot%20(320).png)
If you can see the image above, it means you did it well.
Congratulations!!!.



