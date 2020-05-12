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

You can find the blog post [here](https://community.servicenow.com/community?id=community_blog&sys_id=2c1a22e4dbb81050feb1a851ca961965) which lists out other functions that you can use.

### Installation

- Import the updateset found in [**dist**](/dist) folder. Committing the updateset will create a script include and a related unit test for the script.
- Run the unit test if you modify any functions that have been provided.
