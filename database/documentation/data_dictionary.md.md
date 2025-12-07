# DATA DICTIONARY
## Agricultural Census Data Management System

### 1. DIMENSION TABLES

#### 1.1 COUNTIES
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| COUNTY_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Unique county identifier |
| COUNTY_NAME | VARCHAR2(100) | NOT NULL | County name (e.g., NAIROBI, KILIFI) |
| SUB_COUNTY | VARCHAR2(100) | NULL | Sub-county name |
| TOTAL_HOUSEHOLDS | NUMBER | NOT NULL, CHECK > 0 | Total households in county |
| FARMING_HOUSEHOLDS | NUMBER | NOT NULL, CHECK >= 0 | Households engaged in farming |

#### 1.2 CROPS
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| CROP_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Crop type identifier |
| CROP_NAME | VARCHAR2(50) | UNIQUE, NOT NULL | Crop name (e.g., MAIZE, SORGHUM) |
| CATEGORY | VARCHAR2(30) | NOT NULL | Category (CEREAL, LEGUME, TUBER, VEGETABLE, FRUIT, CASH_CROP) |

#### 1.3 LIVESTOCK_TYPES
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| LIVESTOCK_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Livestock type identifier |
| ANIMAL_NAME | VARCHAR2(50) | UNIQUE, NOT NULL | Animal name (e.g., EXOTIC_CATTLE_DAIRY, SHEEP) |
| CATEGORY | VARCHAR2(30) | NOT NULL | Category (CATTLE, SMALL_RUMINANT, POULTRY, OTHER_LIVESTOCK) |

#### 1.4 FARMING_ACTIVITIES
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| ACTIVITY_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Activity identifier |
| ACTIVITY_NAME | VARCHAR2(50) | UNIQUE, NOT NULL | Activity name (e.g., CROP_PRODUCTION, FISHING) |
| DESCRIPTION | VARCHAR2(200) | NULL | Activity description |

### 2. FACT TABLES

#### 2.1 COUNTY_AGRIC_STATS
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| STAT_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Statistics record identifier |
| COUNTY_ID | NUMBER | FOREIGN KEY REFERENCES COUNTIES(COUNTY_ID) | County reference |
| CROP_ID | NUMBER | FOREIGN KEY REFERENCES CROPS(CROP_ID) | Crop reference (if crop data) |
| LIVESTOCK_ID | NUMBER | FOREIGN KEY REFERENCES LIVESTOCK_TYPES(LIVESTOCK_ID) | Livestock reference (if livestock data) |
| ACTIVITY_ID | NUMBER | FOREIGN KEY REFERENCES FARMING_ACTIVITIES(ACTIVITY_ID) | Activity reference (if activity data) |
| HOUSEHOLDS_COUNT | NUMBER | NOT NULL, CHECK >= 0 | Number of households |
| YEAR | NUMBER | DEFAULT 2019 | Data year |

### 3. AUDIT & SYSTEM TABLES

#### 3.1 SURVEY_COLLECTION_PERIODS
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| PERIOD_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Survey period identifier |
| SURVEY_NAME | VARCHAR2(100) | NOT NULL | Survey name |
| START_DATE | DATE | NOT NULL | Start date of survey period |
| END_DATE | DATE | NOT NULL | End date of survey period |
| STATUS | VARCHAR2(20) | DEFAULT 'ACTIVE', CHECK IN ('ACTIVE','CLOSED','PLANNED') | Current status |
| DESCRIPTION | VARCHAR2(500) | NULL | Description of survey |

#### 3.2 SYSTEM_USERS
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| USER_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | User identifier |
| USERNAME | VARCHAR2(50) | UNIQUE, NOT NULL | Login username |
| USER_TYPE | VARCHAR2(20) | NOT NULL, CHECK IN ('FARMER','EXTENSION_OFFICER','DATA_CLERK','ADMIN') | User role |
| COUNTY_ID | NUMBER | FOREIGN KEY REFERENCES COUNTIES(COUNTY_ID) | Associated county |
| FULL_NAME | VARCHAR2(100) | NULL | User's full name |
| EMAIL | VARCHAR2(100) | NULL | Email address |
| REGISTRATION_DATE | DATE | DEFAULT SYSDATE | Registration date |
| STATUS | VARCHAR2(20) | DEFAULT 'ACTIVE', CHECK IN ('ACTIVE','INACTIVE','SUSPENDED') | Account status |

#### 3.3 AGRIC_SURVEY_AUDIT
| Column | Data Type | Constraint | Description |
|--------|-----------|------------|-------------|
| AUDIT_ID | NUMBER | PRIMARY KEY, GENERATED ALWAYS AS IDENTITY | Audit record identifier |
| AUDIT_TIMESTAMP | TIMESTAMP | DEFAULT SYSTIMESTAMP | When audit occurred |
| USER_ID | NUMBER | FOREIGN KEY REFERENCES SYSTEM_USERS(USER_ID) | User who performed action |
| USERNAME | VARCHAR2(50) | NOT NULL | Username at time of action |
| USER_TYPE | VARCHAR2(20) | NULL | User type at time of action |
| TABLE_NAME | VARCHAR2(100) | NOT NULL | Table affected |
| DML_OPERATION | VARCHAR2(10) | CHECK IN ('INSERT','UPDATE','DELETE') | Type of operation |
| RECORD_ID | VARCHAR2(100) | NULL | Affected record identifier |
| OLD_VALUES | CLOB | NULL | Values before change |
| NEW_VALUES | CLOB | NULL | Values after change |
| OPERATION_STATUS | VARCHAR2(20) | CHECK IN ('ALLOWED','DENIED','PENDING') | Was operation allowed? |
| DENIAL_REASON | VARCHAR2(500) | NULL | Reason if denied |
| SURVEY_PERIOD_ID | NUMBER | FOREIGN KEY REFERENCES SURVEY_COLLECTION_PERIODS(PERIOD_ID) | Active survey period |
| IS_WITHIN_PERIOD | CHAR(1) | CHECK IN ('Y','N') | Was within survey period? |
| IP_ADDRESS | VARCHAR2(50) | NULL | User's IP address |
| SESSION_ID | VARCHAR2(100) | NULL | Database session ID |