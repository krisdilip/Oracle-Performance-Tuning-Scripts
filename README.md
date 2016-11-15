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
     
  3) Grantees of AWR_USER <br>
    ```GRANT AWR_USER TO <username>; ```		
		
  4) Check the role details <br>
     ```SELECT * from ROLE_TAB_PRIVS where role = 'AWR_USER'```   
	  		
|GRANTEE  | OWNER  | TABLE_NAME                 | PRIVILEGE |
|---------| -------| ---------------------------| ----------|
|AWR_USER | SYS    | DBA_HIST_DATABASE_INSTANCE | SELECT    |
|AWR_USER | SYS    | DBA_HIST_SNAPSHOT          | SELECT    |
|AWR_USER | SYS    | DBMS_WORKLOAD_REPOSITORY   | EXECUTE   |
|AWR_USER | SYS    | V_$DATABASE                | SELECT    |
|AWR_USER | SYS    | V_$INSTANCE                | SELECT    |

   5) Generate **AWR** Report
        
        a) Switch to the required schema (Optional if you have logged into the correct schema) 
     ALTER SESSION SET CURRENT_SCHEMA = <USERNAME>;
	   
        b) Set role AWR_USER
             SET ROLE AWR_USER;

        c) Find the snap and Database details
             SELECT DBID, INSTANCE_NUMBER, SNAP_ID, BEGIN_INTERVAL_TIME, END_INTERVAL_TIME
	     FROM DBA_HIST_SNAPSHOT ORDER BY BEGIN_INTERVAL_TIME DESC;

        d) Generate the AWR report in HTML format for the above selected snap
	     SELECT * 
	     FROM TABLE(DBMS_WORKLOAD_REPOSITORY.AWR_REPORT_HTML(<DBID>, <INSTANCE_NUMBER>, <SNAP_ID FROM>, <SNAP_ID TO>));
