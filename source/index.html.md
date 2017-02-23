---
title: UCL API Reference

language_tabs:
  - shell
  - python
  - javascript

toc_footers:
  - <a href='https://uclapi.com/dashboard/'>Get an API Key</a>

search: true
---
# Room Bookings

Yay, you made it 🎉



The base url is `https://uclapi.com/roombookings/`

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
--------- | ---------- | ----------- | -----------
`token` | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token
`roomid` | `433` | Optional | The room id (not to be confused with the room name).
`name` | `Torrington (1-19) 433`| Optional | The name of the room. It often includes the name of the site (building) as well.
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
      "name": "Wilkins Building (Main Building) Portico",
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
`name` | `Wilkins Building (Main Building) Portico` | The name of the room. It often includes the name of the site (building) as well.
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

**Endpoint:** `https://uclapi.com/roombookings/bookings`

This endpoint shows the results to a bookings or space availability query. It returns a paginated list of bookings.
_Note: This endpoint only returns publicly displayed bookings. Departmental bookings are not included._

**Allowed request type:** `GET`

Parameter | Example | Required | Description
---|---|---|---
token | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Required | Authentication token
roomid |  `433`  |  Optional  | The room id (not to be confused with the room name)
start_datetime | `2011-03-06T03:36:45+00:00` | Optional | Start datetime of the booking. Returns bookings with a start_datetime after the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
end_datetime | `2011-03-06T03:36:45+00:00` | Optional | End datetime of the booking. Returns bookings with an end_datetime before the one supplied. Follows the [ISO 8601 formatting standard](https://en.wikipedia.org/wiki/ISO_8601).
date | `20160202` | Optional | Date of the bookings you need. Returns bookings occuring on this day. This query parameter is only considered when `end_datetime` and `start_datetime` are not supplied.
siteid | `086` | Optional | Every room is inside a site (building). All sites have ids.
description | `Lecture` | Optional | Describes what the booking is. Could contain a module code like WIBRG005 or just the type of activity like Lecture.
contact | `Mark Herbster` | Optional | The name of the person who made the booking. Can substrings of the contact name: Queries for `Mark` will include `Mark Herbster`.
results_per_page | `50` | Optional | Number of bookings returned per page. Maximum allowed value is `100`. Defaults to `20`.

The API doesn't return all bookings at once, but instead uses *pagination* which can split results across multiple pages. You can specify the maximum number of results per page by supplying the `results_per_page` parameter. If you do not specify `results_per_page`, it will be set to `20` by default. The maximum accepted value is `100`.

An example of a query is given below:

`url = "https://uclapi.com/api/bookings?contact=Wilhelm&results_per_page=5&token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"`

This url has 3 parameters, `contact`, `token` and `results_per_page`.

`r = requests.get(url)`

Making this request will get us the data shown below:

`{
    "ok": true,
    "bookings": {
        "page_token": "TmJlhCJSUR",
        "bookings": [
            {
                "room": "Darwin Building B05",
                "contact": "Wilhelm Klopp - UCLU Tech Society",
                "start_time": "2016-12-07T00:00::00",
                "end_time": "2016-12-07T00:00::00",
                "description": "Meeting",
                "roomid": "B05A",
                "siteid": "044",
                "slotid": 1038241,
                "weeknumber": 15,
                "phone": ""
            },
            ...
        ],
        "count": 155,
        "next_page_exists": true
    }
}`

Value of the `ok` field tells if the request was successful.

Inside `bookings` we have every booking that fits the relevant filters, along with more information. The first 'page' will contain a `count` field which indicates the total number of results which fit your request. If there are more 'pages' to follow, the boolean field, `next_page_exists`, will be set to True.

To get the other pages in subsequent requests, you only have to pass the `page_token` in another request to the the same URL- no other parameters are required. Then you can make a request to the same endpoint with just the `page_token` and authentication token in `token`.

So the next request url will be:

`url = "https://uclapi.com/api/bookings?token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb&page_token=TmJlhCJSUR"`

The server will keep track of how many pages you have received, and how many you are yet to go through. If `page_token` is given in your request, any other parameter will be ignored. You make requests until the `next_page_exists` is `false`.


### Response

```json
{  
  "ok":true,
  "bookings": {  
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
          "weeknumber": 1,

          "siteid": "086",

          "phone": "45699",
          "description": "",
          "room": "Torrington (1-19) 433",


       },
       ...
    ]
  }
}
```

Field | Example | Description
--------- | ---------- | -----------
ok | `true` | Whether the request was successful
bookings | - | An array of booking objects.
next_page_exists | `true` | True if there is another page with more bookings.
page_token | `TmJlhCJSUR` | Page token parameter that needs to be supplied to view subsequent pages. Only included when the next page exists.


Booking Property | Example | Description
--------- | ---------- | -----------
slotid | `1024991` | A unique id for the booking
contact | `Wilhelm Klopp - UCLU Tech Society` | Name of the person who made the booking
start_time | `2016-10-20T18:00:00+00:00` | start time of the booking. ISO8601 formatted datetime string
end_time | `2016-10-20T19:00:00+00:00` | End time of the booking. ISO8601 formatted datetime string
roomid | `B305` | ID of the room
room | `Cruciform Building B.3.05` | Name of the room
weeknumber | `8` | Week number
phont | ` ` | Phone number

## Paginated request
**Endpoint:** `https://uclapi.com/roombookings/bookings`

This endpoint returns equipment information about a specific room.

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/equipment?page_token=TmJlhCJSUR&token=uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb
```

```python
import requests

params = {"page_token": "TmJlhCJSUR", "token": "uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb"}

r = requests.get("https://uclapi.com/roombookings/equipment", params=params)
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/equipment")
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
`page_token` | `TmJlhCJSUR` | Required | Page token you got from the response

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
The equipment field contains a list of equipment items. The length of this list can be different for every room.
It can also be 0.  
Each equipment item contains a `type`, a `description`, and the number of `units`.

Equipment Item | Example |  Description
--------- | ---------- |  -----------
`type` | `FE` | The type of equipment. Either Fixed Equipment (`FE`) or Fixed Feature (`FF`).
`description` | `Managed PC` | What the piece of equipment actually is.
`units` | `1`| The number of times this piece of equipment exists in the room.


# Get Equipment
**Endpoint:** `https://uclapi.com/roombookings/equipment`

This endpoint returns equipment information about a specific room.

**Allowed request type:** `GET`

### Query Parameters

```shell
curl https://uclapi.com/roombookings/equipment
```

```python
import requests

r = requests.get("https://uclapi.com/roombookings/equipment")
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/equipment")
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
The equipment field contains a list of equipment items. The length of this list can be different for every room.
It can also be 0.  
Each equipment item contains a `type`, a `description`, and the number of `units`.

Equipment Item | Example |  Description
--------- | ---------- |  -----------
`type` | `FE` | The type of equipment. Either Fixed Equipment (`FE`) or Fixed Feature (`FF`).
`description` | `Managed PC` | What the piece of equipment actually is.
`units` | `1`| The number of times this piece of equipment exists in the room.

### Errors

Error | Description
--------- | ---------
`No token provided` | Gets returned when you have not supplied a `token` in your request.
`Token does not exist` | Gets returned when you supply an invalid `token`.
`No roomid supplied` | Gets returned when you don't supply a `roomid`.
`No siteid supplied` | Gets returned when you don'y supply a `siteid`.

# Contributing
These docs are open sourced at [https://github.com/uclapi/apiDocs](https://github.com/uclapi/apiDocs).  
Any and all contributions are welcome! If you spot a type or error, fix it :)
