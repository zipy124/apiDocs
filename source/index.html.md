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

# OAuth
OAuth support allows your application to authenticate with the UCL API against UCL's Single Sign On (SSO) system in order to provide a personalised experience to your users. This also means that you will not ever have to work with Shibboleth, as that is all abstracted away for you. Over the coming months we are also planning to add:
<ul>
  <li>Personalised room bookings data</li>
  <li>Personalised timetable data</li>
  <li>Personalised UCL Union data and submission</li>
</ul>

With OAuth it will be possible for users to authorise your application access to their personal data on a scope-by-scope basis.

<aside class="notice">
  <strong>Please Note!</strong>
  <hr />
  Whilst we roughly follow the OAuth 2.0 Specification, <a href="https://tools.ietf.org/html/rfc6749">RFC6749</a>, we have tweaked some parts of it to provide the most secure possible experience. We also do not (yet!) support every single subset of the specification. If you have a complex use case that we cannot cater for yet, please do let us know, and we will do our best to support you.<br />
  OAuth is a completely new feature to the UCL API and therefore it may contain bugs that we have not yet identified. If you come across any issues please do reach out to us and we will fix any issues as soon as we can.
</aside>

## Getting Started
Once you have created an app in the UCL API Dashboard you will see a dropdown labelled `OAuth Settings`. Click this to access a number of new fields:

Field            | Description
---------------- | ----------------
Client ID | Your application's OAuth Application ID
Client Secret | A parameter used for calculating cryptographic proof of your application's identity. Note that the way this is used is the key difference in our implementation of OAuth from the default.
Callback URL | The URL your users will be redirected to after they have logged in with UCL API. A GET parameter will specify whether they clicked Allow or Deny so that your application can handle both scenarios accordingly.
OAuth Scope | This allows you to request personal data from your users. More scopes will be added over time. You do not need to tick any scopes if you just want your application to use UCL API as an identity service without personal data access.

<aside class="warning">
  <strong>Important!</strong>
  <hr />
  <strong>Your application must have a Callback URL for OAuth to work.</strong> In addition, we strongly recommend that you use <strong>HTTPS with certificate validation enabled</strong> to ensure the tunnel between your application and the UCL API is fully encrypted, especially because this is the first time that you can use the UCL API to get specific personal user data, and that must be kept secure.

  It is <strong>your</strong> responsibility to keep all user data secure and to use it only for the purposes you tell your users you will use it for.
</aside>

