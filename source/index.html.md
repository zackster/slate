---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the WorkReduce API! You can use our API to access WorkReduce API endpoints, which can get information on various tasks and orders on our platform.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

<aside class="notice">
The WorkReduce API is versioned by release date.  If you are just getting started, we recommend using the most recently released version of the API.  If you are using one of our libraries for Python, Ruby, or Node.js, this will be handled for you automatically.<br /><br />

If you are rolling your own connection via curl, you will need to ensure the URL endpoints contain the API release version you are using.<br /><br />

For example, requests to "https://api.workreduce.com/v1/orders" would go to version 1 of the API.
</aside>

API Version | Release date
--------- | -------
v1 | 05-25-2018




# Authentication

> To authorize, use this code:

```ruby
require 'workreduce'

api = WorkReduce::APIClient.authorize!('meowmeowmeow')
```

```python
import workreduce

api = workreduce.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const workreduce = require('workreduce');

let api = workreduce.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

WorkReduce uses API keys to allow access to the API. You can register a new WorkReduce API key at our [developer portal](#).

WorkReduce expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Orders

## Get All Open Orders

```ruby
require 'workreduce'

api = WorkReduce::APIClient.authorize!('meowmeowmeow')
api.orders.get
```

```python
import workreduce

api = workreduce.authorize('meowmeowmeow')
api.orders.get()
```

```shell
curl "https://api.workreduce.com/v1/orders"
  -H "Authorization: meowmeowmeow"
```

```javascript
const workreduce = require('workreduce');

let api = workreduce.authorize('meowmeowmeow');
let orders = api.orders.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 00007847,
    "name": "QA Screenshot of Ad Creative - BigCo",
    "status": "Complete",
    "due_date": "2018-05-03T18:30:00Z",
    "requester_email": "george@big-agency.com",
    "team": "MENA launch ads",
    "description": "Please look at the tag code...",
    "attachments": [
      "https://s3.workreduce.com/bigagency/00007847_0.txt",
      "https://s3.workreduce.com/bigagency/00007847_1.png"
    ],
    "task_type": "qa_screenshot"
  },
  {
    "id": 00007848,
    "name": "Second QA Screenshot of Ad Creative - BigCo",
    "status": "Pending",
    "due_date": "2018-05-14T18:30:00Z",
    "requester_email": "george@big-agency.com",
    "team": "MENA launch ads",
    "description": "Another task for you.  Please perform the...",
    "attachments": [
      "https://s3.workreduce.com/bigagency/00007848_0.txt",
    ],
    "task_type": "qa_screenshot"
  }
]
```

This endpoint retrieves all open orders for your account.

### HTTP Request

`GET https://api.workreduce.com/v1/orders`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
whole_organization | false | If set to true, the result will include orders created by anyone in your organization.
team | false | If set to true, the result will include orders created by anyone in your team.

<aside class="success">
Remember — a happy WorkReduce API request is an authenticated request!
</aside>


## New Order

```ruby
require 'workreduce'

api = WorkReduce::APIClient.authorize!('meowmeowmeow')
api.orders.create(
  :name => 'This is the name for the order',
  :description => 'This is some longer description for the order',
  :due_date => '2018-05-14T18:30:00Z',
  :attachments => [
    'C:\Users\Public\Desktop\screenshot.jpg'
  ],
  :task_type => "qa_screenshot"
)
```

```python
import workreduce

api = workreduce.authorize('meowmeowmeow')
api.orders.create(
  name='This is the name for the order',
  description='This is some longer description for the order',
  due_date='2018-05-14T18:30:00Z',
  attachments=['C:\Users\Public\Desktop\screenshot.jpg'],
  task_type='qa_screenshot'
)
```

```shell
curl "https://api.workreduce.com/v1/orders" \
  -H "Authorization: meowmeowmeow" \
  -d name="This is the name for the order" \
  -d description="This is some longer description for the order" \
  -d due_date="2018-05-14T18:30:00Z" \
  -d task_type="qa_screenshot" \
  -F "attachments[]=@screenshot.jpg" -F "attachments[]=@screenshot2.gif"
```

```javascript
const workreduce = require('workreduce');

let api = workreduce.authorize('meowmeowmeow');
let order = api.orders.create({
  name: 'This is the name for the order',
  description: 'This is some longer description for the order',
  due_date: '2018-05-14T18:30:00Z',
  task_type: 'qa_screenshot',
  attachments: ['C:\Users\Public\Desktop\screenshot.jpg']
});
```

> The above command returns JSON structured like this:

```json
{
  "id": 00007848,
  "created" : "success"
}
```

