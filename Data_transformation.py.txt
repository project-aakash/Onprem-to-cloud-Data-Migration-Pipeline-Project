# Databricks notebook source
configs = {
  "fs.azure.account.auth.type": "CustomAccessToken",
  "fs.azure.account.custom.token.provider.class": spark.conf.get("spark.databricks.passthrough.adls.gen2.tokenProviderClassName")
}

# Optionally, you can add <directory-name> to the source URI of your mount point.
dbutils.fs.mount(
  source = "abfss://bronze@mrkdatalakegen2.dfs.core.windows.net/",
  mount_point = "/mnt/bronze",
  extra_configs = configs
)

# COMMAND ----------

dbutils.fs.ls("/mnt/bronze/SalesLT/")

# COMMAND ----------

configs = {
  "fs.azure.account.auth.type": "CustomAccessToken",
  "fs.azure.account.custom.token.provider.class": spark.conf.get("spark.databricks.passthrough.adls.gen2.tokenProviderClassName")
}

# Optionally, you can add <directory-name> to the source URI of your mount point.
dbutils.fs.mount(
  source = "abfss://silver@mrkdatalakegen2.dfs.core.windows.net/",
  mount_point = "/mnt/silver",
  extra_configs = configs
)

# COMMAND ----------

configs = {
  "fs.azure.account.auth.type": "CustomAccessToken",
  "fs.azure.account.custom.token.provider.class": spark.conf.get("spark.databricks.passthrough.adls.gen2.tokenProviderClassName")
}

# Optionally, you can add <directory-name> to the source URI of your mount point.
dbutils.fs.mount(
  source = "abfss://gold@mrkdatalakegen2.dfs.core.windows.net/",
  mount_point = "/mnt/gold",
  extra_configs = configs
)

# COMMAND ----------

# Databricks notebook source
# MAGIC %md
# MAGIC ## Doing transformation for all tables

# COMMAND ----------

table_name = []

for i in dbutils.fs.ls('mnt/bronze/SalesLT/'):
  print(i.name)
  table_name.append(i.name.split('/')[0])

# COMMAND ----------

table_name

# COMMAND ----------

from pyspark.sql.functions import from_utc_timestamp, date_format
from pyspark.sql.types import TimestampType

for i in table_name:
  path = '/mnt/bronze/SalesLT/' + i + '/' + i + '.parquet'
  df = spark.read.format('parquet').load(path)
  column = df.columns

  for col in column:
    if "Date" in col or "date" in col:
      df = df.withColumn(col, date_format(from_utc_timestamp(df[col].cast(TimestampType()), "UTC"), "yyyy-MM-dd"))

  output_path = '/mnt/silver/SalesLT/' + i + '/'
  df.write.format('delta').mode("overwrite").save(output_path)

# COMMAND ----------

display(df)

# COMMAND ----------

# Databricks notebook source
# MAGIC %md
# MAGIC ## Doing transformation for all tables (Changing column names)

# COMMAND ----------

table_name = []

for i in dbutils.fs.ls('mnt/silver/SalesLT/'):
  print(i.name)
  table_name.append(i.name.split('/')[0])

# COMMAND ----------

table_name

# COMMAND ----------

for name in table_name:
  path = '/mnt/silver/SalesLT/' + name
  print(path)
  df = spark.read.format('delta').load(path)

  # Get the list of column names
  column_names = df.columns

  for old_col_name in column_names:
      # Convert column name from ColumnName to Column_Name format
      new_col_name = "".join(["_" + char if char is upper() and not old_col_name[i - 1].isupper() else char for i, char in enumerate(old_col_name)]).lstrip("_")
      
      # Change the column name using withColumnRenamed and regexp_replace
      df = df.withColumnRenamed(old_col_name, new_col_name)

  output_path = '/mnt/gold/SalesLT/' + name + '/'
  df.write.format('delta').mode("overwrite").save(output_path)

# COMMAND ----------

display(df)
