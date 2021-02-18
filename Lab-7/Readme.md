# Heatwave for Oracle Analytics Cloud (OAC)
1. Create policy
```
a. Go to OCI Dashboard
b. Click Identity > Policies
c. Click button "Create Policy"
d. Enter Policy Name and description: oac-policy
e. Click "Customize (Advanced)" (see on the right)
f. On policy builder, copy and paste the following policies:

allow group <your group> to manage analytics-instances in tenancy
allow group <your group> to manage analytics-instances in compartment <your compartment name>

<see below example>
```
![Image of picture1](https://github.com/tripplea-sg/MySQL_Heatwave_Workshop/blob/main/Lab-7/Screenshot%202021-02-18%20at%208.42.09%20PM.png)
2. Provision Oracle Analytics Cloud (OAC)
```
a. on OCI Dashboard menu
b. Click Analytics > Analytics Cloud
c. Click button "Create Instance"
d. Enter instance name = "workshop"
e. Click button "Create"
```


