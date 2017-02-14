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

This endpoint shows the bookings for a room

## Query Parameters

Parameter | Type | Default Value | Description
---|---|---|---
roomId |  `String[]`  |  All room ids   | A list of the ids of rooms you want to see the bookings for
startDate | `date` | Current date and time rounded up to the nearest hour | The start date and time you want to search from
endDate | `date` | One week from the current date and time rounded up to the nearest hour | The end date and time you want to search to

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
