# Working with MySQL Database Service (MDS)

1.	On the Navigation Menu, under Database, select MySQL -> DB Systems. 
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.39.07%20PM.png)
</br>
2.	On DB Systems in <Compartment Name> Compartment, click on Create MySQL DB System. 

![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.39.46%20PM.png)
</br>
3.	On Create MySQL DB System, under DB System Information, select a Compartment, enter a Name for the DB System, add a Description, select an Availability Domain, select a Fault Domain, select a configuration for the MySQL Shape, and click Next. 
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.40.32%20PM.png)
</br>
Click shape and choose MySQL.Heatwave.VM.Standard.E3
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%207.34.37%20AM.png)
</br>
4.	On Create MySQL DB System, under Database Information, create the Administrator Credentials by entering Username and Password, specify the network information selecting the Virtual Cloud Network and Subnet in the compartment and entering Hosting Name, and click Next.
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.48.33%20PM.png)
</br>
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.49.07%20PM.png)
</br>
5.	On Backup Information, select Enable Automatic Backups, select the Backup Retention Period, select Default Backup Window, and click on Create.
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.49.19%20PM.png)
</br>
6.	The New MySQL DB System will be ready to use after a few minutes. The state will be shown as Creating during the creation.  
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.49.44%20PM.png)
</br>
7.	The state Active indicates that the DB System is ready to use. Check the MySQL endpoint (Address) under Instances in the MySQL DB System Details page.
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%202.50.18%20PM.png)
</br>
</br>
</br>
## Open Connection (NOT INCLUDED IN THE VIDEO)
1. Go to OCI Dashboard and Click Networking >> Virtual Cloud Network
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%205.37.24%20PM.png)
</br>
2. Click your VCN below (sample: workshop-vcn)

![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%206.59.52%20PM.png)
</br>
3. Click Private Subnet 
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%206.59.52%20PM.png)
</br>
4. Click the Security List
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%207.10.07%20PM.png)
</br>
5. Add Ingress Rule (source: 0.0.0.0/24, TCP, All ... Destination: port 3306)
![Image of picture1](https://github.com/tripplea-sg/Cloud_Administration_Workshop/blob/main/Lab-7/Screenshot%202020-11-13%20at%207.13.17%20PM.png)
</br>
## Connect to your MDS from VM
```
mysqlsh {your_user}@{MDS_Internal_IP}:3306
```
Exit from Shell
```
\q
```
## Setup Site-to-Site VPN from On Premise
https://blogs.oracle.com/mysql/migrate-from-on-premise-mysql-to-mysql-database-service
## Without VPN
https://lefred.be/content/testing-mysql-database-service-without-vpn/


  

