## Create Materialized view


##Create source table for the example
```sql
create or replace table test_db.public.lineitem
as 
    select * 
    from "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF1"."LINEITEM";
    
```
##Create Aggregated Materialized view

```sql
create or replace MATERIALIZED VIEW lineitem_by_L_SHIPMODE 
    (SHIPDATE,SHIPMODE,quantity) cluster by (SHIPDATE)
as 
    select L_SHIPDATE,L_SHIPMODE,sum(L_QUANTITY)
    from test_db.public.lineitem
    group by L_SHIPDATE,L_SHIPMODE;
```
    
## check MV:
```sql
select * 
from lineitem_by_L_SHIPMODE 
where  SHIPDATE='1996-05-02' and  SHIPMODE='MAIL';
```

##delete from source table
```sql
delete from  test_db.public.lineitem
where L_SHIPDATE='1996-05-02' and  L_SHIPMODE='MAIL';
```

## check MV:
```sql
select * 
from lineitem_by_L_SHIPMODE 
where  SHIPDATE='1996-05-02' and  SHIPMODE='MAIL';   
```