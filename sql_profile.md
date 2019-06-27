### SQL Profile

1. Get Profile Outline

```    
    SELECT hint as outline_hints 
    FROM (SELECT p.name, p.signature, p.category, row_number() 
         over (partition by sd.signature, sd.category order by sd.signature) row_num, 
         extractValue(value(t), '/hint') hint 
    FROM sys.sqlobj$data sd, dba_sql_profiles p, 
        table(xmlsequence(extract(xmltype(sd.comp_data), '/outline_data/hint'))) t 
    WHERE sd.obj_type = 1 
    AND p.signature = sd.signature 
    AND p.category = sd.category 
    AND p.name like (<PROFILE_NAME>)) 
    ORDER BY row_num; 
```

2. Get query Outline

```
    SELECT extractvalue(VALUE(d), '/hint') AS outline_hints
    FROM xmltable('/*/outline_data/hint'
    passing (SELECT xmltype(other_xml) AS xmlval
            FROM dba_hist_sql_plan WHERE sql_id = <SQLID> AND plan_hash_value = <PLAN_HASH_VALUE>
            AND other_xml IS NOT NULL)
    ) d
```

3. Create New SQL Profile

```
declare
     l_sql clob;
     l_prefix varchar2(30) := 'coe';

begin
       begin
        select sql_fulltext 
        into l_sql
        from gv$sqlarea 
        where sql_id= <SQLID>
        and rownum=1;

       exception
        when NO_DATA_FOUND then 
         select sql_text 
         into l_sql
         from dba_hist_sqltext
         where sql_id= <SQLID>;
       end;


     dbms_sqltune.import_sql_profile( sql_text => l_sql, 
                                     name => l_prefix||'_'||'<SQLID>_<PLAN_HASH_VALUE>',
                                     profile => sqlprof_attr(
                q'!IGNORE_OPTIM_EMBEDDED_HINTS!',
                q'!OPTIMIZER_FEATURES_ENABLE('11.2.0.3')!',
                q'!DB_VERSION('11.2.0.3')!',
                q'!OPT_PARAM('optimizer_dynamic_sampling' 5)!',
                q'!ALL_ROWS!',
                q'!NO_PARALLEL!',
                q'!OUTLINE_LEAF(@"SEL$683B0107")!',
                q'!OUTLINE_LEAF(@"SEL$5DA710D3")!',                
                q'!USE_HASH_AGGREGATION(@"SEL$683B0107")!'
             ),
             force_match => false );
end;
```
