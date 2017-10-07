---
title: UCL API Reference

language_tabs:
  - shell : Shell
  - python : Python
  - javascript : JavaScript

toc_footers:
  - <a href='https://uclapi.com/dashboard/'>Get an API Key</a>

search: true
---
# Welcome
Yay, you made it! 🎉

Welcome to the documentation for UCL's brand new open API. Each service is listed below with examples in three different programming languages (Shell script using cURL, Python and JavaScript) to help you get going as quickly as possible.

The API will be made up of several services, each of which will be separately versioned and explained. Every service will be documented here with important information, tips and examples.

## Get Your API Key
Before you can use the API you should head to the [API Dashboard](https://uclapi.com/dashboard) and sign up using your UCL user account. Once logged into the dashboard simply name your app and you'll be given a key that you can use to access all the services. Simples!

## API Rate Limits

Rate limiting of the API is primarily on a per-user basis. The limit is calculated against user's ID (e-mail) across all access tokens.

The limit is 10 000 requests per day and resets every day midnight London time. (00:00 GMT)

When a request is throttled, the response returned by the server has HTTP Status Code "429 Too Many Requests". It includes a `Retry-After` header with the number of seconds the client is throttled for.

If you would like your rate limit to be increased, contact us at isd.apiteam@ucl.ac.uk

# Version Information

Each service has a header named `uclapi-servicenamehere-version` which you can add to your requests, where `servicenamehere` is the endpoint name for the service you are using. For example, for the Room Bookings service you would set the `uclapi-roombookings-version` header.

```shell
curl -H "uclapi-roombookings-version: 1" https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"
}
headers = {
  "uclapi-roombookings-version": "1"
}
r = requests.get("https://uclapi.com/roombookings/rooms", params=params, headers=headers)
print(r.json())
```

```javascript
var myHeaders = new Headers();
myHeaders.append("uclapi-roombookings-version", "1");
var initData = {
  method: 'GET',
  headers: myHeaders,
  mode: 'cors',
  cache: 'default'
};
fetch("https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb", initData)
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

<aside class="warning">
  <strong>Really Important Warning</strong>
  <hr>
  <ul>
    <li>By default, every UCL API request gets sent to the <strong>latest version</strong> of the service you requested.</li>
    <li>This is okay for test purposes.</li>
    <li>If you are deploying into production you must follow the versioning instructions to ensure you are not affected by breaking changes.</li>
  </ul>
</aside>

On the right hand side you'll see a code sample with the version data added. We are specifying that version `1` of the `roombookings` service should be used for the request URL `https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb`. You should amend the example accordingly for the service and API version you are using.

# OAuth

This is a quick guide to OAuth support in UCL API for developers. OAuth is a protocol that lets external apps request secure access to private UCL account data without getting your password. This can be done with a "Sign In With UCL" button on your website which avoids UCL users from needing to set up another account username and password. It also allows you as a developer to, for example, get a user's personal timetable.

Sounds intriguing? Demo of "Sign In With UCL" is located [here](https://uclapi-oauth-demo.glitch.me).

[![Remix on Glitch](https://cdn.glitch.com/2703baf2-b643-4da7-ab91-7ee2a2d00b5b%2Fremix-button.svg)](https://glitch.com/edit/#!/remix/uclapi-oauth-demo)

## Sign In With UCL Button

If you want to add a "Sign In With UCL" button to your website, which looks like this:

![](https://s3.eu-west-2.amazonaws.com/uclapi-static/SignInWithUCLSmall.png)

you can copy the following the following code:

```plaintext
<a href="https://uclapi.com/oauth/authorise?client_id=CLIENT_ID&state=STATE">
<img src="https://s3.eu-west-2.amazonaws.com/uclapi-static/SignInWithUCLSmall.png">
</a>
```

where `CLIENT_ID` and `STATE` should be replaced by `client_id` of your app and a random `state`.

## Scopes

OAuth scopes specify how your app needs to access a UCL user's account. As an app developer, you set the desired scopes in the [API Dashboard](https://uclapi.com/dashboard). When a user is responding to your authorisation request, the requested scopes will be displayed to the user.

## OAuth Workflow

If your application wants to use OAuth, you **must** set a callback URL in the dashboard. Then the app should follow this procedure:

1. Send a request to `https://uclapi.com/oauth/authorise` with `state` and the application's `client_id`.

2. The user will need to log in with their UCL credentials on the UCL Single Sign-on website (if not logged in already).

3. The user will be allowed to either authorise or deny the application, based on the OAuth scope requested. If the application is authorised, the callback URL receives `client_id`, `state` (specified in 1.), `result`, and `code`.

If the application is denied, the callback URL receives `result` and `state`, and no private data will be provided to the application.

4. To receive the OAuth user token (for performing actions on user's behalf), we require `code` (from 3.), `client_id`, and `client_secret`. These should then be set to `https://uclapi.com/oauth/token`, which will return a response containing `state`, `ok`, `client_id`, `token` (OAuth user token), and `scope` (scopes the app can access on the user's behalf).

## Authorise
**Endpoint:**

The base URL is: `https://uclapi.com/oauth/authorise`

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/oauth/authorise/?client_id=123.456&state=1
```

```python
import requests

params = {
  "client_id": "123.456",
  "state": "1",
}
r = requests.get("https://uclapi.com/oauth/authorise", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/oauth/authorise/?client_id=123.456&state=1")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
})
```

Parameter | Example | Required | Description
--------- | ------- | -------- | -----------
`client_id` | `123.456` | Required | Client ID of the authenticating app.
`state` | `1` | Required | OAuth state.

### Response

Redirection to authorise page.

### Errors

Error | Description
--------- | ---------
`Incorrect parameters supplied.` | Gets returned when you have not supplied a `client_id` and a `state` in your request.
`App does not exist for client id.` | Gets returned when you supply an invalid `client_id`.
`No callback URL set for this app.` | Gets returned when you have not set a callback URL for your app.

## Token

**Endpoint:**

The base URL is: `https://uclapi.com/oauth/token`

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/oauth/token?code=mysecretcode&client_id=123.456&client_secret=secret
```

```python
import requests

