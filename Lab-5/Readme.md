# Working with MySQL Heatwave
Extract data.zip using "unzip" to local drive and perform the following

1. Load Dump to MySQL Heatwave
Login to database using MySQL Shell
```
mysqlsh system@<your_load_balancer_public_ip>:3306
```
On MySQL Shell
```
mysqlsh > util.loadDump('data', {dryRun:true})
mysqlsh > util.loadDump('data', {dryRun:false})
```

2. Switch MySQL Shell's session to SQL
```
mysqlsh > \sql
```

3. Query performance without Heatwave
Using the same session
```
mysqlsh > select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```
Notice the execution time is around 45 seconds
```
mysqlsh > explain select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental; 
```
Notice that execution plan is not optimized (big execution cost)

4. Enable Heatwave on tables used by the query
Heatwave does not support VIRTUAL column, so we need to exclude this column from table patient
```
mysqlsh > alter table patient modify md5lname varchar(300) GENERATED ALWAYS AS (md5(`lastName`)) VIRTUAL not secondary;
```
Use heatwave on table patient
```
mysqlsh > alter table covid19.patient secondary_engine=rapid;
```
Use heatwave on table country
```
mysqlsh > alter table covid19.country secondary_engine=rapid;
```
Use heatwave on table province
```
mysqlsh > alter table covid19.province secondary_engine=rapid;
```

5. Load data from InnoDB based tables to Heatwave
Load table province to Heatwave
```
mysqlsh > alter table covid19.province secondary_load;
```
Load table country to Heatwave
```
mysqlsh > alter table covid19.country secondary_load;
```
Load table patient to Heatwave
```
mysqlsh > alter table covid19.patient secondary_load;
```
Check table status from performance schema 
```
mysqlsh > SELECT NAME, LOAD_STATUS, LOAD_PROGRESS, POOL_TYPE FROM performance_schema.rpd_tables,performance_schema.rpd_table_id WHERE rpd_tables.ID = rpd_table_id.ID;
```

6. Check query performance with Heatwave
Use the same query, notice that execution time drop to 0.5 seconds
```
mysqlsh > select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```
Check the change on execution plan, notice that RAPID engine is involved
```
mysqlsh > explain select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```

7. How to turn-off heatwave at session level  
```
mysqlsh > set use_secondary_engine=OFF;
```
Now, with RAPID engine is set to OFF, running the same query will need 45 seconds again to complete
```
mysqlsh > select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```
The query execution plan is also changed back to the original plan
```
mysqlsh > explain select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```
8. How to turn-on heatwave at session level
```
mysqlsh > set use_secondary_engine=ON;
```
Check the query, notice that execution time is 0.5 seconds
```
mysqlsh > select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```
Check the execution plan, notice that RAPID storage engine is involved
```
mysqlsh > explain select count(*), continental from country c, patient pt, province p where p.countryid=c.id and pt.provinceid=p.id group by continental order by continental;
```
9. Ensure change propagation is ENABLED
```
mysqlsh > SELECT VARIABLE_VALUE FROM performance_schema.global_status WHERE VARIABLE_NAME = 'rapid_change_propagation_status';
```
10. Add rows to table Treatment
```
mysqlsh > CALL covid19.addTreatment(0, 1000);
```
11. Best Practices
```
https://dev.mysql.com/doc/heatwave/en/heatwave-best-practices.html
```
