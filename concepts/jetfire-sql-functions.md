# Jetfire SQL functions
Jetfire provides custom SQL functions to ease the development of recipes. When a function expects `var_args` it means it allows variable number of arguments of any type, including star `*`. In addition Spark got a huge set of built-in functions which are documented here https://spark.apache.org/docs/2.4.0/api/sql.

### `get_names(var_args): Array<String>`
Returns an array of the field names of a struct or row.
#### Example
```sql
select get_names(*) from mydb.mytable
-- Returns the column names of 'mydb.mytable'
```
```sql
select get_names(some_struct.*) from mydb.mytable
-- Returns the field names of 'some_struct'
```
### `cast_to_strings(var_args): Array<String>`
Casts the arguments to an array of strings. It handles array, struct and map types by casting it to JSON strings.
#### Example
```sql
select cast_to_strings(*) from mydb.mytable
-- Returns the values of all columns in 'mydb.mytable' as strings
```

### `to_metadata(var_args): Map<String, String>`
Creates metadata compatible type from the arguments. In practice it does `map_from_arrays(get_names(var_args), cast_to_strings(var_args))`. This function is convenient when you want to easily transform your columns or structures into a format that fits the metadata field found in CDP. 
#### Example
```sql
select to_metadata(*) from mydb.mytable
-- Creates a metadata structure from all the columns found in 'mydb.mytable'
```

### `to_metadata_except(excludeFilter: Array<String>, var_args)`
Returns a metadata structure (`Map<String, String>`) where strings found in `excludeFilter` will exclude keys found in `var_args`.
It's convenient when you want to put most of the columns into metadata, but not all of them, eg. `to_metadata_except(array("someColInStarToExclude"), *)`

#### Example
```sql
select to_metadata_except(array("myCol"), myCol, testCol) from mydb.mytable
-- Will create a map where myCol is filtered out, eg. the result will in this case be Map("testCol" -> testCol.value.toString) 
```

### `asset_ids(assetNames: Array<String>, rootAssetName: String): Array<BigInt>`
Tries to find matching asset names in the asset hierarchy which as root asset name as root. It will return the ids of the assets matched. **Note** currently it will abort the job if there were no match. See [Assets](https://doc.cognitedata.com/concepts/#assets) for more information about what is an Asset in CDP. 

#### Example
```sql
select asset_ids(array("PV10"), "MyBoat")
```

### `is_new(name: String, version: BigInt)`
Returns true if the version provided is higher than the version found with the specified name, based on the last time the transformation was ran. This is a core part of [Incremental Load](incremental_load.md).

#### Example
```sql
select lastChanged from mydb.mytable where is_new("mydb.mytable.version", lastChanged)
```
