---
title: UCL API Reference

language_tabs:
  - shell : Shell
  - shell--shellv1: Shell v1
  - python : Python 
  - python--pythonv1 : Python v1
  - javascript : JavaScript
  - javascript--javascriptv1 : JavaScript v1

toc_footers:
  - <a href='https://uclapi.com/dashboard/'>Get an API Key</a>

search: true
---
# Room Bookings

Yay, you made it 🎉

The base url is `https://uclapi.com/roombookings/`

By default every request is sent to the latest version of the API, which is **Version 1**. You should aim to write all your code against the latest version of the API. However, you should always specific in the header which version you would like to fetch data from. This is so that when the API is updated in the future you will not be affected by breaking changes.

The header variable you can set for Room Bookings is `uclapi-roombookings-version: 1.0`.

Please note that this is different behaviour to the Engineering Hub API; in that system the API version is specified explicitly in the URL. It is not necessary to specify a version when experimenting, but it is _highly_ recommended that you specify the target version in any production code as this will protect you against any breaking changes in the API as it is developed.
Each of the languages includes a `v1` option which will show you how to include this header in your request.

# Get Rooms
**Endpoint:** `https://uclapi.com/roombookings/rooms`

This endpoint returns rooms and information about them.
If you don't specify any query parameters besides the `token`, all rooms will be returned.  
_Note: This endpoint only returns publicly bookable rooms. Departmentally bookable rooms are not included._

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```shell--shellv1
curl -H "uclapi-roombookings-version: 1.0" https://uclapi.com/roombookings/rooms?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```python
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"
}
r = requests.get("https://uclapi.com/roombookings/rooms", params=params)
print(r.json())
```

```python--pythonv1
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"
}
headers = {
  "uclapi-roombookings-version": "1.0"
}
r = requests.get("https://uclapi.com/roombookings/rooms", params=params, headers=headers)
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

```javascript--javascriptv1
var myHeaders = new Headers();
myHeaders.append("uclapi-roombookings-version", "1.0");
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

Parameter | Example | Required | Description
--------- | ---------- | ----------- | -----------
`token` | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token
`roomid` | `433` | Optional | The room id (not to be confused, with the room name).
`roomname` | `Torrington (1-19) 433`| Optional | The name of the room. It often includes the name of the site (building) as well.
`siteid` | `086`| Optional | Every room is inside a site (building). All sites have ids.
`sitename` | `Torrington Place, 1-19`| Optional | Every site (building) has a name. In some cases this is contained in the room name as well.
`classification` | `CR`| Optional | The type of room. `LT` = Lecture Theatre, `CR` = Classroom, `SS` = Social Space, `PC1` = Public Cluster
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
`roomid` | `Z4` | The room id (not to be confused with the room name)
`siteid` | `005`| Every room is inside a site (building). All sites have ids.
`sitename` | `Main Building` | The name of the site (building).
`capacity` | `50` | The number of people that can fit in the room.
`classification` | `SS`| The type of room. `LT` = Lecture Theatre, `CR` = Classroom, `SS` = Social Space, `PC1` = Public Cluster
`automated` | `N` | Whether bookings in this room will be confirmed automatically. `A` stands for automated, and `N` for not automated. `P` represents that the confirmation will be automatic, but only under certain circumstances.
`location` | - | Contains an object with the key `address`, whose value is an array of address information, which when combined will make up a complete address.

### Errors

Error | Description
--------- | ---------
`No token provided` | Gets returned when you have not supplied a `token` in your request.
`Token does not exist` | Gets returned when you supply an invalid `token`.


#Get Bookings
## Regular request

**Endpoint:** `https://uclapi.com/roombookings/bookings`

This endpoint shows the results to a bookings or space availability query. It returns a paginated list of bookings.
_Note: This endpoint only returns publicly displayed bookings. Departmental bookings are not included._

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&contact=Mark
```

```shell--shellv1
curl -H "uclapi-roombookings-version: 1.0" https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&contact=Mark
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

```python--pythonv1
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb",
  "contact": "Mark"
}
headers = {
  "uclapi-roombookings-version": "1.0"
}
r = requests.get("https://uclapi.com/roombookings/bookings", params=params, headers=headers)
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

```javascript--javascriptv1
var myHeaders = new Headers();
myHeaders.append("uclapi-roombookings-version", "1.0");
var initData = {
  method: 'GET',
  headers: myHeaders,
  mode: 'cors',
  cache: 'default'
};
fetch("https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&contact=Mark", initData)
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

Parameter | Example | Required | Description
---|---|---|---
token | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token
roomid |  `433`  |  Optional  | The room id (not to be confused with the room name)
start_datetime | `2011-03-06T03:36:45+00:00` | Optional | Start datetime of the booking. Returns bookings with a `start_datetime` after the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
end_datetime | `2011-03-06T03:36:45+00:00` | Optional | End datetime of the booking. Returns bookings with an `end_datetime` before the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
date | `20160202` | Optional | Date of the bookings you need. Returns bookings occuring on this day. **This query parameter is only considered when `end_datetime` and `start_datetime` are not supplied.**
siteid | `086` | Optional | Every room is inside a site (building). All sites have ids.
description | `Lecture` | Optional | Describes what the booking is. Could contain a module code (for example `WIBRG005`) or just the type of activity (for example `Lecture`).
contact | `Mark Herbster` | Optional | The name of the person who made the booking. Can substrings of the contact name: Queries for `Mark` will include `Mark Herbster`.
results_per_page | `50` | Optional | Number of bookings returned per page. Maximum allowed value is `100`. Defaults to `20`.


