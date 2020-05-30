# SNCommonLibraryFunctions

## Description

**_This project is an attempt to reduce boilerplate code, reduce clutter and promote reusability._**

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
var options = {
    table: "sc_task",
    query: "active=true^request_item=" + ritmSYDID,
};
var answer = JSON.parse(new CommonLibraryFunctions().hasRecord(options));
gs.print("hasRecord:"+JSON.stringify(answer));
```

_Below are the functions included in the script currently_. I am hoping to expand this in future.

- **hasRecord**: Utility function that checks if the record exists or not. This can be used both on client and server side.

```javascript
//Server side

var options = {
    table: "incident",
    query: "active=true",
};
var answer = JSON.parse(new CommonLibraryFunctions().hasRecord(options));
gs.print("hasRecord:" + JSON.stringify(answer));

//Client side

var options = {
    table: "incident",
    query: "active=true",
};

var ga = new GlideAjax("CommonLibraryFunctions");
ga.addParam("sysparm_name", "hasRecord");
ga.addParam("sysparm_options", JSON.stringify(options));
ga.getXML(function (response) {
    var answer = JSON.parse(response.responseXML.documentElement.getAttribute("answer"));
    alert("hasRecord:" + JSON.stringify(answer));
});
```

- **getRecord**: Utility function that returns the gliderecord object. This can be used on server side.

```javascript
var options = {
    table: "incident",
    query: "active=true",
};
var answer = new CommonLibraryFunctions().getRecord(options);
gs.print("getRecord:" + answer.getRowCount());
```

- **getField**: Utility function that returns a field value of a record. This can be used both on client and server side.

```javascript
//Server side

var options = {
    table: "sys_user",
    query: "sys_id=" + gs.getUserID(),
    field: "manager",
};
var answer = JSON.parse(new CommonLibraryFunctions().getField(options));
gs.print("getField:" + JSON.stringify(answer));

//Client side

var options = {
    table: "sys_user",
    query: "sys_id=" + g_user.userID,
    field: "manager",
};

var ga = new GlideAjax('CommonLibraryFunctions');
ga.addParam('sysparm_name', 'getField');
ga.addParam('sysparm_options', JSON.stringify(options));
ga.getXML(function (response) {
    var answer = JSON.parse(response.responseXML.documentElement.getAttribute("answer"));
    alert("getField:" + JSON.stringify(answer));
});
```

- **getRecordsField**: Utility function that returns a field value of multiple records. Return data also includes duplicate. This can be used both on client and server side.

```javascript
//Server side

var options = {
    table: "incident",
    query: "active=true",
    field: "number",
};
var answer = JSON.parse(new CommonLibraryFunctions().getRecordsField(options));
gs.print("getRecordsField:" + JSON.stringify(answer));

//Client side

var options = {
    table: "incident",
    query: "active=true",
    field: "number",
};

var ga = new GlideAjax('CommonLibraryFunctions');
ga.addParam('sysparm_name', 'getRecordsField');
ga.addParam('sysparm_options', JSON.stringify(options));
ga.getXML(function (response) {
    var answer = JSON.parse(response.responseXML.documentElement.getAttribute("answer"));
    alert("getRecordsField:" + JSON.stringify(answer));
});
```

- **getUnique**: Utility function that returns list of unique values of records. This can be used both on client and server side.

```javascript
//Server side

var options = {
    table: "incident",
    query: "active=true",
    field: "category",
};
var answer = JSON.parse(new CommonLibraryFunctions().getUnique(options));
gs.print("getUnique:" + JSON.stringify(answer));

//Client side

var options = {
    table: "incident",
    query: "active=true",
    field: "category",
};

var ga = new GlideAjax('CommonLibraryFunctions');
ga.addParam('sysparm_name', 'getUnique');
ga.addParam('sysparm_options', JSON.stringify(options));
ga.getXML(function (response) {
    var answer = JSON.parse(response.responseXML.documentElement.getAttribute("answer"));
    log("getUnique:" + JSON.stringify(answer));
});
```

- **getAggregate**: Utility function that returns the glideaggregate object. This can be used on server side.

```javascript
var options = {
    table: "incident",
    query: "active=true",
};
var answer = new CommonLibraryFunctions().getAggregate(options);
gs.print("getAggregate:" + answer.getRowCount());
```

- **getRecordCount**: Utility function that returns count of records. This can be used on both client and server side.

```javascript
//Server side

var options = {
    table: "incident",
    query: "active=true",
};
var answer = JSON.parse(new CommonLibraryFunctions().getRecordCount(options));
gs.print("getRecordCount:" + JSON.stringify(answer));

//Client side

var options = {
    table: "incident",
    query: "active=true",
};

var ga = new GlideAjax("CommonLibraryFunctions");
ga.addParam("sysparm_name", "getRecordCount");
ga.addParam("sysparm_options", JSON.stringify(options));
ga.getXML(function (response) {
    var answer = JSON.parse(response.responseXML.documentElement.getAttribute("answer"));
    alert("getRecordCount:" + JSON.stringify(answer));
});
```

- **executeJob**: Utility function to execute a scheduled job. This can be used both on client and server side.

- **log**: Utility function used to log message. This can be used both on client and server side.

```javascript
//Server side

new CommonLibraryFunctions().log({ message: "Log Message" });

//Client side

var options = {
    message: "Log Message"
};
var ga = new GlideAjax("CommonLibraryFunctions");
ga.addParam("sysparm_name", "log");
ga.addParam("sysparm_options", JSON.stringify(options));
ga.getXML();
```

### Installation

- Import and commit the updateset found in [**dist**](/dist) folder.
- A new script include and a related unit test for the script.
- Run the unit test if you modify any functions that have been provided.

### License

GNU General Public License v3.0
