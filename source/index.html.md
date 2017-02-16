---
title: UCL API Reference

language_tabs:
  - shell
  - python
  - javascript

toc_footers:
  - <a href='https://uclapi.com/dashboard/'>Get an API Key</a>

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

## Query Parameters

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

## Response

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
Each equipment item contains a `type`, a `description`, and the number of `units`.

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

# Get Equipment
**Endpoint:** `https://uclapi.com/roombookings/equipment`

This endpoint returns equipment information about a specific room.

**Allowed request type:** `GET`

## Query Parameters

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

## Response

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

# Contributing
These docs are open sourced at [https://github.com/uclapi/apiDocs](https://github.com/uclapi/apiDocs).  
Any and all contributions are welcome! If you spot a type or error, fix it :)
