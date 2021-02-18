# Heatwave for Oracle Analytics Cloud (OAC)
1. Create policy
```
a. Go to OCI Dashboard
b. Click Identity > Policies
c. Click button "Create Policy"
d. Enter Policy Name: oac-policy
e. Click "Customize (Advanced)" (see on the right)
f. On policy builder, copy and paste the following policies:

allow group Administrator-Group to manage analytics-instances in tenancy
allow group Administrator-Group to manage analytics-instances in compartment <your compartment name>

<see below example>
```

