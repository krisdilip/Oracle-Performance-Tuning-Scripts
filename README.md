# Oracle-Performance-Tuning-Scripts

### AWR - Report generation
  1) Create a new role and give the following grants to the role <br>     
     ```CREATE ROLE AWR_USER NOT IDENTIFIED;```
     
  2) Object privileges granted to AWR_USER <br>
     ```GRANT SELECT ON DBA_HIST_DATABASE_INSTANCE TO AWR_USER;```          
     ```GRANT SELECT ON DBA_HIST_SNAPSHOT TO AWR_USER; ```     
     ```GRANT EXECUTE ON DBMS_WORKLOAD_REPOSITORY TO AWR_USER;```     
     ```GRANT SELECT ON V_$DATABASE TO AWR_USER;```     
     ```GRANT SELECT ON V_$INSTANCE TO AWR_USER; ```
     
  3) Grantees of AWR_USER  
     ```GRANT AWR_USER TO <username>;```
     
