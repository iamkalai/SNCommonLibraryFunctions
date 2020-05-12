# SNCommonLibraryFunctions

## Description

**_This project is an attempt to reduce repeatedly writing boilerplate code, reduce clutter and promote reusability._**

The project creates a new script include called **'CommonLibraryFunctions'**.

### What does the script do

For now, it can help you avoid writing some boilerplate Gliderecord queries. Let's take a sample scenario. You are being asked to check if RITM has any active tasks before performing some action.

_Usual query will look like this._

```javascript
var answer = false;
var ritmSYDID = 'something';
var activeTask = new GlideRecord('sc_task');
activeTask.addEncodedQuery('active=true^request_item=' + ritmSYDID);
activeTask.query();
if (activeTask.hasNext()) {
    answer = true;
}
gs.print(answer);
```

You can use the CommonLibraryFunctions to remove boilerplate gliderecord. _Your new script will look something like this_

```javascript
var ritmSYDID = 'something';
var answer = new CommonLibraryFunctions().hasValidRecord('sc_task','active=true^request_item=' + ritmSYDID);
gs.print(answer);
```

_Below are the functions included in the script currently_. I am hoping to expand this in future.

- **hasValidRecord**: Utility function that checks if the record exists or not. This can be used both client and server side. Example for this can be found above.

- **getRecord**: Utility function that returns the gliderecord object. This can be used on server side.

```javascript
//Get the user object
var userID = gs.getUserID();
var user = new CommonLibraryFunctions().getRecord('sys_user', 'sys_id=' + userID);
if (user) {
    gs.print(user.manager);
}
```

- **getFieldValue**: Utility function that returns a field value of a record. This can be used both client and server side.

```javascript
//Get the manager of user object
var userID = gs.getUserID();
var manager = new CommonLibraryFunctions().getFieldValue('sys_user', 'sys_id=' + userID, 'manager');
gs.print(manager);
```

- **getRecordsFieldValue**: Utility function that returns a field value of multiple records. Return data also includes duplicate. This can be used both client and server side.

```javascript
//Get list of active incident numbers
- var incidents = new CommonLibraryFunctions().getRecordsFieldValue('incident','active=true','number');
gs.print(incidents);
```

- **getUniqueValue**: Utility function that returns list of unique values of records. This can be used both client and server side.

```javascript
//Get list of unique callers for incidents
var callers = new CommonLibraryFunctions().getUniqueValue('incident','active=true','caller_id');
gs.print(callers);
```

- **executeScheduledJob**: Utility function to execute a scheduled job. This can be used both client and server side.

- **customErrorLogger**: Utility function used to log message. This can be used both client and server side.

### Installation

- Import the updateset found in [**dist**](/dist) folder. Committing the updateset will create a script include and a related unit test for the script.
- Run the unit test if you modify any functions that have been provided.
