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
Ways to connect to MDS:
1. Site to site VPN from local network to VCN (https://blogs.oracle.com/mysql/migrate-from-on-premise-mysql-to-mysql-database-service)
2. SSH tunelling using private key to bastion host (https://lefred.be/content/testing-mysql-database-service-without-vpn-part-2/)
3. Public facing MySQL Router on bastion host (not recommended, complicated setting, very unsecure)
4. Load balancer (not recommended, very easy and stright forward setting, very unsecure)
## How to quickly connect to MDS/Heatwave from your laptop using Load Balancer
Do not use this for real environment because it's unsafe. We use this to simplify this workshop material.
1. Create Internet Gateway and Routing 
- On OCI dashboard, choose Networking > Virtual Cloud Network
- Choose your VCN, check if Internet Gateway and Routing are exist
- If internet Gateway isn't exist, create one. On VCN page, click: Internet Gateway > Create
- If Routing table isn't exist, create one. On VCN page, click: Route Tables > Create Route Tables
- On VCN page, check your routing tables. It should have a row with "Target Type" = "internet gateway". If not, then click "Add Route Rules" > Choose "Internet Gateway for "Target Type" > Use "0.0.0.0/0" as Destination CIDR Block 
- On VCN page, check your security list (click on "Resources" menu on the left side). 
  - Click the default security list. 
  - Check the ingress rule, add ingress rule
  - It should allow port 3306 from all sources to all destinations. To simplify this workshop and eliminate issue, let's put aside security by allowing all ports.  from all sources to all destinations (Set source 0.0.0.0/0, port: All on both source and destination). See below picture:
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-02-24%20at%2012.05.43%20PM.png)
  - Click "Add Ingress Rule"
2. Create Load Balancer for your MDS/Heatwave
- On OCI dashboard, choose Networking > Load Balancers > Create Load Balancer
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%207.52.54%20AM.png)
Set minimum bandwidth and maximum bandwidth, and click Next
- On choose backend, set as follow and click Next
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%207.52.54%20AM.png)
- On Configure Listener, set as follow and click Next
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%208.08.18%20AM.png)
Done! We have 1 Load Balancer.
3. Link this load balancer to MDS / Heatwave instance
- Go to your load balancer, click "Bucket Sets" > click the bucket set you see on the page
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%208.16.39%20AM.png)
- Click: Backends > "Add Backends"
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%208.18.52%20AM.png)
- Set IP Address as internal IP Address of the MDS, and Port is the MySQL Port number
- Click "Add" and just wait until new backend is registered
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%208.30.08%20AM.png)
Now we can connect to MDS / Heatwave from our laptop
1. On Load Balancer Details page, see Public IP address
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-3/Screenshot%202021-01-26%20at%208.30.34%20AM.png)
2. On your local machine, connect to MDS via mysql CLI
```
$ mysql -u<user> -p<password> -h<Load_balancer_public_IP_address>
```
3. On your local, machine, connect to MDS via mysql shell
```
$ mysqlsh <user>@<Load_balancer_public_IP_address>:<Load_balancer_port>
```
