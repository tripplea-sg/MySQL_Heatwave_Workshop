# Heatwave for Oracle Analytics Cloud (OAC)
## 1. Create policy
```
a. Go to OCI Dashboard
b. Click Identity > Policies
c. Click button "Create Policy"
d. Enter Policy Name and description: oac-policy
e. Click "Customize (Advanced)" (see on the right)
f. On policy builder, copy and paste the following policies:

allow group Administrators to manage analytics-instances in tenancy
allow group Administrators to manage analytics-instances in compartment ManagedCompartmentForPaaS

<see below example>
```
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-7/Screenshot%202021-02-18%20at%2010.20.56%20PM.png)
## 2. Provision Oracle Analytics Cloud (OAC)
```
a. on OCI Dashboard menu
b. Click Analytics > Analytics Cloud
c. Click button "Create Instance"
d. Enter instance name = "workshop"
e. Click button "Create"
```
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-7/Screenshot%202021-02-18%20at%208.47.45%20PM.png)
Wait until OAC instance is running.
## 3. Access OAC istance
There are 3 ways to access MDS/Heatwave from OAC:
1. Load Balancer IP address
2. Remote Data Gateway (RDG)
3. Private Access Connect (PAC) - Recommended
Click the OAC instance to go to the following form.
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-7/Screenshot%202021-02-18%20at%2010.45.17%20PM.png)
Click the URL to open the Analytics Cloud
## 4. Create Connection
- Click button "Create" on the top right.
- Click Connection and select MySQL
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-7/Screenshot%202021-02-18%20at%2011.15.43%20PM.png)
- Enter 
```
Connection Name = covid19
Host = Load Balancer IP
Port = 3306
Database Name = covid19
Username = test
Password = Manager@123
```
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-7/Screenshot%202021-02-18%20at%2011.15.59%20PM.png)
## 5. Create and Select Data Source
- Click button "Create" on the top right.
- Click "Data Set" and select "covid19"
- Add the following SQL
```
select p.id, p.age, p.gender, p.admin2, v.province, v.countryname, c.continental, p.acquireddate, p.recovereddate, p.passawaydate from covid19.patient p, covid19.province v, covid19.country c where p.provinceid=v.id and v.countryid=c.id;
```
- Give it a name and make sure you select Live in the Data Access field
- You are now ready to build the dashboard You can pick and choose columns from the left pane to build your dashboard

