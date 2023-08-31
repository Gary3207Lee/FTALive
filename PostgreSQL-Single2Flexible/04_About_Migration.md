# Single to Flexible Migration

## Migration Journey

![Migration Journey](./img/04_01_Migration_Journey.png)

### 1. Assessment

- Assess Applications
- Assess Databases
  * [Review Migration Assessment Questions with Customer](01_Assess/PremigrationQuestions.md)
  * [Migration Assessment Helper Queries](01_Assess/Queries.sql)

- Feature Mapping
  * [Single Server Extensions](https://learn.microsoft.com/en-us/azure/postgresql/single-server/concepts-extensions)
  * [Flexible Server Extensions](https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/concepts-extensions)

- Check Business/Service Condition

### 2. Planning

- Migration Schedule
- Online vs Offline
  * [Offline/Online Pros & Cons](https://learn.microsoft.com/en-us/azure/postgresql/migrate/concepts-single-to-flexible#how-to-migrate-from-single-server-to-flexible-server)

- Migration Method
- Capacity Planning
- Target Spec Sizing
- Build Test Scenario for Post-Migration (DEV/UAT)
- Build Check Scenario for Post-Migration (PROD)
- Backup & Recovery (RTO/RPO)
- High Availability & BCDR

### 3. Rehersal

- Rehearsal Migration in DEV/UAT Environment </br>
  (Capture Resource Utilization and Elapsed Time)
- Run Test Scenario for Post-Migration (DEV/UAT)
- Review the result and take necessary actions
- Apply additional WBS to Migration Plan if required

### 4. Execution

- Execure Migration including Production Environment

### 5. Cut-Over

- System Cut-Over

### 6. Post-Migration

- Run System Check Scenario for Post-Migration(PROD)
- Monitoring & Remedation
