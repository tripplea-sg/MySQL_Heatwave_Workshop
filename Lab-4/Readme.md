# Migrate your local database to MDS
## A. Backup your local database using MySQL Shell 
1. Create folder on your local called "backup" (e.g. D:\backup)
2. Backup your database using the following command </br>
Login to local database using mysql shell
```
mysqlsh root@localhost:3306
```
Enter password as "Manager@123" and "Y" to store password
```
Please provide the password for 'root@localhost:3306': 
Save password for 'root@localhost:3306'? [Y]es/[N]o/Ne[v]er (default No): Y
```
On mysql shell, backup database using the following command:
```
util.dumpInstance("backup", {dryRun: false, ocimds: true, compatibility: ["strip_definers", "strip_restricted_grants"], consistent:true})
```
## B. Restore your local database to MDS using MySQL Shell





Obtain data.zip from the trainer, extract data.zip to local drive and perform the following