### Response


```json
{
    "ok": true,
    "next_page_exists": true,
    "page_token": "6hb14hXjRV",
    "count": 1197,
    "bookings": [
      {  
          "slotid": 983481,
          "contact": "Ms Leah Markwick",
          "start_time": "2016-09-01T09:00:00+00:00",
          "end_time": "2016-09-01T18:00:00+00:00",
          "roomid": "433",
          "roomname": "Torrington (1-19) 433",
          "weeknumber": 1,

          "phone": "45699",
          "description": "",


       },
       ...
    ]
}

```


Field | Example | Description
--------- | ---------- | -----------
ok | `true` | Whether the request was successful
bookings | - | An array of booking objects.
next_page_exists | `true` | True if there is another page with more bookings.
page_token | `6hb14hXjRV` | Page token parameter that needs to be supplied to view subsequent pages. Only included when the next page exists.

The bookings array contains booking objects, which each have the following properties.

Booking Property | Example | Description
--------- | ---------- | -----------
slotid | `1024991` | An id for the booking. Combined with `weeknumber` they can be assumed to be almost unique.
contact | `Wilhelm Klopp - UCLU Tech Society` | Name of the person who made the booking
start_time | `2016-10-20T18:00:00+00:00` | Start datetime of the booking. Returns bookings with a `start_datetime` after the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
end_time | `2016-10-20T19:00:00+00:00` | End datetime of the booking. Returns bookings with an `end_datetime` before the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
roomid | `B305` | The room ID (not to be confused with the room name)
roomname | `Cruciform Building B.3.05` | The name of the room. It often includes the name of the site (building) as well.
siteid | `044` | Every room is inside a site (building). All sites have ids.
weeknumber | `8` | The week the booking is in.
phone | `45699` | Phone number (UCL extension)

## Paginated request
**Endpoint:** `https://uclapi.com/roombookings/bookings`

Paginated requests also go to the `/bookings` endpoint, but other than the regular requests only the `token` and `page_token` should be supplied. All other filters are "remembered" from the initiating regular request.

The API remembers how many requests you've made, so to move through the pages you can keep making the same request (with the same `token` and `page_token`) until the response contains `false` in the `next_page_exists` field.

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&page_token=6hb14hXjRV
```

```shell--shellv1
curl -H "uclapi-roombookings-version: 1.0" https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&page_token=6hb14hXjRV
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

```python--pythonv1
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb",
  "page_token": "6hb14hXjRV"
}
headers = {
  "uclapi-roombookings-version": "1.0"
}
r = requests.get("https://uclapi.com/roombookings/bookings", params=params, headers=headers)
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

```javascript--javascriptv1
var myHeaders = new Headers();
myHeaders.append("uclapi-roombookings-version", "1.0");
var initData = {
  method: 'GET',
  headers: myHeaders,
  mode: 'cors',
  cache: 'default'
};
fetch("https://uclapi.com/roombookings/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&page_token=6hb14hXjRV", initData)
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
`page_token` | `6hb14hXjRV` | Required | Page token received in regular `/bookings` request. The page token does not change as you paginate through the results.

### Response

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


# Get Equipment
**Endpoint:** `https://uclapi.com/roombookings/equipment`

This endpoint returns equipment/feature information about a specific room. So, for example whether there is a `Whiteboard` or a `DVD Player` in the room. A full example can be seen [here.](https://roombooking.ucl.ac.uk/rb/bookableSpace/roomInfo.html?room=B15B&building=016&invoker=EFD)

You _need_ to supply a `token`, `roomid`, and `siteid` to get a response.

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```shell--shellv1
curl -H "uclapi-roombookings-version: 1.0" https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```python
import requests

r = requests.get("https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb")
print(r.json())
```

```python--pythonv1
import requests

params = {
  "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"
}
headers = {
  "uclapi-roombookings-version": "1.0"
}
r = requests.get("https://uclapi.com/roombookings/equipment", params=params, headers=headers)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb")
.then((response) => {
  return response.json()
})
.then((json) => {
  console.log(json);
  })
```

```javascript--javascriptv1
var myHeaders = new Headers();
myHeaders.append("uclapi-roombookings-version", "1.0");
var initData = {
  method: 'GET',
  headers: myHeaders,
  mode: 'cors',
  cache: 'default'
};
fetch("https://uclapi.com/roombookings/equipment?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb", initData)
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
`roomid` | `433` | Required | The room id (not to be confused with the room name)
`siteid` | `086`| Required | Every room is inside a site (building). All sites have ids.

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

# Contributing
These docs are open sourced at [https://github.com/uclapi/apiDocs](https://github.com/uclapi/apiDocs).

Any and all contributions are welcome! If you spot a type or error, fix it :)

