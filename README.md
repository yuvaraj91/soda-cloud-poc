# Data quality with Soda Cloud

Proof-of-concept for the free trial of Soda Cloud + dbt. Tested in Snowflake.


# Setup

Quick local setup

```
python3 -m venv .venv
source .venv/bin/activate
pip install -i https://pypi.cloud.soda.io soda-snowflake
```

Validate by running `soda --help`.

To exit the virtual environment, run the command `deactivate`.

# Snowflake Connection

I use my personal Snowflake account as the test data warehouse. The `configuration.yml` file stores the access details.
Copy and paste the connection configuration details for Snowflake as in the example below.


```
data_source snowflake:
   type: snowflake
   connection:
     username: ${SNOWFLAKE_USER}
     password: ${SNOWFLAKE_PASS}
     account: ${SNOWFLAKE_ACCOUNT}
     database: local
     warehouse: compute_wh
     role: accountadmin
     session_parameters:
       QUERY_TAG: soda-queries
       QUOTED_IDENTIFIERS_IGNORE_CASE: false
   schema: public
```

In Soda Cloud, go to your avatar > Profile, then access the API keys tab. Click the plus icon to generate a new set of API keys.
Add the block to the configuration.yml file. The configuration.yml file contains the values, these needs to be saved in your environment file (e.g., `.zshrc` for Mac OS)

Test the connection: `soda test-connection -d snowflake -c configuration.yml`.


# Running Soda cli in the Terminal

With the sample checks as defined in `checks.yml`, run the following command:

```bash
soda scan -d snowflake -c configuration.yml checks.yml
```

The checks are run and sent to Soda Cloud. To avoid sending to Soda Cloud if you just want to test it locally, run instead:

```bash
soda scan -d snowflake -c configuration.yml checks.yml --local
```