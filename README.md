# SQOOP

Sqoop shell command is called through ExecuteStreamCommand processor in Apache NiFi

#### sqoop from PostgreSQL to Hive

```
-c "sqoop import --connect 'jdbc:postgresql://xxx.xxx.xxx.xxx:xxxx/db' \
--driver org.postgresql.Driver \
--connection-manager org.apache.sqoop.manager.GenericJdbcManager \
--username 'user_name' \
--password 'passwrd' \
--table schema.tmp_agg_data \
--where 'dt_ag=20211112' \
--direct \
--hive-import \
--hive-table 'table_name' \
--hive-database 'dbzhubanova' \
--hive-overwrite -m 1"
```

#### sqoop from Oracle to Hive
UpdateAttribute processor can be used to set the properties. It can be used in sqoop command as ${property}

```
-c "sqoop import --connect 'jdbc:oracle:thin:@(description=(address=(protocol=tcp)(host=host-sby)(port=xxxx))(connect_data=(service_name=serv_name)))' \
--username USER_NAME \
--password PASSWD \
--query 'select * from  dwh.${table_name} partition (${oraclepartition}) where $CONDITIONS' \
--hive-import \
--hive-table 'dwh_${table_name}' \
--hive-database 'dbzhubanova' \
--hive-overwrite -m 32 \
--hive-partition-key 'oraclepartition' \
--hive-partition-value '${oraclepartition}' \
--null-string '\\N' \
--split-by ${split_columns} \
--delete-target-dir \
--target-dir '/user/nifi_user/dwh.${table_name}_${oraclepartition}' \
--map-column-hive ${columns_mapping} \
--mapreduce-job-name ${table_name}_${oraclepartition} \
"
```
