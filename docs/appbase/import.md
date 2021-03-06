# Import command

Import command can be used to import data from any supported database source into appbase.io/ES cluster. 
It goes like - 

```
abc import --src_uri {URI} --src_type {DBType} --tail [URI|Appname]
```

To view the complete list of input parameters supported, use -

```
abc import --help
```

At the time of writing, the list of parameters supported looks like -

```
--config=                                    Path to external config file, if specified, only that is used
--log.level="info"                           Only log messages with the given severity or above. Valid levels: [debug, info, error]
--replication_slot=standby_replication_slot  [postgres] replication slot to use
--src_filter=.*                              Namespace filter for source
--src_type=postgres                          type of source database
--src_uri=http://user:pass@host:port/db      url of source database
--tail=false                                 allow tail feature
--test=false                                 if set to true, only pipeline is created and sync is not started. Useful for checking your configuration
--transform_file=                             URI of transform file to use
--typename=mytype                            [csv] typeName to use
```

Note that you only need to set the parameters that are required for the source database type. For example, you don't set `replication_slot` when taking CSV as the source. 

**Note** - Help for [transform_file](../importer/transform_file.md) is available here.


## Examples


### CSV

```sh
abc import --src_type=csv --typename=csvTypeName --src_uri="file.csv" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```


### ElasticSearch

```sh
abc import --src_type=elasticsearch --src_uri="http://USER:PASS@HOST:PORT/INDEX" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```

We can also use an Appbase app as source.

```sh
abc import --src_type=elasticsearch --src_uri="https://USER:PASS@scalr.api.appbase.io/APPNAME2" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```

We can even use the app's name as URI once we are logged in.

```sh
abc import --src_type=elasticsearch --src_uri=APPNAME2 APPNAME
```


### MongoDB

```sh
abc import --src_type=mongodb -t --src_uri="mongodb://USER:PASS@HOST:PORT/DB" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```


### MSSQL

```sh
abc import --src_type=mssql --src_uri="sqlserver://USER:PASSWORD@SERVER:PORT?database=DBNAME" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```

For more source URL patterns, see [go-mssqldb](https://github.com/denisenkom/go-mssqldb#connection-parameters-and-dsn)'s GitHub page. 


### MySQL

```sh
abc import --src_type=mysql --src_uri="USER:PASS@tcp(HOST:PORT)/DBNAME" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```

For more source URL patterns, see [go-sql-driver/mysql](https://github.com/go-sql-driver/mysql#examples)'s GitHub page. 


### Postgres

```sh
abc import --src_type=postgres -t --replication_slot="standby_replication_slot" --src_uri="postgresql://USER:PASS@HOST:PORT/DBNAME" "https://USER:PASS@scalr.api.appbase.io/APPNAME"
```

### Using a config file

```sh
abc import --config=test.env
```

File extension doesn't matter. 
The file `test.env` should be an INI/ENV like file with key value pair containing the values of attributes required for importing.
Example of a test.env file is --

```ini
src_type=csv
src_uri=/full/path/to/file.csv
typename=csvTypeName

dest_type=elasticsearch
dest_uri=https://USER:PASS@scalr.api.appbase.io/APPNAME
```

Note that the key names are same as what we have in `import` parameters. 

