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
Note:
1. "backup": the location where backup file will be stored
2. "dryRun" (true | false): false - do actual backup, true - check if the backup can be successfully executed
3. "ocimds" (true | false): true - for import to MDS to ensure compatibility, false - does not check this
4. "compatibility: strip_definers": Remove the DEFINER clause from views, routines, events, and triggers
5. "compatibility: strip_restricted_grants": Remove specific privileges that are restricted by MDS
6. "consistent" (true | false): true - consistency on InnoDB is guarranted
## B. Restore your local database to MDS using MySQL Shell





Obtain data.zip from the trainer, extract data.zip to local drive and perform the following

