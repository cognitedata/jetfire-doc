## Incremental Load
Incremental load means to load only new or updated data, with the goal of being more efficient. Jetfire support incremental load out of the box for [Raw](https://doc.cognitedata.com/api/v1/#tag/Raw) tables. 

When you query a Raw table in Jetfire, you will have one column for key and one for lastChanged, including your own columns. Most filters you apply to the columns will end up running against all the data in the table. However there is an exception - when filtering lastChanged. Filters on the lastChanged column is optimized by pushing the filter all the way to the Raw service itself. That also means incremental load can be ran manually by executing `select * from someTable where lastChanged > 123123213`, where a greater than filter is applied to only get rows that have changed since some time. However, with that approach you would have to manually bump the number you compare with, so that you only process the latest stuff. Instead Jetfire exposes a method called [`is_new`](https://github.com/cognitedata/jetfire-doc/blob/master/concepts/jetfire-sql-functions.md#is_newname-string-version-bigint), which returns true when the row is new, or false otherwise. 

### In practice
```sql
select * from someTable where is_new("someTable", lastChanged)
```
The query above will in the first run process all the rows of someTable, however in the second run it will only process what is new. The `"someTable"` is the name of the `is_new` filter and can be set to anything. It's used to differentiate multiple `is_new` calls in the same query, so that one can do `is_new` filtering on more than one table. Therefore it's best practice to use the name of the relation as the name of the `is_new` filter. 

**NOTE** `is_new` will always return true in preview, for ease of development of the transformation.  

### Backfill
Sometimes you will want to process all the data again, even though nothing has changed, this is also called backfill. How to do that today, is by changing the `is_new` filter name, for example by adding a postfix with an incrementing number (`"someTable_23"`). 
