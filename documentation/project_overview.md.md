# Agricultural Census Data Management System - Project Overview

## Project Description
The Agricultural Census Data Management System is a government database solution for managing national agricultural census data. This system collects, validates, and analyzes agricultural statistics from all Kenyan counties for policy making and planning.

## Business Objectives
1. Centralize agricultural census data from 47 Kenyan counties
2. Validate data quality with business rules
3. Provide real-time agricultural statistics
4. Enable data-driven government policy decisions
5. Track survey collection periods and data submission deadlines

## Database Scope
The database supports the following core functionalities:

### 1. County Data Management
- Store demographic data for each county
- Track farming vs non-farming households
- Manage sub-county level data

### 2. Crop & Livestock Statistics
- Record crop production by county
- Track livestock populations
- Monitor farming activities (irrigation, fishing, etc.)

### 3. Survey Period Management
- Define official data collection periods
- Enforce data submission deadlines
- Track survey compliance

### 4. Audit & Security
- Comprehensive audit trail of all data changes
- Role-based access control (Farmers, Data Clerks, Admins)
- Business rule enforcement via triggers

## Technical Specifications

### Database Technology
- **DBMS**: Oracle Database 21c
- **Architecture**: Multitenant with PDB
- **Database Name**: MON_25858_FATIME_AGRISUPPLY_DB
- **Character Set**: AL32UTF8

### Performance Requirements
- Response time: < 2 seconds for analytical queries
- Support for 50 concurrent government users
- 24/7 availability during survey periods
- Point-in-time recovery for data integrity

### Security Requirements
- Role-based access control (RBAC)
- Comprehensive audit logging
- Survey period restrictions
- Data validation at multiple levels

## Database Design Principles

### 1. Normalization
- Third normal form (3NF) compliance
- 5 main tables with proper relationships
- Referential integrity enforcement

### 2. Data Integrity
- CHECK constraints for data validation
- Foreign key relationships
- Business rules enforced via triggers

### 3. Auditability
- All data changes logged
- User context captured
- Operation status tracked

## Implementation Status

### Completed Features
1. **Database Creation**: PDB with proper naming convention
2. **Table Structure**: 5 normalized tables with 100+ rows each
3. **PL/SQL Components**: 5 functions, 4 procedures, 2 packages
4. **Advanced Features**: Compound triggers, audit logging, business rules
5. **Security**: User management with role-based restrictions

### Technical Implementation
- **Tables**: COUNTIES, CROPS, LIVESTOCK_TYPES, FARMING_ACTIVITIES, COUNTY_AGRIC_STATS
- **Business Rules**: Survey period restrictions with user-type variations
- **Audit Trail**: Comprehensive logging of all DML operations
- **Validation**: Multiple layers (CHECK constraints, PL/SQL validation)

## Success Metrics
- Data accuracy: 100% constraint validation
- Performance: < 2s query response time
- Security: Comprehensive audit trail
- Compliance: Business rules enforced at database level

## Contact Information
For questions regarding this agricultural census database:
- **Student**: Fatime Dadi Wardougou (ID: 25858)
- **Course**: Database Development with PL/SQL (INSY 8311)
- **Institution**: Adventist University of Central Africa
- **Date**: December 2025