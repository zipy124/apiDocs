---
title: UCL API Reference

language_tabs:
  - shell
  - python
  - javascript

---
# Getting Started

Yay, you made it 🎉

The base url is `https://uclapi.com/roombookings/`

# Get Rooms
**Endpoint:** `https://uclapi.com/roombookings/rooms`

This endpoint returns rooms and information about them.
If you don't specify any query parameters besides the `token`, all rooms will be returned.  
_Note: This endpoint only returns publicly bookable rooms. Departmentally bookable rooms are not included._

**Allowed request type:** `GET`

## Query Parameters

```shell
curl https://uclapi.com/roombookings/rooms
```

```python
import requests

r = requests.get("https://uclapi.com/roombookings/rooms")
print(r.json())
```

```javascript
fetch("https://uclapi.com/roombookings/rooms")
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
`roomid` | `433` | Optional | The room id (not to be confused with the room name)
`siteid` | `086`| Optional | Every room is inside a site (building). All sites have ids.
`name` | `Torrington (1-19) 433`| Optional | The room name often includes the site name as well.
`classification` | `CR`| Optional | The type of room. `LT` = Lecture Theatre, `CR` = Classroom, `SS` = Social Space, `PC1` = Public Cluster
`capacity` | `55`| Optional | Every room has a set capacity of how many people can fit inside it. When supplied, all rooms with the given capacity or greater will be returned.

## Response

```json
{
  "ok": true,
  "rooms": [
    {
      "capacity": 50,
      "roomid": "Z4",
      "name": "Wilkins Building (Main Building) Portico",
      "siteid": "005",
      "classification": "SS",
      "location": {
        "address1": "",
        "address2": "",
        "address3": "",
        "address4": ""
      }
    },
    {
      "capacity": 55,
      "roomid": "433",
      "name": "Torrington (1-19) 433",
      "siteid": "086",
      "classification": "CR"
    },
    ...
  ]
}
```

#Get Bookings for a Room

### `/bookings`

This endpoint shows the bookings for a room.

`start_datetime` and `end_datetime` should be given in ISO8601 format. No other formats are accepted.

If `start_datetime` or `end_datetime` is given, `startdate` will be ignored. If `startdate` is used, it will return all bookings for that specific day with all the other given filters applied

The API doesn't return all bookings at once. It uses something called pagination. If there are a lot of results, we split them up across different pages.
The results are paginated. You can specify the maximum number of results per page by supplying the `results_per_page` parameter. The maximum value we'll except for this is `100`. If this is not specified with the request, it will be set to 20 by default. The first page will have a `count` field which is the total number of results for the request made. If there are more pages, `next_page_exists` will be set to True. For subsequent requests, you only have to pass the `page_token`, no other parameters are required. Server will keep track of the pages you have been through and how many results you need to go through. If `page_token` is given every other parameters will be ignored.


## Query Parameters

Parameter | Example | Required | Description
---|---|---|---
token | `uclapi-5d58c3c4e6bf9c-c2910ad3b6e054-7ef60f44f1c14f-a05147bfd17fdb` | Optional | Authentication token
roomId |  `433`  |  Optional  | id of the room that you want to see the boookings for
start_datetime | `2011-03-06T03:36:45+00:00` | Optional | Datetime of the starting time
end_datetime | `2011-03-06T03:36:45+00:00` | Optional | Enddatetime of the bookings
startdate | `20160202` | Optional | Date of the bookings you need. This will only be used if above two are not given
site_id | `086` | Optional | Every room is inside a site (building). All sites have ids.
description | `Lecture` | Optional | I have no fucking clue what this is
contact | `Mark Herbster` | Optional | Name of the person who booked the room. You can part of the name or full name to filter it.
results_per_page | `50` | Optional | How many bookings per page. Maximum allowed is 100 per page. If nothing is given, we will use 20 as default

**Restrictions:** `nil`

**Allowed request type:** `GET`

```shell
curl https://uclapi.com/api/v1/bookings?roomId=abc123&startDate=2016-10-23T11:00:00&endDate=2016-10-23T14:00:00
```

```python
import requests

requests.get("https://uclapi.com/api/v1/bookings?roomId=abc123&startDate=2016-10-23T11:00:00&endDate=2016-10-23T14:00:00")
```

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://uclapi.com/api/v1/bookings?roomId=abc123&startDate=2016-10-23T11:00:00&endDate=2016-10-23T14:00:00', true);
xhr.send();

// response from the server
xhr.responseText;
```

## Response

> Response

```json
[
  {
    "bookingId": "def456",
    "startDate": "2016-10-23T12:00:00",
    "endDate": "2016-10-23T13:00:00"
  }
]
```

Field | Type | Description
--------- | ---------- | -----------
bookingId | `String` | The of the booking
startDate | `date` | Start date and time of the booking
endDate | `date` | End date and time of the booking

#See Which Rooms are Available in a Timeframe
#Add a Booking
#Delete a Booking
