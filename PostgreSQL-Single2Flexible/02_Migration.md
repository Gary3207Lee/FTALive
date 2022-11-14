# Migration Method and Tips

## Migration Method
### 1. Verify feature usage
- Check for AAD, CMK, RR usage (Public preview features)
- Check for extensions usage
### 2. Plan migration
- DB sizes
- target versions
- offline or online
- batch
- target databases
### 3. Provision target Flexible Server
- Suitable SKU, Storage, Networking
- SLA considerations (HA, Zone)
- Backup scenario
### 4. Migrate & Cutover
- Choose right migration method
- Reduce source workload
- Cutover when pending byte=0
### 5. Test
- Verify data
- Validate your applications
- Validate performance
- Validate extensions
### 6. Cleanup
- Delete source and migrate AAD application after confirming the target is all good

## [Migration tool - Azure Database for PostgreSQL Single Server to Flexible Server (preview)](https://learn.microsoft.com/en-us/azure/postgresql/migrate/concepts-single-to-flexible)

## Current Restrictions for Migration Tool
### Logical replication
- DDLs not replicated
- Needs primary keys for merge replication
- Unlogged tables not handled
- Large objects not replicated

### Server restart
- Source database server may require a restart if WAL_LEVEL is changed to LOGICAL 

### Parallel runs
- Up to eight databases per server per migration can be run in parallel
- Data sizes less than 1 TB only supported currently 

1) Depending on the size of data to be migrated, the migration time could vary from a few minutes to a few hours.
2) Do not run any write workloads on your source server during the time of migration. It is recommended to put your source server in a read-only mode.
3) Recommended to use only for individual DBs of size <= 1 TB.
4) You can migrate up to 8 databases from a single server to flexible server in a single migration. If you need to migrate more databases, please create multiple migrations between the same source and target.

We have a plan to release new tool covers over 1TB capacity, soon.