## Client Libraries
Our technical flow is fully documented below so that you can implement a client library yourself. However, if you just want to get started straight away, we have some client libraries available for the following platforms:
* Django: [django-uclapi-oauth](https://github.com/uclapi/django-uclapi-oauth)

There are more to come, and we welcome contributions from the community to expand our ready-made library offerings!

## Authentication Flow
Our OAuth flow is not unlike any other flow from an organisation such as Slack or Google. However, you should read this section anyway to ensure you understand how to interface with us.

### User Login Experience
Before delving into the technical details, this is the experience a user should expect to see.
- User clicks a `Log In with UCL API` button.
- If the user does not have an active UCL SSO session, they will be asked to log in with UCL's Shibboleth SSO service.
- User is redirected back to a page which tells them which data will be passed to your app. They can either `Allow` or `Deny` this.
- User is redirected back to your application. If they denied the request, your app should tell them that this happened. If they accepted the request then your app should store this in a cookie so that the user does not have to log in through us again (unless you change the app's scope, as they will need to accept the new scopes for your app to access them).

### OAuth Technical Flow
In order to make this happen, there are a number of endpoints your application must respond to. A full example for Django is available [on our GitHub](https://github.com/uclapi/django-uclapi-oauth) for you to adapt. 

### Step 0: Render a Log In Button
In order to prevent CSRF attacks, we recommend you render a POST form with a login button and CSRF token to your users to let them log into UCL API. When clicked, this should generate a `state` parameter used to track the user through the login flow. You should store the `state` in the user's session cookie so that you can check that a given login response is valid.

In Django you might do something like the following.

> HTML form that is rendered to the user

```html
<form action="/uclapi/login/process" method="post">
    {% csrf_token %}
    <button type="submit">Log in with UCL API</button>
</form>
```

> Backend code

```python
@csrf_protect
def process_login(request):
    state = generate_state()
    request.session["state"] = state
    auth_url = "https://uclapi.com/oauth/authorise"
    auth_url += "?client_id=" + YOUR_CLIENT_ID
    auth_url += "&state=" + state
  
    return redirect(auth_url)
```

Your `generate_state` function should generate a random, long, ephemeral and unpredictable string.

### Step 1: The Log In
Once a `state` parameter is stored in the session, your application does not have to do anything until the callback. We and UCL's SSO system will handle authenticating the user.

### Step 2: The Callback
Once a user has passed the UCL login process we will tell them what data your app will be able to access, and from there they can choose either `Deny` or `Allow`. Both of these will redirect to the callback URL that you specify, but with different parameters.

#### Denied Login
If the login is denied, you the URL redirected to will be: `https://your/callback/url?result=denied&state=YOUR_STATE_PARAMETER`.

#### Allowed Login
If the user clicks to allow the application to access their data, they will be sent to `https://your/calllback/url?result=allowed&code=CODE_PARAMETER&client_id=YOUR_CLIENT_ID&state=YOUR_STATE_PARAMETER`.

The most important parameter to note here is the code parameter. This can be exchanged, via your application's secret, for an OAuth token for the user that you should store securely on your server. The process for this is as follows in an environment such as Django. The exchange should be done in the background by your app's server, and the endpoint is `https://uclapi.com/oauth/token?code=CODE_PARAMETER&client_secret_proof=CODE_ENCRYPTED_USING_CLIENT_SECRET`.

> Getting an OAuth token from a code

```python
def allowed(request):
    try:
        code = request.GET.get("code")
        client_id = request.GET.get("client_id")
        state = request.GET.get("state")
    except KeyError:
        return JsonResponse({
            "error": "Parameters missing from request."
        })

    try:
        session_state = request.session["state"]
    except KeyError:
        return JsonResponse({
            "ok": False,
            "error": "There is no session cookie set containing a state"
        })

    hmac_digest = hmac.new(bytes(YOUR_CLIENT_SECRET, 'ascii'),
                           msg=code.encode('ascii'),
                           digestmod=hashlib.sha256).digest()
    client_secret_proof = base64.b64encode(hmac_digest).decode()

    url = "https://uclapi.com/oauth/token"
    params = {
        'grant_type': 'authorization_code',
        'code': code,
        'client_secret_proof': client_secret_proof
    }

    r = requests.get(url, params=params)

    try:
        token_data = r.json()

        if token_data["ok"] is not True:
            return JsonResponse({
                "ok": False,
                "error": "An error occurred: " + token_data["error"]
            })

        if token_data["state"] != state:
            return JsonResponse({
                "ok": False,
                "error": "The wrong state was returned"
            })

        if token_data["client_id"] != client_id:
            return JsonResponse({
                "ok": False,
                "error": "The wrong client ID was returned"
            })

        # Retrieve the OAuth token
        token_code = token_data["token"]
        # The scope information is returned as a JSON object
        scope_data = json.loads(token_data["scope"])
    except KeyError:
        return JsonResponse({
            "ok": False,
            "error": "Proper JSON was not returned by the token endpoint"
        })

    #
    # NOW SAVE THE OAUTH TOKEN IN A DATABASE
    # Also you should render a success page to the user
    #
```

### Step 3 (Optional): Retrieve User Data
Once you have a user token you can retrieve basic user data, such as their name, email address, etc., by using the `oauth/user/data` endpoint. Note that once you have a user token, you **must** get a Nonce code from the `/oauth/nonce` endpoint in order to get personal data. This is protect against replay attacks.

```python
  # Get a Nonce from the oauth/nonce endpoint
  url = "https://uclapi.com/oauth/nonce"
  params = {
      'token': token_code,
      'client_secret_proof': client_secret_proof
  }

  r = requests.get(url, params=params)

  nonce_data = r.json()
  try:
      nonce = nonce_data["nonce"]
  except KeyError:
      return JsonResponse({
          "ok": False,
          "error": "No nonce was received."
      })

  # Generate a vertification string, which is just the token, an ampersand
  # and the nonce concatenated. This is then encrypted with the client secret
  # as before.
  # This protects against replay attacks.
  verification_str = token_code + "&" + nonce

  hmac_digest = hmac.new(bytes(YOUR_CLIENT_SECRET, 'ascii'),
                          msg=verification_str.encode('ascii'),
                          digestmod=hashlib.sha256).digest()
  client_secret_proof = base64.b64encode(hmac_digest).decode()

  url = "https://uclapi.com/oauth/user/data"
  params = {
      'token': token_code,
      'nonce': nonce,
      'client_secret_proof': client_secret_proof
  }

  r = requests.get(url, params=params)

  return JsonResponse(r.json())
```

> Example JSON Response

```json
{
  "ok": true,
  "department": "Dept of Well Documented APIs™️",
  "email": "abcdefg@ucl.ac.uk",
  "cn": "abcdefg",
  "full_name": "Ms Amazing B Person",
  "scope_number": 2,
  "upi": "amper11",
  "given_name": "Amazing B"
}
```

## Client Secrets
Most of the above looks like a normal OAuth 2.0 login flow, and that's because it is. There is one major difference, however, and that is that **you never, ever, send your raw client secret in a request**!. This is so that even if somebody was to Man-in-The-Middle (MiTM) the connection, and you did not verify certificates, the most the attacker would be able to get would be a user token. If a user token is compromised it should still be regenerated; however, it is completely unusable without the `client_secret_proof` parameter which is generated using the Client Secret. A `client_secret_proof` parameter **must** be sent with every request to the API. In addition, every request that includes a user's token requires a randomly-generated, one-use nonce. This prevents even a stolen `client_secret_proof` parameter from being used twice with the same token. The algorithm for generating this parameter is as follows:
<ol>
  <li>
    Before anything, use the `/oauth/nonce` endpoint to get a Nonce parameter for your request.
  </li>
  <li>
    Now, take the user token and add an ampersand (&amp;) and the nonce provided to you in the previous step. You should get a value that looks like `uclapi-user-abcdefg123456hij-abcdefg123456hij-abcdefg123456hij-abcdefg123456hij&nonceabcd1234abcd1234abcd1234abcd1234abcd1234`.
  </li>
  <li>
    Next, convert this to string bytes using the ASCII character set.
  </li>
  <li>
    Take the Client Secret and also convert this to ASCII bytes.
  </li>
  <li>
    Create a <strong>SHA256 HMAC<strong> representation of the message data using the Client Secret as the key.
  </li>
  <li>
    Base64 encode this HMAC to a string.
  </li>
  <li>
    Attach it to your JSON request. The `key` is <strong>always</strong> `client_secret_proof`, and the value should be the Base64 string you generated above.,
  </li>
</ol>

The reason this process is so great is that you *never, ever* send your Client Secret out in the open or over a broken HTTPS connection. It also means that if somebody captures some HTTPS traffic and tries to retransmit it in order to make the same thing happen twice the nonce will no longer exist, and the attempt will be blocked. This technique is more complex than usual OAuth, but exists to keep your UCL data secure.

An example of this in use is available above in the Retrieve User Data section.

# Get Involved
This documentation is all open sourced at [https://github.com/uclapi/apiDocs](https://github.com/uclapi/apiDocs).

The full API is open sourced at [https://github.com/uclapi/uclapi](https://github.com/uclapi/uclapi).

Any and all contributions are welcome! If you spot a typo or error, feel free to fix it and submit a pull request :)
