Connecting Apache Superset with Databricks
Overview
This guide provides a step-by-step approach to integrating Apache Superset, an open-source data exploration and visualization platform, with Databricks, a unified analytics platform powered by Apache Spark.
Prerequisites
- A running instance of Apache Superset (self-hosted or Docker)
- Access to a Databricks workspace with SQL warehouses
- A Databricks personal access token
- Required Python packages (sqlalchemy-databricks, or databricks-sql-connector)
1. Install Required Dependencies
Run one of the following commands based on your preferred connector:

```bash
pip install sqlalchemy-databricks
```
or
```bash
pip install databricks-sql-connector
```

For Docker setups, add to requirements-local.txt or Dockerfile.
2. Generate a Databricks Personal Access Token
1. Login to your Databricks account.
2. Navigate to User Settings > Access Tokens.
3. Generate a new token and copy it securely.
3. Gather Connection Details
- Server hostname (e.g., abc-databricks.cloud.databricks.com)
- HTTP Path from SQL Warehouse
- Access token
- Port (default: 443)
4. Create a Database Connection in Superset
**Option A (Recommended): SQLAlchemy URI**
```
databricks+connector://token:<TOKEN>@<HOST>:443/sql/protocolv1/o/<ORG_ID>/<HTTP_PATH>
```

**Option B: Using ODBC (PyODBC)**
```
databricks://token:<TOKEN>@<HOST>:443/default?http_path=<HTTP_PATH>
```
5. Test Connection
Click 'Test Connection' in Superset UI. If successful, click 'Connect'.
6. Docker Setup (Optional)
**Dockerfile Additions:**
```Dockerfile
FROM apache/superset
USER root
RUN pip install sqlalchemy-databricks
USER superset
```

**requirements-local.txt:**
```
sqlalchemy-databricks
```

**Rebuild Docker:**
```bash
docker-compose down
docker-compose build
docker-compose up -d
```
Troubleshooting
- `ModuleNotFoundError`: Ensure the correct Python environment
- Token errors: Check for expiration or miscopy
- HTTP path errors: Confirm correct formatting
Security Best Practices
- Never hardcode tokens
- Rotate tokens regularly
- Apply role-based access control
References
- https://docs.databricks.com/integrations/jdbc.html
- https://superset.apache.org/docs/databases/
