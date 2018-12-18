---
title: Sunsama API V0 Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://sunsama.com'>Sunsama</a>

includes:
  - errors
  - examples

search: true
---

# Introduction

Welcome to the Sunsama API V0. You can use this API to create, edit, and retrieve information about tasks in your Sunsama workspace. 

# Authentication

## Getting your API Key

Please contact support@sunsama.com to request an API key for your workspace. 

All requests must be authenticated with the `Authorization` header with your workspace api key as the value. 

> To authorize requests pass your workspace-api-key as the `Authorization` header. 

```javascript
var options = {
    uri: 'https://sunsama.com/api/v0/tasks',
    qs: {
        id: 'xxxxxxxxxx'
    },
    headers: {
        'Authorization': 'workspace-api-key',
        'Content-Type': 'application/json',
    },
    json: true,
};
```

# Tasks 

## Get a Task

```javascript
var options = {
    uri: 'https://sunsama.com/api/v0/tasks',
    qs: {
        id: 'task-id',
    },
    headers: {
        'Authorization': 'workspace-api-key',
        'Content-Type': 'application/json',
    },
    json: true // Automatically parses the JSON string in the response
};

rp(options)
    .then((task) => console.log(task))
    .catch(function (err) {
        // API call failed...
    });
```

> The above command returns JSON structured like this:

```json
{
    "_id": "9oFhjoAWQqhntetxj",
    "createdBy": "ashutosh@sunsama.com",
    "text": "send that email",
    "createdAt": "2018-09-04T05:38:24.531Z",
    "completed": true,
    "private": false,
    "followers": [],
    "notes": "<div>complex notes that I made</div>",
    "subtasks": [
        {
            "title": "do 1"
        },
        {
            "title": "do 2"
        },
        {
            "title": "do 3"
        }
    ],
    "lastModified": "2018-09-04T21:13:46.029Z",
    "comments": [
        {
            "text": "<div>my first comment</div>",
            "createdAt": "2018-09-04T07:15:12.635Z",
            "user": "ashutosh@sunsama.com"
        },
        {
            "text": "uploaded this file: <a href=\"https://cdn.filestackcontent.com/Jd5krXjPQ4e1Jv2mhNY9?policy=eyJjb250YWluZXIiOiJzdW5zYW1hLXN0b3JhZ2UtZGV2IiwibWF4U2l6ZSI6MTA0ODU3NjAsImV4cGlyeSI6MTY5Mzg0NzQwNTA0NiwiY2FsbCI6WyJyZWFkIl0sInBhdGgiOiIvZ3JvdXBzL2R1Q25HNnhlQkNIUjg3WG1zLyoifQ==&signature=b4e0e19665cf4bd0e6577cc8235ad2939ce158a39c2bddb356d8bf7f9b0a810f\" target=\"_blank\">Screen Shot 2018-08-28 at 14.12.41.png</a>",
            "createdAt": "2018-09-04T17:10:05.062Z",
            "user": "ashutosh@sunsama.com"
        }
    ],
    "completeOn": "2018-09-03T20:00:00.000Z",
    "workingSessions": [],
    "assignee": "ashutosh@sunsama.com"
}
```

This endpoint retrieves a single task. 

### HTTP Request

`GET https://sunsama.com/api/v0/tasks`

### URL Query String Parameters

Parameter | Required | Description
--------- | ------- | -----------
id | false | The id of the task you want to retrieve. 
externalResourceId | false | If you created this task with the API and specified an externalResourceId at the time of creation, you can use this id to find the task again. 

<aside class="success">
You must include either `id` or `externalResourceId` to be able retrieve a task 
</aside>

## Create a new task

```javascript
var options = {
    method: 'POST',
    uri: 'https://sunsama.com/api/v0/tasks',
    qs: {
        text: 'my new task',
        createdBy: 'ashutosh@sunsama.com', 
    },
    headers: {
        'Authorization': 'workspace-api-key',
        'Content-Type': 'application/json',
    },
    json: true,
};

rp(options)
    .then(({ taskId }) => console.log(taskId))
    .catch(function (err) {
        // API call failed...
    });
```

> The above command returns JSON structured like this:

```json
{
  "taskId": "9oFhjoAWQqhntetxj"
}
```

This endpoint creates a single task. 


## Get Tasks on a Day