This endpoint creates a new Order.

### HTTP Request

`POST https://api.workreduce.com/v1/orders`

### URL Parameters

Parameter | Description
--------- | -----------
name | The name for the Order
due_date | The due date of the Order, formatted according to ISO 8601
description | A description containing instructions for executing the Order
attachments | A list of files to attach to the Order
task_type | The task type of the order.  Get this from the list of codes of [task types available to your account](#list-all-task-types)


## Update An Existing Order

```ruby
require 'workreduce'

api = WorkReduce::APIClient.authorize!('meowmeowmeow')
api.orders.update(
  :id => '00007848',
  :name => 'This is the updated name for the order',
  :description => 'This is the updated desc for the order',
  :due_date => '2018-05-14T18:30:00Z',
  :attachments => [
    'C:\Users\Public\Desktop\new_attachment.jpg'
  ],
  :task_type => "qa_screenshot"
)
```

```python
import workreduce

api = workreduce.authorize('meowmeowmeow')
api.orders.update(
  id='00007848',
  name='This is the updated name for the order',
  description='This is the updated desc for the order',
  due_date='2018-05-14T18:30:00Z',
  attachments=['C:\Users\Public\Desktop\new_attachment.jpg'],
  task_type="qa_screenshot"
)
```

```shell
curl "https://api.workreduce.com/v1/orders" \
  -H "Authorization: meowmeowmeow" \
  -d id="00007848" \
  -d name="This is the updated name for the order" \
  -d description="This is the updated desc for the order" \
  -d due_date="2018-05-14T18:30:00Z" \
  -d task_type="qa_screenshot" \
  -F "attachments[]=@new_screenshot.jpg" -F "attachments[]=@new_screenshot2.gif"
```

```javascript
const workreduce = require('workreduce');

let api = workreduce.authorize('meowmeowmeow');
let order = api.orders.update({
  id: '00007848',
  name: 'This is the updated name for the order',
  description: 'This is the updated desc for the order',
  due_date: '2018-05-14T18:30:00Z',
  task_type: 'qa_screenshot',
  attachments: ['C:\Users\Public\Desktop\new_screenshot.jpg']
});
```

> The above command returns JSON structured like this:

```json
{
  "id": 00007848,
  "updated" : "success"
}
```

### HTTP Request

`PUT https://api.workreduce.com/v1/orders/<ID>`

### URL Parameters

Parameter | Required | Description
--------- | ----------- | -----------
id | Required | The ID of the Order to update
name | Optional | The new name of the Order
description | Optional| The new description of the Order
due_date | Optional| The new due date for the Order.  ISO 8601.
attachments | Optional | Any new attachments for the Order.  Does not delete old attachments.
task_type | Optional | The new task type of the order.  Get this from the list of codes of [task types available to your account](#list-all-task-types)

### Notes

You must pass in at least one optional parameter or the API will return a [400 Error](#errors).

## Cancel An Existing Order

```ruby
require 'workreduce'

api = WorkReduce::APIClient.authorize!('meowmeowmeow')
api.orders.cancel('00007848')
```

```python
import workreduce

api = workreduce.authorize('meowmeowmeow')
api.orders.cancel('00007848')
```

```shell
curl "https://api.workreduce.com/v1/orders/00007848"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const workreduce = require('workreduce');

let api = workreduce.authorize('meowmeowmeow');
let order = api.orders.cancel('00007848');
```

> The above command returns JSON structured like this:

```json
{
  "id": 00007848,
  "deleted" : "success"
}
```

This endpoint deletes a specific Order.

### HTTP Request

`DELETE https://api.workreduce.com/v1/orders/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Order to delete





# Tasks

## List All Task Types

```ruby
require 'workreduce'

api = WorkReduce::APIClient.authorize!('meowmeowmeow')
api.tasks.get
```

```python
import workreduce

api = workreduce.authorize('meowmeowmeow')
api.tasks.get()
```

```shell
curl "https://api.workreduce.com/v1/tasks"
  -H "Authorization: meowmeowmeow"
```

```javascript
const workreduce = require('workreduce');

let api = workreduce.authorize('meowmeowmeow');
let tasks = api.tasks.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "code": "qa_screenshot",
    "description": "QA Screenshot of Ad Creative",
  },
  {
    "code": "recon",
    "description": "Reconciliation of quarterly ad spend",
  }
]
```

This endpoint retrieves all task types your account is authorized to request.

### HTTP Request

`GET https://api.workreduce.com/v1/tasks`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
whole_organization | false | If set to true, the result will include task types available to anyone in your organization.
team | false | If set to true, the result will include task types available to anyone in your team.
