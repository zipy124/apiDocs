---
title: UCL API Reference

language_tabs:
  - shell
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - contributing

---
# Introduction

The base url is `https://uclapi.com/api/v1`

# Get All Rooms
### `/rooms/all`

This endpoint gets a list of all available rooms and their metadata.

## Query Parameters


```shell
curl https://uclapi.com/api/v1/rooms/all
```

```python
import requests

requests.get("https://uclapi.com/api/v1/rooms/all")
```

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://uclapi.com/api/v1/rooms/all', true);
xhr.send();

// response from the server
xhr.responseText;
```

**Restrictions:** `nil`

**Allowed request type:** `GET`

## Response

> Response

```json
[
  {
    "roomName":"Gustave Tuck Lecture Theater",
    "roomId": "abc123",
    "capacity": 200
  } 
]
```

Field | Type | Description
--------- | ---------- | -----------
roomName | `String` | The name of the room
roomId | `String` | The rooms id string
Capacity | `Number` | The total capacity of the room 

#Get Bookings for a Room
#See Which Rooms are Available in a Timeframe
#Add a Booking
#Delete a Booking
