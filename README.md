Create/Update an MSSQL Database in Azure
=========

Creates or updates new Microsoft SQL Database instance on existing Database Server on Azure

Requirements
------------

ansible >=2.9.0
Requires Azure_rm Modules

[Azure Account information]('https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html')
[Azure Resource module options]('https://docs.ansible.com/ansible/latest/modules/azure_rm_resource_module.html')


**MSSQL Server must present first**

Role Variables
--------------

```yaml
#Default required
resource_group:
sql_server: #Name of the existing SQL server

#SQL Database Options
sql_databases:
  - sql_db_name:  #Friendly Name of the database instance
sql_createmode: #Sets the type of creation mode of the database instance (ex. default, copy, restore, etc.)
SQL_DATABASE_FORCE_UPDATE: #SQL Database will be updated if given parameters differ from existing resource state. To force SQL Database update in any circumstances set this parameter to True.
sql_db_size: # SKU size for datbase, Basic for DTU and so on

sql_db_source_name: #When using create mode as "copy" this var will be needed, it defines the name of the datebase you are copying from for your new DB
subscription_id: #When using create mode as "copy" this var will be needed for the SOURCE ID
```

Dependencies
------------

MSSQL Server must be created first

Example Playbook
----------------

> Creating 1 Database in a server with SKU sizing

```yaml
---
- name: Azure Playbook for a single Azure SQL database instance
  hosts: localhost
  pre_tasks:
    - name: Setting desired state
      set_fact:
#dependency Vars
        resource_group: test
        sql_server: "ThisSQLserver"
#Role Vars
        sql_databases:
          - sql_db_name: "ThisDadatabase1"
        sql_db_size: Basic
  connection: local

  roles:
    - azure-sqldatabase
```

> Creating multiple Databases in a single server with defaults and Sku size

```yaml
- name: Azure Playbook for multiple Azure SQL database instance
  hosts: localhost
  pre_tasks:
    - name: Setting desired state
      set_fact:
#dependency Vars
        resource_group: test
        sql_server: "ThisSQLserver"
#Role Vars
        sql_databases:
          - sql_db_name: "ThisDadatabase1"
          - sql_db_name: "ThisDadatabase2"
          - sql_db_name: "ThisDadatabase3"
          - sql_db_name: "ThisDadatabase4"
        sql_db_size: Basic

  connection: local

  roles:
    - azure-sqldatabase
```

> Creating 1 Database from another existing database (copy)

```yaml
---
- name: Azure Playbook for a single Azure SQL database instance
  hosts: localhost
  pre_tasks:
    - name: Setting desired state
      set_fact:
#dependency Vars
        resource_group: test
        sql_server: "ThisSQLserver"
#Role Vars
        sql_databases:
          - sql_db_name: "ThisDadatabase1"

        sql_db_size: S0
        sql_createmode: copy
        sql_db_source_name: full-database-name-to-copy-from
        subscription_id: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx'
  connection: local

  roles:
    - azure-sqldatabase
```

> Creating multiple Databases from one existing database (copy)

```yaml
---
- name: Azure Playbook for a single Azure SQL database instance
  hosts: localhost
  pre_tasks:
    - name: Setting desired state
      set_fact:
#dependency Vars
        resource_group: test
        sql_server: "ThisSQLserver"
#Role Vars
        sql_databases:
          - sql_db_name: "ThisDadatabase1"
          - sql_db_name: "ThisDadatabase2"
          - sql_db_name: "ThisDadatabase3"
          - sql_db_name: "ThisDadatabase4"
        sql_db_size: S0
        sql_createmode: copy
        sql_db_source_name: full-database-name-to-copy-from
        subscription_id: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx'
  connection: local

  roles:
    - azure-sqldatabase
```

License
-------

MIT

Author Information
------------------

Adam Brassard: ABrass on IRC