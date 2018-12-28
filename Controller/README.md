<p align="center">
  <img src="https://assets.futuready.com/v2/asset/images/logo-futuready.png" height="40" /><br/>
  <span><b>BasicModel</b>: <span>Universal Model for Futuready Codeigniter [BETA]</span><br/>
  develop by and for <a href="https://futuready.com/" target="_blank">futuready.com</a>
</p>
  
<br/>

## General options

All API should have api key in header, please ask developer for any api_key.

The following syntax applies to all services, except as noted.

```endpoint
GET/POST: {controller}/{method}/{value:optional}

{
    "primarykey": pk_value,
    ...
    "user_id": "yourusername"
}
```
| Parameter | Description |
| --- | --- |
| `controller` | Class / filename as a blueprint or a set of instruction / method |
| `method` | Method name |
| `value` | Optional; One of the following method use this value: [`getsingle`](###getsingle) |
| `primarykey`| String of id/primary key; for open the data detail, please get all data first using [`getsingle`](###getsingle) method |
| `pk_value`| String or integer as value of those primary key |
| `user_id`| To capture user who doing update and store it to database |
| `body format`| Only `json` is supported at the moment. This parameter is mandatory for all POST method |

## Available Method

GET Method:
- [x] **getsingle**: Return array of one records from table; *without softDelete;
- [x] **get**: Return array of records from table; *without softDelete;
- [x] **readsoftdeleted**: Return array of softDelete records from table;

POST Method:
- [x] **insert**: Insert record, on successful updates return success: true;
- [x] **update**: Updates record, on successful updates return success: true;
- [x] **Delete**: Instead delete record from table, we add deletedAt in database; (soft deleted)
- [x] **updatesoftdeleted**: Restore data which already in softDeletedRecord;

Another / Additional:
- [x] **hardDeleteRecord**: Delete record from table, and store it to log;

## Running it locally

- `cd application/models`
- `git clone git@github.com:devhubapp/devhub.git`

That's it. It will copy the basicModel to your model. 

## Data Schema

For purpose of soft delete vs hard delete, our return of array will look like this:

```json
[
    {
        "id": "1",
        ...
        "createdAt": null,
        "updatedAt": null,
        "deletedAt": null
    },
    {
        "id": "2",
        ...
        "createdAt": null,
        "updatedAt": null,
        "deletedAt": null
    }
]
```

or in table view looks like this:

```
┌────┬─────┬───────────┬───────────┬───────────┐
│ id │ ... │ createdAt │ updatedAt │ deletedAt │
├────┼─────┼───────────┼───────────┼───────────┤
│  1 │ ... │           │           │           │
├────┼─────┼───────────┼───────────┼───────────┤
│  2 │ ... │           │           │           │
└────┴─────┴───────────┴───────────┴───────────┘

- `createdAt`   | DateTime | Use to capture time data stored in database
- `updatedAt`   | DateTime | CURRENT_TIMESTAMP | Use to capture time data updated
- `deletedAt`   | DateTime | Use to capture time data softdeleted
```

<br/>

## How to Call

## `getRecordCount`

***Goal :***
  * return number of rows
  
***How to call:***
```php
  $this->BasicModels->getRecordCount('tbl_name',$condition_array);
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table 
- `$condition `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);

<br/>

## `getRecords`

***Goal :***
  * return array of records from table (without softDelete)
  
***How to call:***
```php
  $this->BasicModels->getRecords('tbl_name',$condition_array,$select,...);
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table 
- `$condition `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);
- `$select `: ('col1,col2,col3');
- `$order_by `: array('colname1'=>order,'colname2'=>order); Order='ASC OR DESC';
- `$start `: start for paging (number);
- `$limit `: limit for paging (number);
- `$join `: array('jointable'=>$table_b,'match_a'=>$table_a_reference,'match_b'=>$table_b_reference,'join_type'=>$join_type); join_type = FALSE/'LEFT'/'RIGHT';

> Note: in case where we need joins, you can pass joins in controller also:
```php
  $this->db->join('tbl_nameB AS b','tbl_nameA.col=b.col','left');
  $this->BasicModels->getRecords('tbl_name',$condition_array,$select,...);	
```

<br/>

## `getSoftDeleteRecords`

***Goal :***
  * return array of softDelete records from table
  
***How to call:***
```php
  $this->BasicModels->getSoftDeleteRecords('tbl_name',$condition_array,$select,...);
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table 
- `$condition `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);
- `$select `: ('col1,col2,col3');
- `$order_by `: array('colname1'=>order,'colname2'=>order); Order='ASC OR DESC';
- `$start `: start for paging (number);
- `$limit `: limit for paging (number);
- `$join `: array('jointable'=>$table_b,'match_a'=>$table_a_reference,'match_b'=>$table_b_reference,'join_type'=>$join_type); join_type = FALSE/'LEFT'/'RIGHT';

> Note: in case where we need joins, you can pass joins in controller also:
```php
  $this->db->join('tbl_nameB AS b','tbl_nameA.col=b.col','left');
  $this->BasicModels->getRecords('tbl_name',$condition_array,$select,...);	
```

<br/>

## `insertRecord`

***Goal :***
  * insert record, on successful updates return success: true.
  
***How to call:***
```php
  $this->BasicModels->insertRecord('tbl_name',$data_array,$id);
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table 
- `$data_array++ `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);
- `$id `: primary column value. only use insert_id if ID is autoincrement;

<br/>

## `updateRecord`

***Goal :***
  * updates record, on successful updates return success: true.
  
***How to call:***
```php
  $this->BasicModels->updateRecord('tbl_name',$data_array,$pri_col,$id)
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table;
- `$data_array++ `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);
- `$pri_col++ `: primary key or column name depending on which update query need to fire;
- `$id `: primary column value. only use insert_id if ID is autoincrement;

<br/>

## `softDeleteRecord`

***Goal :***
  * instead delete record from table, we add deletedAt in database
  
***How to call:***
```php
  $this->BasicModels->softDeleteRecord('tbl_name','pri_col',$id)
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table;
- `$pri_col++ `: primary key or column name depending on which update query need to fire;
- `$id `: primary column or condition column value;

> It will useful while deleting record from single table. *delete join will not work here.

<br/>

## `restoreSoftDeletedRecord`

***Goal :***
  * restore data which already in softDeletedRecord.
  
***How to call:***
```php
  $this->BasicModels->restoreSoftDeletedRecord('tbl_name','pri_col',$id)
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table;
- `$data_array++ `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);
- `$pri_col++ `: primary key or column name depending on which update query need to fire;
- `$id `: primary column or condition column value;

> It will useful while deleting record from single table. *delete join will not work here.

<br/>

## `hardDeleteRecord`

***Goal :***
  * delete record from table, and store it to log.
  
***How to call:***
```php
  $this->BasicModels->hardDeleteRecord('tbl_name','pri_col',$id)
```
***Parameters :***
> ++ indicates parameter is must

- `$tbl_name++ `: name of table;
- `$data_array++ `: array('column_name1'=>$column_val1,'column_name2'=>$column_val2);
- `$pri_col++ `: primary key or column name depending on which update query need to fire;
- `$id `: primary column or condition column value;

> It will useful while deleting record from single table. *delete join will not work here.

<br/>

## Author

Copyright (c) 2018 [Futuready](https://futuready.com).
