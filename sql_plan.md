### Generate Plan for an Sql_ID
## Added by Abhishek

  To use the DISPLAY_CURSOR functionality, the calling
  user must have SELECT privilege on V$SQL_PLAN_STATISTICS_ALL,
  V$SQL, and V$SQL_PLAN, otherwise it will show an appropriate
  error message.

      select * from table(dbms_xplan.display_cursor(<sql_id>, <cursor_child_no>, <format>));

      Example:: 
  
      select * from table(dbms_xplan.display_cursor('47nmzprwd0jvf',0,'TYPICAL'));

### Generate Plan for a SQL_ID and Plan_hash_value

  To use the DISPLAY_AWR functionality, the calling
  user must have  SELECT prvilege on DBA_HIST_SQL_PLAN and
  DBA_HIST_SQLTEXT
  
        select * from table(dbms_xplan.display_awr(<Sql_ID>,<plan_hash_value>,<db_id>,<format>));
	
        Example::
  
        select * from table(dbms_xplan.display_awr('arhxdcpuw0xr1',1389270680));

#### Format:
          Determines what information stored in the plan will be
          shown. The format string can use the following predefined
          three formats, each representing a common use case:
  
          'BASIC':   Display only the minimum set of information, i.e. the
                     operation id, the operation name and its option
  
          'TYPICAL': This is the default. Display most information
                     of the explain plan (operation id, name and option,
                     #rows, #bytes and optimizer cost). Pruning,
                     parallel and predicate information are only
                     displayed when applicable. Excludes only PROJECTION,
                     ALIAS and REMOTE SQL information (see below).
  
          'ALL':     Maximum user level, like typical with additional
                     informations (PROJECTION, ALIAS and information about
                     REMOTE SQL if the operation is distributed).
 

          For finer control on the display output, the following keywords
          can be added to the above three standard format to customize their
          default behavior. Each keyword either represents a logical group
          of plan table columns (e.g. PARTITION) or logical additions to the
          base plan table output (e.g. PREDICATE). Format keywords must
          be separated by either a comma or a space:
  
          ROWS: if relevant, shows number of rows estimated by the optimizer
  
          BYTES: if relevant, shows number of bytes estimated by the
                 optimizer
  
          COST: if relevant, shows optimizer cost information
  
          PARTITION: If relevant, shows partition pruning information
  
          PARALLEL: If relevant, shows PX information (distribution method
                    and table queue information)
  
          PREDICATE: If relevant, shows the predicate section
  
          PROJECTION: If relevant, shows the projection section
  
          ALIAS: If relevant, shows the "Query Block Name / Object Alias"
                 section
  
          REMOTE: If relevant, shows the information for distributed query
                  (e.g. remote from serial distribution and remote SQL)
  
          NOTE: If relevant, shows the note section of the explain plan.
  
        Format keywords can be prefixed by the sign '-' to exclude the
        specified information. For example, '-PROJECTION' exclude
        projection information.
  
  	

