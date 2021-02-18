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
\q
```
Note:
1. "backup": the location where backup file will be stored
2. "dryRun" (true | false): false - do actual backup, true - check if the backup can be successfully executed
3. "ocimds" (true | false): true - for import to MDS to ensure compatibility, false - does not check this
4. "compatibility: strip_definers": Remove the DEFINER clause from views, routines, events, and triggers
5. "compatibility: strip_restricted_grants": Remove specific privileges that are restricted by MDS
6. "consistent" (true | false): true - consistency on InnoDB is guarranted
## B. Restore your local database to MDS using MySQL Shell
Connect to your MDS using MySQL Shell thru load balancer public IP Address
```
mysqlsh system@<load-balancer-ip-address>:3306
```
Check GTID status
```
 MySQL  193.122.72.139:3306 ssl  JS > \sql show variables like 'gtid%';
+----------------------------------+--------------------------------------------+
| Variable_name                    | Value                                      |
+----------------------------------+--------------------------------------------+
| gtid_executed                    | 2126f055-5fb0-11eb-bf31-02001700c24f:1-234 |
| gtid_executed_compression_period | 0                                          |
| gtid_mode                        | ON                                         |
| gtid_next                        | AUTOMATIC                                  |
| gtid_owned                       |                                            |
| gtid_purged                      | 2126f055-5fb0-11eb-bf31-02001700c24f:1-234 |
+----------------------------------+--------------------------------------------+
6 rows in set (0.2549 sec)
 MySQL  193.122.72.139:3306 ssl  JS > 
```
Restore database from backup:
```
MySQL  193.122.72.139:3306 ssl  JS > util.loadDump('backup',{dryRun:false,updateGtidSet:'append'})
```
Note:
1. "backup": is the backup file location on local machine
2. updateGtidSet (off | append | replace): is parameter to apply GTID set from source.
- off (default): not apply GTID set
- append: if GTID set is not superset 
- replace: if GTID set is superset 
## C. Using Replication to Replicate Delta transaction to achieve a minimum migration downtime
Check GTID status
```
MySQL  193.122.72.139:3306 ssl  JS > \sql show variables like 'gtid%';
+----------------------------------+---------------------------------------------------------------------------------------+
| Variable_name                    | Value                                                                                 |
+----------------------------------+---------------------------------------------------------------------------------------+
| gtid_executed                    | 1b6a39d4-71b3-11eb-81ca-23285f67e33f:1-55,
2126f055-5fb0-11eb-bf31-02001700c24f:1-327 |
| gtid_executed_compression_period | 0                                                                                     |
| gtid_mode                        | ON                                                                                    |
| gtid_next                        | AUTOMATIC                                                                             |
| gtid_owned                       |                                                                                       |
| gtid_purged                      | 1b6a39d4-71b3-11eb-81ca-23285f67e33f:1-55,
2126f055-5fb0-11eb-bf31-02001700c24f:1-234 |
+----------------------------------+---------------------------------------------------------------------------------------+
```
The GTID set is a subset of GTID set on source database, thus replication can be created between source and MDS to replicate all delta transactions until cutover.
 

