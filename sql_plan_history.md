### SQL Plan history

To find the historical plan hash values and average execution time of an sql_id

        select ss.snap_id, ss.instance_number node, begin_interval_time, sql_id, plan_hash_value,
               nvl(executions_delta,0) execs,
               (elapsed_time_delta/decode(nvl(executions_delta,0),0,1,executions_delta))/1000000 avg_etime,
               (buffer_gets_delta/decode(nvl(buffer_gets_delta,0),0,1,executions_delta)) avg_lio
        from DBA_HIST_SQLSTAT S, DBA_HIST_SNAPSHOT SS
        where sql_id = <Sql_ID>
        and ss.snap_id = S.snap_id
        and ss.instance_number = S.instance_number
        and executions_delta > 0
        order by 1, 2, 3