```javascript
var options = {
    uri: 'https://sunsama.com/api/v0/tasks/day',
    qs: {
        userEmail: 'ashutosh@sunsama.com',
        date: '2018-12-17T00:00:00-07:00',
        timezone: 'America/Los_Angeles',
    },
    headers: {
        'Authorization': 'workspace-api-key',
        'Content-Type': 'application/json',
    },
    json: true // Automatically parses the JSON string in the response
};

rp(options)
    .then((task) => console.log(task))
    .catch(function (err) {
        // API call failed...
    });
```

> The above command returns JSON structured like this:

This endpoint retrieves an array of tasks. 

### HTTP Request

`GET https://sunsama.com/api/v0/tasks/day`

### URL Query String Parameters

Parameter | Required | Description
--------- | ------- | -----------
userEmail | true | The Sunsama account email for the user's tasks you want
timezone | true | Timezone that you want to view tasks in, remember tasks rollover at midnight local in Sunsama
date | true | Date that you want tasks for, should be in ISO8601 forma


### HTTP Request

`POST http://sunsama.com/api/v0/`

### URL Parameters

Parameter | Required | Type |Description
--------- | ----------- | ----------- | -----------
text | true | String | The main title of the task (this is what shows in the Kanban view).
createdBy | true | String (email) | The e-mail address of the user who is creating the task. 
notes | false | String (html) | This is the "description" that shows up when the Kanban card is opened up. 
assignee | false | String (email) | The e-mail address of the user who the task is assigned to. Tasks without an assignee show up under the creator's column until there's an assignee
startDate | false | String (ISO8601) | The first Kanban day column this task should show up on. Must be ISO-8601 and in the UTC timezone. If not specified, the task will end up in the backlog. 
dueDate | false | String (ISO8601) | The due date of the task. Must be ISO-8601 in the UTC timezone. 
channelName | false | String | The name of the channel that the task should be assigned to. Defaults to "no channel" also known #all. 
externalResourceId | false | String | String identifier that can be provided when creating a task, can be used to reference your task later. Typically you'd use this to link a Sunsama task to a source record in your systems. The value must be unique within your workspace. 
url | false | String | The URL that should be displayed inside this task, provide this if you want a quick way for users to access some source item. 
completeDate | false | String (ISO8601) | The date on which this task was completed. 
completedBy | false | String | The email address of the user that completed the task, must be specified if the `completeDate` is specified. 

## Modify a task

Pass in a `id` or `externalResourceId` to identify a task and then pass any other fields to modify them, fields that are not passed are not changed. You can pass the value `null` as a value for any query parameter in order to remove that fields e.g. to remove a due date for example. 

```javascript
var options = {
    method: 'PATCH', 
    uri: 'https://sunsama.com/api/v0/tasks',
    qs: {
        id: 'task-id',
        text: 'a new title', 
    },
    headers: {
        'Authorization': 'workspace-api-key',
        'Content-Type': 'application/json',
    },
    json: true // Automatically parses the JSON string in the response
};

rp(options)
    .then((task) => console.log(task))
    .catch(function (err) {
        // API call failed...
    });
```

> The above command returns JSON structured like this:

```json
{ "status": "success" }
```

This endpoint modifies a single task. 

### HTTP Request

`PATCH https://sunsama.com/api/v0/tasks`

### URL Parameters

Parameter | Required | Type |Description
--------- | ----------- | ----------- | -----------
id | must specify this or externalResourceId | String | The id of the task resource (cannot be modified). 
externalResourceId | must specify this or id | String | The externalResourceId of the task that was specified when it was created (cannot be modified)
text | false | String | The main title of the task (this is what shows in the Kanban view).
notes | false | String (html) | This is the "description" that shows up when the Kanban card is opened up. It is not recommended to modify this with the API because it will overwrite existing notes i.e. this doesn't play nice with collaborative notes. 
assignee | false | String (email) | The e-mail address of the user who the task is assigned to. Tasks without an assignee show up under the creator's column until there's an assignee
startDate | false | String (ISO8601) | The first Kanban day column this task should show up on. Must be ISO-8601 and in the UTC timezone. If not specified, the task will end up in the backlog. 
dueDate | false | String (ISO8601) | The due date of the task. Must be ISO-8601 in the UTC timezone. 
channelName | false | String | The name of the channel that the task should be assigned to. Defaults to "no channel" also known #all. 
completeDate | false | String (ISO8601) | The date on which this task was completed. 
completedBy | false | String | The email address of the user that completed the task, must be specified if the `completeDate` is specified. 