params = {
  "client_id": "123.456",
  "code": "1",
  "client_secret": "secret",
}
r = requests.get("https://uclapi.com/oauth/token", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/oauth/token?code=mysecretcode&client_id=123.456&client_secret=secret")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
})
```

Parameter | Example | Required | Description
--------- | ------- | -------- | -----------
`client_id` | `123.456` | Required | Client ID of the authenticating app.
`code` | `mysecretcode` | Required | Secret code obtained from the `authorise` endpoint.
`client_secret` | `mysecret` | Required | Client secret of the authenticating app.

### Response

```json
{
    "scope": "[]",
    "state": "1",
    "ok": true,
    "client_id": "123.456",
    "token": "uclapi-user-abc-def-ghi-jkl",
}
```

### Errors

Error | Description
--------- | ---------
`The client did not provide requisite data to get the token.` | Gets returned when you have not supplied a `client_id`, `code`, `client_secret` in your request.
`The code received was invalid, or has expired. Please try again.` | As error message.
`Client secret incorrect.` | Gets returned when the client secret was incorrect.

## User Data

**Endpoint:**

The base URL is: `https://uclapi.com/oauth/user/data`

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/oauth/user/data?client_secret=secret&token=uclapi-user-abd-def-ghi-jkl
```

```python
import requests

params = {
  "token": "uclapi-user-abd-def-ghi-jkl",
  "client_secret": "secret",
}
r = requests.get("https://uclapi.com/oauth/token", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/oauth/user/data?client_secret=secret&token=uclapi-user-abd-def-ghi-jkl")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
})
```

Parameter | Example | Required | Description
--------- | ------- | -------- | -----------
`token` | `uclapi-user-abd-def-ghi-jkl` | Required | OAuth user token.
`client_secret` | `mysecret` | Required | Client secret of the authenticating app.

### Response

```json
{
    "department": "Dept of Department",
    "email": "xxxxxxx@ucl.ac.uk",
    "ok": true,
    "full_name": "Full Name",
    "cn": "xxxxxxx",
    "given_name": "Full",
    "upi": "fname12",
    "scope_number": 0,
}
```

User Data Property | Example |  Description
--------- | ---------- |  -----------
`department` | `Dept Of Computer Science` | Department the user belongs to.
`email` | `zcabmrk@ucl.ac.uk` | E-mail for the given user.
`ok` | `true` | Returns if the query was successful.
`full_name` | `Martin Mrkvicka` | Full name of the user.
`cn` | `.` | No idea.
`given_name` | `Martin` | Given first name of the user.
`upi` | `mmrkv12` | Unique Person Identifier.
`scope_number` | `0` | Scopes the application has access to on behalf of the user.

### Errors

Error | Description
--------- | ---------
`Token does not exist.` | Gets returned when `token` does not exist.
`Client secret incorrect.` | Gets returned when the client secret was incorrect.


# Room Bookings

The base url is `https://uclapi.com/roombookings/`

