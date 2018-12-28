<p align="center">
  <img src="https://assets.futuready.com/v2/asset/images/logo-futuready.png" height="40" /><br/>
  <span><b>API Documentation</b></span><br/>
  develop by and for <a href="https://futuready.com/" target="_blank">futuready.com</a>
</p>
  
<br/>

## General options

**All API should have api key in header**, please ask developer for any api_key.

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
| `value` | Optional; One of the following method use this value: [`getsingle`](#getsingle) |
| `primarykey`| String of id/primary key; |
| `pk_value`| String or integer as value of those primary key |
| `user_id`| To capture user who doing update and store it to database |
| `body format`| Only `json` is supported at the moment. This parameter is mandatory for all POST method |

## Available Method

GET Method:
- [x] **[`getsingle`](#getsingle)**: Return array of one records from table; *without softDelete;
- [x] **[`get`](#get)**: Return array of records from table; *without softDelete;
- [x] **[`readsoftdeleted`](#readsoftdeleted)**: Return array of softDelete records from table;

POST Method:
- [x] **[`insert`](#insert)**: Insert record, on successful updates return success: true;
- [x] **[`update`](#update)**: Updates record, on successful updates return success: true;
- [x] **[`Delete`](#Delete)**: Instead delete record from table, we add deletedAt in database; (soft deleted)
- [x] **[`updatesoftdeleted`](#updatesoftdeleted)**: Restore data which already in softDeletedRecord;

Another / Additional:
- [x] **[`hardDeleteRecord`](#hardDeleteRecord)**: Delete record from table, and store it to log; (by request)

## Running it locally

- `git clone --git link will be shared later--`

That's it. It will copy the controller. 

## Data Schema

For purpose of soft delete vs hard delete, our return of array will look like this:

```endpoint
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

```endpoint
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

## `getsingle`

***Goal :***
  * return array of one records from table; *without softDelete;
  
***How to call:***
```endpoint
  GET: directoryname/{controller}/getsingle/{value}
```
***Parameters :***
> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `{value}++`: primary key value;

<br/>

## `get`

***Goal :***
  * return array of records from table (without softDelete)
  
***How to call:***
```endpoint
  GET: directoryname/{controller}/get
```
***Parameters :***
> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `id++ `: primary key
- `user_id++ `: your username;
- `...`: another column & value;

<br/>

## `getSoftDeleteRecords`

***Goal :***
  * return array of softDelete records from table
  
***How to call:***
```endpoint
  GET: directoryname/{controller}/readsoftdeleted
```
***Parameters :***
> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `id++ `: primary key
- `user_id++ `: your username;
- `...`: another column & value;

<br/>

## `insertRecord`

***Goal :***
  * insert record, on successful updates return success: true.
  
***How to call:***
```endpoint
  POST: directoryname/{controller}/insert
```
***Parameters :***
```
    {
        "id": 1,
        ...
        "user_id": "yourusername"
    }
```

> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `id++ `: primary key
- `user_id++ `: your username;
- `...`: another column & value;

<br/>

## `updateRecord`

***Goal :***
  * updates record, on successful updates return success: true.
  
***How to call:***
```endpoint
  POST: directoryname/{controller}/update
```
***Parameters :***
```
    {
        "id": 1,
        ...
        "user_id": "yourusername"
    }
```

> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `id++ `: primary key
- `user_id++ `: your username;
- `...`: another column & value;

<br/>

## `DeleteRecord`

***Goal :***
  * instead delete record from table, we use softdelete. For use harddelete / remove record from database, please ask backend developer.
  
***How to call:***
```endpoint
  POST: directoryname/{controller}/delete
```
***Parameters :***
```
    {
        "id": 1,
        "user_id": "yourusername"
    }
```

> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `id++ `: primary key
- `user_id++ `: your username;

<br/>

## `restoreSoftDeletedRecord`

***Goal :***
  * restore data which already in softDeletedRecord.
  
***How to call:***
```endpoint
  POST: directoryname/{controller}/updatesoftdeleted
```
***Parameters :***
```
    {
        "id": 1,
        "user_id": "yourusername"
    }
```

> ++ indicates parameter is must
- `{controller}++`: Controller Name;
- `id++ `: primary key
- `user_id++ `: your username;

<br/>

## Author

Copyright (c) 2018 [Futuready](https://futuready.com).
