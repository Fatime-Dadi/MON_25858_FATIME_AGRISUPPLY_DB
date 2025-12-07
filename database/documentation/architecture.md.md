# System Architecture
## Agricultural Census Data Management System

## Architecture Overview
The system follows a **three-tier architecture**:
1. **Presentation Tier**: SQL Developer, Web Interface, Mobile Apps
2. **Application Tier**: PL/SQL stored procedures, functions, packages
3. **Data Tier**: Oracle 21c Database with PDB architecture

## Database Architecture
### Physical Architecture
- **CDB**: Container Database (Root container)
- **PDB**: `MON_25858_FATIME_AGRISUPPLY_DB` (Pluggable Database)
- **Tablespaces**:
  - `AGRI_DATA`: Primary data storage (50MB, autoextend)
  - `AGRI_IDX`: Index storage (25MB, autoextend)
  - `AGRI_TEMP`: Temporary operations (50MB, autoextend)

### Memory Configuration
- **SGA_TARGET**: 1GB
- **PGA_AGGREGATE_TARGET**: 500MB
- **Shared Pool**: 300MB
- **Buffer Cache**: 600MB

## Security Architecture
### User Management
- **Admin User**: `FATIME_ADMIN` (DBA privileges)
- **Application Users**: Role-based access
- **Audit Users**: Read-only access to audit logs

### Security Features
1. **Authentication**: Password-based with complexity rules
2. **Authorization**: Role-based access control (RBAC)
3. **Auditing**: Comprehensive audit trail for all DML operations
4. **Encryption**: Data at rest encryption (TDE planned)

## Performance Architecture
### Indexing Strategy
- **Primary Keys**: Automatic index creation
- **Foreign Keys**: Indexed for join performance
- **Frequently Queried Columns**: Additional indexes
- **Composite Indexes**: For common query patterns

### Partitioning Strategy
- **Range Partitioning**: By YEAR for time-based data
- **List Partitioning**: By COUNTY_ID for geographic data
- **Composite Partitioning**: Range-List combination

## Backup and Recovery
### Backup Strategy
- **Daily Incremental**: Changed data blocks
- **Weekly Full**: Complete database backup
- **Archive Logs**: Continuous archiving enabled

### Recovery Objectives
- **RPO**: 15 minutes (Maximum data loss)
- **RTO**: 30 minutes (Maximum downtime)

## Scalability Design
### Vertical Scaling
- Memory increase up to 4GB
- CPU allocation based on workload
- Storage auto-extend enabled

### Horizontal Scaling (Future)
- Read replicas for reporting
- Data Guard for high availability
- Sharding for geographic distribution

## Integration Architecture
### Data Sources
1. Farmer registration systems
2. Mobile data collection apps
3. IoT sensors (temperature, humidity)
4. Market price APIs
5. Weather data feeds

### Export Interfaces
1. CSV export for Excel analysis
2. JSON API for web applications
3. PDF reports for printing
4. XML for government systems

## Monitoring Architecture
### Performance Monitoring
- **Active Session History (ASH)**
- **Automatic Workload Repository (AWR)**
- **Real-time SQL monitoring**
- **Tablespace usage alerts**

### Health Monitoring
- **Database availability**: Ping tests
- **Backup status**: Verification alerts
- **Security alerts**: Failed login attempts
- **Capacity alerts**: 80% threshold warnings