Version Header | Latest Version
-------------- | --------------
`uclapi-roombookings-version` | `1`

## Get Rooms
**Endpoint:** `https://uclapi.com/roombookings/rooms`

This endpoint returns rooms and information about them.
If you don't specify any query parameters besides the `token`, all rooms will be returned.  
_Note: This endpoint only returns publicly bookable rooms. Departmentally bookable rooms are not included._

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"
}
r = requests.get("https://uclapi.com/roombookings/rooms", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

Parameter | Example | Required | Description
--------- | ------- | -------- | -----------
`token`   | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token.
`roomname` | `Torrington (1-19) 433`| Optional | The name of the room. It often includes the name of the site (building) as well.
`roomid` | `433` | Optional | The room ID (not to be confused, with the `roomname`).
`siteid` | `086`| Optional | Every room is inside a site (building). All sites have IDs.
`sitename` | `Torrington Place, 1-19`| Optional | Every site (building) has a name. In some cases this is contained in the `roomname` as well.
`classification` | `CR`| Optional | The type of room. `LT` = Lecture Theatre, `CR` = Classroom, `SS` = Social Space, `PC1` = Public Cluster.
`capacity` | `55`| Optional | Every room has a set capacity of how many people can fit inside it. When supplied, all rooms with the given capacity or greater will be returned.

### Response

```json
{
  "ok": true,
  "rooms": [
    {
      "roomname": "Wilkins Building (Main Building) Portico",
      "roomid": "Z4",
      "siteid": "005",
      "sitename": "Main Building",
      "capacity": 50,
      "classification": "SS",
      "automated": "N",
      "location": {
        "coordinates": {
          "lat": "51.524699",
          "lng": "-0.13366"
        },
        "address": [
          "Gower Street",
          "London",
          "WC1E 6BT",
          ""
        ]
      }
    }
    ...
  ]
}
```

The room field contains a list of rooms that match your query. If no filters are applied, all rooms will be returned.

Room Property | Example |  Description
--------- | ---------- |  -----------
`roomname` | `Wilkins Building (Main Building) Portico` | The name of the room. It often includes the name of the site (building) as well.
`roomid` | `Z4` | The room ID (not to be confused with the `roomname`).
`siteid` | `005`| Every room is inside a site (building). All sites have IDs.
`sitename` | `Main Building` | The name of the site (building).
`capacity` | `50` | The number of people that can fit in the room.
`classification` | `SS`| The type of room. `LT` = Lecture Theatre, `CR` = Classroom, `SS` = Social Space, `PC1` = Public Cluster.
`automated` | `N` | Whether bookings in this room will be confirmed automatically. `A` stands for automated, and `N` for not automated. `P` represents that the confirmation will be automatic, but only under certain circumstances.
`location` | - | Contains an object with two keys `address`, and `coordinates`. `address` contains an array of address information, which when combined will make up a complete address. <br /> `coordinates` contains a `lat` and `lng` key with the latitude and longitude of the room.

### Errors

Error | Description
--------- | ---------
`No token provided` | Gets returned when you have not supplied a `token` in your request.
`Token does not exist` | Gets returned when you supply an invalid `token`.


## Get Bookings

### Regular Request

**Endpoint:** `https://uclapi.com/roombookings/bookings`

This endpoint shows the results to a bookings or space availability query. It returns a paginated list of bookings.
_Note: This endpoint only returns publicly displayed bookings. Departmental bookings are not included._

**Allowed request type:** `GET`

#### Query Parameters

```shell
curl https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&contact=Mark
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb",
  "contact": "Mark"
}
r = requests.get("https://uclapi.com/roombookings/bookings", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&contact=Mark")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

Parameter | Example | Required | Description
---|---|---|---
token | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token.
roomname | `Cruciform Building B.3.05` | Optional | The name of the room. It often includes the name of the site (building) as well.
roomid |  `433`  |  Optional  | The room ID (not to be confused with the `roomname`).
start_datetime | `2011-03-06T03:36:45+00:00` | Optional | Start datetime of the booking. Returns bookings with a `start_datetime` after the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
end_datetime | `2011-03-06T03:36:45+00:00` | Optional | End datetime of the booking. Returns bookings with an `end_datetime` before the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
date | `20160202` | Optional | Date of the bookings you need, in the format *YYYYMMDD*. Returns bookings occurring on this day. **This query parameter is only considered when `end_datetime` and `start_datetime` are not supplied.**
siteid | `086` | Optional | Every room is inside a site (building). All sites have IDs.
description | `Lecture` | Optional | Describes what the booking is. Could contain a module code (for example `WIBRG005`) or just the type of activity (for example `Lecture`).
contact | `Mark Herbster` | Optional | The name of the person who made the booking. Substrings of the contact name can be used: Queries for `Mark` will include `Mark Herbster`. Sometimes, a society or student group may be the contact for a booking.
results_per_page | `50` | Optional | Number of bookings returned per page. Maximum allowed value is `1000`. Defaults to `1000`.


#### Response


```json
{
   "ok": true,
   "bookings": [
      {  
          "slotid": 998503,
          "end_time": "2016-09-02T18:00:00+00:00",
          "description": "split weeks to assist rooming 29.06",
          "roomname": "Torrington (1-19) 433",
          "siteid": "086",
          "contact": "Ms Leah Markwick",
          "weeknumber": 1,
          "roomid": "433",
          "start_time": "2016-09-02T09:00:00+00:00",
          "phone": "45699"
       },
       ...
    ],
    "next_page_exists": true,
    "page_token": "6hb14hXjRV",
    "count": 1197
}

```


Field | Example | Description
--------- | ---------- | -----------
ok | `true` | Boolean indicating whether the request was successful.
bookings | - | An array of booking objects.
next_page_exists | `true` | True if there is another page containing more bookings.
page_token | `6hb14hXjRV` | Page token parameter that needs to be supplied to view subsequent pages. Only included when the next page exists.
count | `1197` | Total number of bookings matching the query. The `count` field will only be in the first response to a query

The bookings array contains booking objects, which each have the following properties.

Booking Property | Example | Description
--------- | ---------- | -----------
slotid | `1024991` | An ID for the booking. Combined with `weeknumber` they can be assumed to be almost unique.
contact | `Wilhelm Klopp - UCLU Tech Society` | Name of the person (and society/student group, if applicable) who made the booking.
start_time | `2016-10-20T18:00:00+00:00` | Start datetime of the booking. Returns bookings with a `start_datetime` after the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
end_time | `2016-10-20T19:00:00+00:00` | End datetime of the booking. Returns bookings with an `end_datetime` before the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
roomname | `Cruciform Building B.3.05` | The name of the room. It often includes the name of the site (building) as well.
roomid | `B305` | The room ID (not to be confused with the `roomname`).
siteid | `044` | Every room is inside a site (building). All sites have IDs.
weeknumber | `8` | The week the booking is in.
phone | `45699` | Phone number of the room (UCL extension).
description |	`Lecture` |	Describes what the booking is. Could contain a module code (for example WIBRG005) or just the type of activity (for example Lecture).

### Paginated Request
**Endpoint:** `https://uclapi.com/roombookings/bookings`

Paginated requests also go to the `/bookings` endpoint but, unlike the regular requests, only the `token` and `page_token` should be supplied. All other filters are "remembered" from the initiating regular request.

The API "remembers" how many requests you've made, so to move through the pages you can keep making the same request (with the same `token` and `page_token`) until the response's `next_page_exists` field is `false`.

**Allowed request type:** `GET`

#### Query Parameters

```shell
curl https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&page_token=6hb14hXjRV
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb",
  "page_token": "6hb14hXjRV"
}

r = requests.get("https://uclapi.com/roombookings/bookings", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&page_token=6hb14hXjRV")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

Parameter | Example | Required | Description
--------- | ---------- | ----------- | -----------
`token` | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token
`page_token` | `6hb14hXjRV` | Required | Page token received in initial `/bookings` request. The page token does not change as you paginate through the results.

#### Response

```json
{
    "ok": true,
    "page_token": "wmCUiJgItq",
    "next_page_exists": true,
    "bookings": [
      {
        "roomname": "Christopher Ingold Building XLG2 Auditorium",
        "slotid": 733044,
        "siteid": "067",
        "start_time": "2017-03-20T13:00:00+00:00",
        "roomid": "XLG2",
        "weeknumber": 30,
        "description": "",
        "end_time": "2017-03-20T14:00:00+00:00",
        "phone": "",
        "contact": "Dr Mark Roberts"
      },
      ...
    ]
}
```

The response to a paginated request is identical to that of a regular request, except that no `count` field is provided.


### Errors
Error                 | Description
----------------------| -----------
`No token provided`   | Gets returned when you have not supplied a `token` in your request.
`Token does not exist`| Gets returned when you supply an invalid `token`.
`date/time isn't formatted as suggested in the docs`  | Passed datetime parameter does not conform to the ISO8601 format
`results_per_page should be an integer`  | `results_per_page` should always be an integer.
`Page token does not exist` | The passed `page_token` parameter isn't a valid one.

## Get Equipment
**Endpoint:** `https://uclapi.com/roombookings/equipment`

This endpoint returns any equipment/feature information about a specific room. So, for example whether there is a `Whiteboard` or a `DVD Player` in the room. A full example can be seen [here.](https://roombooking.ucl.ac.uk/rb/bookableSpace/roomInfo.html?room=B15B&building=016&invoker=EFD)

You _need_ to supply a `token`, `roomid`, and `siteid` to get a response.

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&roomid=433&siteid=086
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb",
  "roomid": "433"
  "siteid": "086"
}

r = requests.get("https://uclapi.com/roombookings/equipment", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&roomid=433&siteid=086")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

Parameter | Example | Required | Description
--------- | ------- | -------- | -----------
`token` | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token
`roomid` | `433` | Required | The room ID (not to be confused with the `roomname`)
`siteid` | `086`| Required | Every room is inside a site (building). All sites have IDs.

### Response

```json
{
  "ok": true,
  "equipment": [
    {
      "type": "FF",
      "description": "Managed PC",
      "units": 1
    },
    {
      "type": "FE",
      "description": "Chairs with Tables",
      "units": 1
    },
    ...
  ]
}
```
The equipment field contains a list of equipment items. This list can have a different length depending on the room, and it can also be empty.

Each equipment item contains a `type`, a `description`, and the number of `units`.

Equipment Item | Example      |  Description
-------------- | ------------ |  -----------
`type`         | `FE`         | The type of equipment. Either Fixed Equipment (`FE`) or Fixed Feature (`FF`).
`description`  | `Managed PC` | What the piece of equipment actually is.
`units`        | `1`          | The number of times this piece of equipment exists in the room.

### Errors

Error                 | Description
----------------------| -----------
`No token provided`   | Gets returned when you have not supplied a `token` in your request.
`Token does not exist`| Gets returned when you supply an invalid `token`.
`No roomid supplied`  | Gets returned when you don't supply a `roomid`.
`No siteid supplied`  | Gets returned when you don't supply a `siteid`.


# Search

The base url is `https://uclapi.com/search/`

Version Header | Latest Version
-------------- | --------------
`uclapi-search-version` | `1`

## Get People
**Endpoint:** `https://uclapi.com/search/people`

This endpoint returns matching people and information about them.

_Note: This endpoint only returns a maximum of 20 matches._

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/search/people \
-d token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
-d query='Jane'
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb",
  "query": "Jane"
}
r = requests.get("https://uclapi.com/search/people", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&query=Jane")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
})
```

Parameter | Example | Required | Description
--------- | ------- | -------- | -----------
`token`   | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token.
`query` | `Jane Doe`| Required | Name of the person you are searching for

### Response

```json
{
  "ok": true,
  "people": [
    {
      "name": "Jane Doe",
      "status": "Student",
      "department": "Dept of Med Phys & Biomedical Eng",
      "email": "jane.doe.17@ucl.ac.uk"
    },
    ...
  ]
}
```

The people field contains a list of people that match your query.

Person Attribute | Example |  Description
--------- | ---------- |  -----------
`name` | `Jane Doe` | The name of the person.
`status` | `Staff` | Tell's us whether the person is a student of member of staff.
`department` | `UCL Medical School`| The department the student studies or works under.
`email` | `jane.doe.17@ucl.ac.uk` | The email of the person.

### Errors

Error | Description
--------- | ---------
`No token provided` | Gets returned when you have not supplied a `token` in your request.
`Token does not exist` | Gets returned when you supply an invalid `token`.
`No query provided` | Gets returned when you have not supplied a `query` in your request.


# Get Involved
This documentation is all open sourced at [https://github.com/uclapi/apiDocs](https://github.com/uclapi/apiDocs).

The full API is open sourced at [https://github.com/uclapi/uclapi](https://github.com/uclapi/uclapi).

Any and all contributions are welcome! If you spot a typo or error, feel free to fix it and submit a pull request :)
