
### Policies

The function must have the following behavior:
It must take as arguments a schema name and an object (table, view, or synonym) name as inputs. Define input


1. Create Policy Function
```
CREATE OR REPLACE FUNCTION hide_sal_comm (
 v_schema IN VARCHAR2, 
 v_objname IN VARCHAR2)

RETURN VARCHAR2 AS
con VARCHAR2 (200);

BEGIN
 con := 'deptno=30';
 RETURN (con);
END hide_sal_comm;
```

2. Add Policy to Table

```
BEGIN
 DBMS_RLS.ADD_POLICY (
  object_schema     => 'SCOTT', 
  object_name       => 'EMP',
  policy_name       => 'hide_sal_policy', 
  policy_function   => 'hide_sal_comm' 
  );
END; 
```

3. Drop Policy

```
BEGIN 
    dbms_rls.drop_policy('SCOTT', 
        'EMP', 
        'hide_sal_policy' 
        );
END; 
```
