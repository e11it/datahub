> This doc is for older versions (v0.2.1 and before) of WhereHows. Please refer to [this](../wherehows-etl/README.md) for the latest version.

This is an optional feature.

Almost every enterprise has an LDAP server. This information is essential for any metadata containing an LDAP ID. For example, if we have the owner ID of a dataset, we also would like to know more about the owner: email address, phone number, manager, department, etc. All that information comes from the LDAP server.

## Extract
Major related files: [LdapExtract.py](../wherehows-etl/src/main/resources/jython/LdapExtract.py)

There are two major parts at this stage: individual information and group/user mapping.

* For an individual user, extract the following standardized attributes from the LDAP server:

    'user_id', 'distinct_name', 'name', 'display_name', 'title', 'employee_number', 'manager', 'mail', 'department_number', 'department', 'start_date', 'mobile'

    You can specify the actual LDAP return attributes in the job properties. 

* For a group account, find all the users in the group. Because a group can contain a subgroup, two mapping files are stored, one raw mapping file, and one flattened mapping file that will find all users in the subgroup.

## Transform
Major related files: [LdapTransform.py](../wherehows-etl/src/main/resources/jython/LdapTransform.py)

Additional derived information is added during the transform stage: hierarchy. The path from the top management to the specified user is generated by recursively looking for manager until it reaches the CEO.

## Load
Major related files: [LdapLoad.py](../wherehows-etl/src/main/resources/jython/LdapLoad.py)

Loading from staging table into final table.