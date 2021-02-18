# Preparation
## A. Download Installer and Data Sets
1. Download and install MySQL Server 8.0.23 to your laptop (skip download if you already have MySQL 8.0.23 on your laptop)
```
Download the MySQL Server 8.0.23 installer from https://dev.mysql.com/downloads/mysql/
```
2. Download and install MySQL Shell to your laptop (skip the following steps if you already have MySQL Shell)
```
Download the MySQL Shell 8.0.23 from https://dev.mysql.com/downloads/shell/

Install MySQL Shell 8.0.23 to your laptop.
Follow this URL: https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-install-windows-quick.html
```
3. Download Data Sets from Server
```
a. Get Server's IP Address, username, and password from the instructor
b. Connect to Server using scp or windows scp
c. Download 3 files: sakila-schema.sql, sakila-data.sql, and data.zip

e.g. command in Linux: scp ftpuser@193.122.75.56:/home/ftpuser/* .
```
## B. Install Database Server 8.0.23 if you have not install before
Run the installer
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-0/Screenshot%202021-02-18%20at%202.30.39%20PM.png)
Click button "continue"
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-0/Screenshot%202021-02-18%20at%202.30.50%20PM.png)
On license page, click button "continue"
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-0/Screenshot%202021-02-18%20at%202.31.07%20PM.png)
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-0/Screenshot%202021-02-18%20at%202.32.16%20PM.png)
On Configuration page, choose "Use Strong Password Encryption" and click button "Next"
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-0/Screenshot%202021-02-18%20at%202.32.30%20PM.png)
Key-in to-be "root password" as "Manager@123", enable checkbox "Start MySQL Instance once finished", and click button "Finish"
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-0/Screenshot%202021-02-18%20at%202.32.57%20PM.png)
Ensure database with port 3306 is running.
```
1. Open terminal
2. Login to MySQL

mysql -uroot -p"Manager@123"

3. Show databases

mysql > show databases;
mysql > exit;

```
## C. Change GTID mode and server_id of your local database
Login to local mysql
```
mysql -uroot -p"Manager@123"
```
Change server_id value
```
set persist server_id=100;
```
Change GTID_MODE to ON
```
set persist gtid_mode='OFF_PERMISSIVE';
set persist gtid_mode='ON_PERMISSIVE';
set persist enforce_gtid_consistency='ON';
set persist gtid_mode='ON';
exit;
```

## D. Prepare the Data Set
1. Restore sakila schema and data to your local database
```
mysql -uroot -p"Manager@123" < sakila-schema.sql
mysql -uroot -p"Manager@123" < sakila-data.sql
```
2. Check if sakila schema is already inside your local database
```
mysql -uroot -p"Manager@123" -e "show databases"
```
3. Unzip data set
```
unzip data.zip
```
