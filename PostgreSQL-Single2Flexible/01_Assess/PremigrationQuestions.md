### Initial Questions
* What is the configuration of the source and target?
* How many databases exist on the source server and what is the size of each databases?

    ```sql
    SELECT pg_size_pretty( pg_database_size('postgres') );
    ```
* Are you planning to upgrade versions during the migration?
* How many tables exist in each database and what is the size of each table?

    ```sql
    SELECT n.nspname as "Schema",
           c.relname as "Name",
      CASE c.relkind 
              WHEN 'r' THEN 'table' 
              WHEN 'v' THEN 'view' 
              WHEN 'm' THEN 'materialized view' 
              WHEN 'i' THEN 'index' 
              WHEN 'S' THEN 'sequence' 
              WHEN 's' THEN 'special' 
              WHEN 'f' THEN 'foreign table' 
              WHEN 'p' THEN 'partitioned table' 
              WHEN 'I' THEN 'partitioned index' 
              END as "Type",
      CASE c.relpersistence 
              WHEN 'p' THEN 'permanent' 
              WHEN 't' THEN 'temporary' 
              WHEN 'u' THEN 'unlogged' 
              END as "Persistence",
            pg_catalog.pg_size_pretty(pg_catalog.pg_table_size(c.oid)) as "Size"
      FROM pg_catalog.pg_class c
      LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
      WHERE c.relkind IN ('r','p','')
         AND n.nspname <> 'pg_catalog'
         AND n.nspname <> 'information_schema'
         AND n.nspname !~ '^pg_toast'
      ORDER BY 1,2;
    ```
* How much down time is acceptable during the migration?
* Are there any schemas other than 'public' in the database(s)?
* What extensions are installed on the source?
    ```sql
    SELECT e.extname AS "Name", 
           e.extversion AS "Version", 
           n.nspname AS "Schema", 
           c.description AS "Description"
    FROM pg_catalog.pg_extension e 
    LEFT JOIN pg_catalog.pg_namespace n ON n.oid = e.extnamespace 
    LEFT JOIN pg_catalog.pg_description c ON c.objoid = e.oid 
      AND c.classoid = 'pg_catalog.pg_extension'::pg_catalog.regclass
    ORDER BY 1;
    ```
* How oftern is vacuuming performed on the source? Is there bloat?
    ```sql
    SELECT schemaname, 
           relname, 
           n_dead_tup, 
           n_live_tup, 
           round(n_dead_tup::float/n_live_tup::float*100) dead_pct ,
           autovacuum_count, 
           last_vacuum, 
           last_autovacuum,
           last_autoanalyze
    FROM pg_stat_user_tables  
    WHERE n_live_tup > 0  
    ORDER BY n_live_tup DESC;
    ```
* Are there any large objects?
    ```sql
    SELECT oid as "ID",
           pg_catalog.pg_get_userbyid(lomowner) as "Owner",
           pg_catalog.obj_description(oid, 'pg_largeobject') as "Description"
    FROM pg_catalog.pg_largeobject_metadata   
    ORDER BY oid;
    ```
* Are there any orphan large objects?
    ```sql
    SELECT oid 
    FROM pg_largeobject_metadata m 
    WHERE not exists (SELECT 1 FROM <name_of_table>  WHERE m.oid=name_of_oid_column);
    ```
* Are there any archive tables with a large size (>1TB)?
* Is the networking configuration public or private?
