---
title: Wie Betaalt Wat API
language_tabs:
  - http: HTTP
  - java: Java
  - javascript: JavaScript
  - shell: curl
  - swift: Swift
  - ruby: Ruby
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<h1 id="wie-betaalt-wat-api">Wie Betaalt Wat API v0.90.15</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

# API documentation for Wie Betaalt Wat service"

## Checking of abilities

Clients often need to know 'can a user delete this Thing?" or "should
I show a "create" button below this collection of things?"

In WBW, clients can use links to determine business logic:

Links contain all links to possible actions on this list.

Actions are "create new members on a list" and "get all members from
this list". The latter can also be described as "get the members
collection".

When a link is not there, the action is not possible (e.g. because of
rights, or other business logic). Appearence of links on collection can
act as a permission checker for clients. E.g. when the "create" link
would not be there, the current user cannot create members on this list.

(Note: this example is not very realistic, since all members can always
create members on a list, so this link will always be there: it is just
an example of how this linkin works).

## Caching of endpoints: Conditional GET.

When GETting endpoints, for example to refresh something, a client can
greatly speed up communication by sending the correct caching headers.

It removes a lot of load from the backend, greatly reduces the data-
transfer and avoids updating objects locally when nothing has changed.
Because of this, it becomes a vaiable option to fetch resources on the
backend very often: just as the backend instead of implementing logic
locally to determine wheter or not the backend might have changed
something.

A cached response will either return a 200-OK response with a body, or
it will return a 304-Not Modified without a body.

Any GET-endpoint may be cached in this way. Not just endpoints that
return a single object, but also collections which are paged, sorted
and so on.
POST, PATCH or DELETE requests can never be cached.

### E-Tag
[E-tags](https://en.wikipedia.org/wiki/HTTP_ETag) are part of the HTTP
protocol and are used as a mechanism to version responses.

When an endpoint is requested, and it supports caching, an ETag header
is included in the response.

    curl -i http://example.com/balances/mine

    HTTP/1.1 200 OK
    Etag: "f049a9825a0239db3d01ead65a5ea522"
    Last-Modified: Tue, 02 Dec 2014 18:31:42 GMT

The client now needs to store, or track the etag. In order to use it in
a subsequent request.
This response is a 200 OK, so it will contain a body with a JSON
payload.

    curl -i -H 'If-None-Match: "f049a9825a0239db3d01ead65a5ea522"' http://example.com/balances/mine

    HTTP/1.1 304 Not Modified
    Etag: "f049a9825a0239db3d01ead65a5ea522"
    Last-Modified: Tue, 02 Dec 2014 18:31:42 GMT

The client now knows that nothing has changed. It knows that it does
not need to update any views, models or local storage. This response
was also very light to generate for the backend, so the response will
be returned a lot faster than the 200-OK response with content.

The downside is that the client needs to record the Etags for responses
in order to re-use them with future requests.

### If-Modified-Since
[If-Modified-Since](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
are a mechanism to allow the server to send a cached response based
on a timestamp which the client knows it has content for.

Our endpoints **do not allow this caching method**, because it does not
allow discrimination of requests and responses with locales set.
Using this, would mean that you always get the response in the locale
with which the first request was made.

Base URLs:

* <a href="https://stagingwiebetaaltwat.nl/api">https://stagingwiebetaaltwat.nl/api</a>

<a href="http://example.com/terms/">Terms of service</a>

 License: MIT

<h1 id="wie-betaalt-wat-api-users">users</h1>

Users and Accounts

## post__confirmations

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/confirmations HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/confirmations");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/confirmations',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/confirmations \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/confirmations',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /confirmations`

This resource does not need a valid session (but can  be performed when there is a valid session) When token sent in the request, matches a user, we will confirm that user, regardless of the current user, if any.  When the token sent in the request does not match a user, an error will be returned.

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="post__confirmations-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Confirmation created: account is confirmed|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request, if token could not be found|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## put__confirmations

> Code samples

```http
PUT https://stagingwiebetaaltwat.nl/api/confirmations HTTP/1.1
Host: stagingwiebetaaltwat.nl

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/confirmations");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/confirmations',
  method: 'put',

  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PUT https://stagingwiebetaaltwat.nl/api/confirmations

```

```ruby
require 'rest-client'
require 'json'

result = RestClient.put 'https://stagingwiebetaaltwat.nl/api/confirmations',
  params: {
  }

p JSON.parse(result)

```

`PUT /confirmations`

Replace the confirmation: resend confirmation mail. Can be used for re-sending a registration confirmation, or a re-confirmation of a changed email.

<h3 id="put__confirmations-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Confirmation replaced: confirmation email resent. On error, e.g. if the email is not found or user has no pending confirmations, we still return a 200 OK, to disallow external users to mine emails.|None|

<aside class="success">
This operation does not require authentication
</aside>

## post__invited_users

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/invited_users HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/invited_users");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/invited_users',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/invited_users \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/invited_users',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /invited_users`

Register new invited user

> Body parameter

```json
{
  "user": {
    "email": "daan@example.com",
    "password": "87bd29c584ae5223aa8a96b4",
    "first_name": "Daan",
    "last_name": "de Jong",
    "invitation_token": "20f936a0",
    "terms_accepted": true,
    "subscribed_for_updates": true,
    "intent_url": "invitations/{token}"
  }
}
```

<h3 id="post__invited_users-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UserAsInviteeContainer](#schemauserasinviteecontainer)|true|User that was invited and is to be registered|

> Example responses

> 201 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="post__invited_users-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|A Current User object. Depending on backend logic,
            this user is either a pending, unconfirmed user, or a logged in,
            confirmed user.

            Basically, a user will be confirmed and logged in if the email
            that was used is the same as the email the member was invited on.
            In that case, we can decide to automatically confirm the user,
            and log the user in, instead of sending a confirmation email.

            For clients, however, this should not matter, as this works
            transparantly. Clients only need to determine wether they want
            to register an invited user (user with an invitation token) or a
            normal user, one without an invitation token. And decide to use
            this InvitedUser Resource or the User Resource.

            A client can determine if a user is logged in, by requesting the
            current user resource.|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__users_current

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/users/current HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/current");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/current',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/users/current \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/users/current',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /users/current`

The user object for the user currently logged in.

> Example responses

> 200 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="get__users_current-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Avatar for the User|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__users

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/users HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/users \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/users',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /users`

Register new user

> Body parameter

```json
{
  "user": {
    "email": "daan@example.com",
    "password": "d81dfccc5cccde34fb44a37c",
    "first_name": "Daan",
    "last_name": "de Jong",
    "terms_accepted": true,
    "subscribed_for_updates": true,
    "intent_url": "invitations/{token}"
  }
}
```

<h3 id="post__users-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UserAsRegistrantContainer](#schemauserasregistrantcontainer)|true|User to be registered|

> Example responses

> 201 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="post__users-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|A Current User object|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__users_sign_in

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/users/sign_in HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/sign_in");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/sign_in',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/users/sign_in \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/users/sign_in',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /users/sign_in`

Sign in an existing user. Note: this resource is inconsistent due to implementation details. which will be fixed in a future version.

> Body parameter

```json
{
  "email": "daan@example.com",
  "password": "5755f8cd04bb1369c7c09126"
}
```

<h3 id="post__users_sign_in-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UserAsSignIn](#schemauserassignin)|true|User to be signed in|

> Example responses

> 201 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="post__users_sign_in-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|A Current User object|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__users_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/users/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/users/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/users/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /users/{id}`

Update user Profile.

> Body parameter

```json
{
  "user": {
    "first_name": "Daan",
    "last_name": "de Jong",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "[DEPRECATED in API v4 see financial accounts] D.R.W. De Jong en Zonen B.V.",
    "city": "Rotterdam",
    "locale": "nl",
    "terms_accepted": true,
    "subscribed_for_updates": true
  }
}
```

<h3 id="patch__users_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the User|
|body|body|[UserAsProfileContainer](#schemauserasprofilecontainer)|true|Profile to be changed|

> Example responses

> 200 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="patch__users_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Current User object|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__users_{id}_intent

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/users/{id}/intent HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/{id}/intent");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/{id}/intent',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/users/{id}/intent \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/users/{id}/intent',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /users/{id}/intent`

Update intent url.

> Body parameter

```json
{
  "user": {
    "intent_url": "http://example.com"
  }
}
```

<h3 id="patch__users_{id}_intent-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the User|
|body|body|[UserIntentContainer](#schemauserintentcontainer)|true|User intent url|

> Example responses

> 200 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="patch__users_{id}_intent-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Current User object|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__accounts_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/accounts/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/accounts/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/accounts/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/accounts/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/accounts/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /accounts/{id}`

## Update user Account.

This endpoint updates the account-details for a user.

Both emailaddress and password may only be updated if the user provides
a correct old password.

Changing the password has no effect on the current session. User remains
logged in, the client should record the new session/cookie, though, for
that might change.

Changing the emailaddress means a confirmation mail is sent out to that
emailaddress. Once that is confirmed (by following the confirmation-URL
in that email) this is the new emailaddress, any old emailaddress is now
lost. Untill the moment the email is confirmed, it appears in the
current user object as unconfirmed_email. When a user with a pending
email confirmation changes the email (again), the old unconfirmed email
becomes invalid. Following the confirmation-link sent to that
emailaddress has no effect. A new confirmation email is sent to the new
emailaddress.

A user cannot log in with an unconfirmed email, untill it is confirmed.
After which it is the only way to log in.

NOTE: After registration, a confirmation mail is sent too. No
account-details, nor any Profile details can be changed untill this
initial account is confirmed.

> Body parameter

```json
{
  "user": {
    "old_password": "8e0b4eb84e6472f4830bc85d",
    "password": "c320011fc8375f46f1dac8ae",
    "email": "otherdaan@example.com"
  }
}
```

<h3 id="patch__accounts_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Account (User)|
|body|body|[UserAsAccountContainer](#schemauserasaccountcontainer)|true|Account to be changed|

> Example responses

> 201 Response

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}
```

<h3 id="patch__accounts_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|A Current User object|[CurrentUser](#schemacurrentuser)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__accounts_{id}

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/accounts/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/accounts/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/accounts/{id}',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/accounts/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/accounts/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /accounts/{id}`

## Delete user Account.

This endpoint deletes the account for a user.

All sensitive user data is removed (like financial accounts, password)
or scrambled (email).

Additionally all accepted members of this user are be deleted.

The account delete will only succeed when there are no members for this
user with a non-zero balance.

If the delete fails a 400 bad request is returned. This will most often
occur with the error "members: ["Members is invalid"] because one of the
members has a non-zero balance.

After account deletion the user can reregister using his/her email
address.

<h3 id="delete__accounts_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Account (User)|

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__accounts_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Account removed succesfully|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__users_sign_out

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/users/sign_out HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/sign_out");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/sign_out',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/users/sign_out \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/users/sign_out',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /users/sign_out`

Sign out the current user. Note: this resource is inconsistent due to implementation details. which will be fixed in a future version.

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__users_sign_out-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A empty response|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__users_{id}_avatar

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/users/{id}/avatar HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: multipart/form-data
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/{id}/avatar");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'multipart/form-data',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/{id}/avatar',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/users/{id}/avatar \
  -H 'Content-Type: multipart/form-data' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'multipart/form-data',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/users/{id}/avatar',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /users/{id}/avatar`

Attach Avatar to User

> Body parameter

```yaml
image: string

```

<h3 id="patch__users_{id}_avatar-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the User|
|body|body|[patchExpenses_id_image](#schemapatchexpenses_id_image)|false|none|
|» image|body|string(binary)|true|none|

> Example responses

> 200 Response

```json
{
  "links": {
    "update": {
      "href": "/api/users/{user_id}/avatar"
    },
    "delete": {
      "href": "/api/users/{user_id}/avatar"
    }
  },
  "image": {
    "original": "http://example.com/images/image.png",
    "large": "http://example.com/images/large_image.png",
    "medium": "http://example.com/images/medium_image.png",
    "small": "http://example.com/images/small_image.png"
  }
}
```

<h3 id="patch__users_{id}_avatar-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Avatar for the User|[Avatar](#schemaavatar)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__users_{id}_avatar

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/users/{id}/avatar HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/{id}/avatar");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/{id}/avatar',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/users/{id}/avatar \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/users/{id}/avatar',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /users/{id}/avatar`

Delete Avatar from user: revert to default Avatar

<h3 id="delete__users_{id}_avatar-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the User|

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__users_{id}_avatar-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A empty response|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__users_:user_id_financial_accounts

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/users/:user_id/financial_accounts HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/users/:user_id/financial_accounts");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/users/:user_id/financial_accounts',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/users/:user_id/financial_accounts \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/users/:user_id/financial_accounts',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /users/:user_id/financial_accounts`

The list of financial account objects for the specified user.

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "financial_account": {
        "id": "5b3f19e5-b875-4550-8723-192799dd1091",
        "publish_details": true,
        "payment_method": "ManualIBAN",
        "iban": "NL39 RABO 0300 0652 64",
        "account_holder": "D.R.W. De Jong en Zonen B.V.",
        "created_at": 1554119930,
        "updated_at": 1554119930,
        "state": "pending",
        "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
        "oauth_intent": "splitser://example/status",
        "oauth_platform": "android",
        "external_account_id": "test@paypal.com"
      }
    }
  ]
}
```

<h3 id="get__users_:user_id_financial_accounts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|List of financial accounts.             They may contain either full or restricted set of data|[FinancialAccountCollection](#schemafinancialaccountcollection)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-members">members</h1>

Members on List

<a href="https://bitbucket.org/ttyams/wbw-ios/wiki/Member">Member on Wiki</a>

## get__api_lists_{list_id}_members

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /api/lists/{list_id}/members`

Fetch all members on a list

<h3 id="get__api_lists_{list_id}_members-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the List|
|filter[with_empty]|query|boolean|false|Filter to in- or exclude empty groups. Adding the filter query param|

#### Detailed descriptions

**filter[with_empty]**: Filter to in- or exclude empty groups. Adding the filter query param
will return all groups for a list including the empty groups. Empty
groups are groups that have no members attached to them. Either
because all members were removed, or because no members were added,
yet.
The backend reserves the right to remove these empty groups after X
time, to keep the database clean.
One could best see these empty groups as "Drafts" or not-yet-finished
groups.

*TODO:* implement filter availability discoverability in the API.

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "member": {
        "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
        "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
        "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
        "nickname": "daantjepaantje",
        "email": "daantjepaantje@example.com",
        "is_current": false,
        "invitation": {
          "links": null,
          "invitation": {
            "state": "active",
            "url": "https://links.example.com/lists/{token}"
          }
        },
        "created_at": 0,
        "updated_at": 0,
        "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
        "avatar": {
          "links": {
            "update": {
              "href": "/api/users/{user_id}/avatar"
            },
            "delete": {
              "href": "/api/users/{user_id}/avatar"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png"
          }
        }
      }
    }
  ]
}
```

<h3 id="get__api_lists_{list_id}_members-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Members|[MemberCollection](#schemamembercollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__api_lists_{list_id}_members

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/api/lists/{list_id}/members',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /api/lists/{list_id}/members`

Create (invite) a Member

> Body parameter

```json
{
  "member": {
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com"
  }
}
```

<h3 id="post__api_lists_{list_id}_members-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the List|
|body|body|[MemberInputContainer](#schemamemberinputcontainer)|true|Member to be generated|

> Example responses

> 200 Response

```json
{
  "member": {
    "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com",
    "is_current": false,
    "invitation": {
      "links": null,
      "invitation": {
        "state": "active",
        "url": "https://links.example.com/lists/{token}"
      }
    },
    "created_at": 0,
    "updated_at": 0,
    "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  }
}
```

<h3 id="post__api_lists_{list_id}_members-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Member|[MemberContainer](#schemamembercontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__members_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/members/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/members/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/members/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/members/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/members/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /members/{id}`

Fetch single Member.

<h3 id="get__members_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Member|

> Example responses

> 200 Response

```json
{
  "member": {
    "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com",
    "is_current": false,
    "invitation": {
      "links": null,
      "invitation": {
        "state": "active",
        "url": "https://links.example.com/lists/{token}"
      }
    },
    "created_at": 0,
    "updated_at": 0,
    "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  }
}
```

<h3 id="get__members_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Member|[MemberContainer](#schemamembercontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__members_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/members/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/members/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/members/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/members/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/members/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /members/{id}`

Update a Member

> Body parameter

```json
{
  "nickname": "daantjepaantje"
}
```

<h3 id="patch__members_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Member|
|body|body|[MemberUpdateInput](#schemamemberupdateinput)|true|none|

> Example responses

> 200 Response

```json
{
  "member": {
    "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com",
    "is_current": false,
    "invitation": {
      "links": null,
      "invitation": {
        "state": "active",
        "url": "https://links.example.com/lists/{token}"
      }
    },
    "created_at": 0,
    "updated_at": 0,
    "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  }
}
```

<h3 id="patch__members_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A member|[MemberContainer](#schemamembercontainer)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__members_{id}

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/members/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/members/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/members/{id}',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/members/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/members/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /members/{id}`

Marks a Membere as deleted.

<h3 id="delete__members_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Member|

> Example responses

> 200 Response

```json
{
  "member": {
    "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com",
    "is_current": false,
    "invitation": {
      "links": null,
      "invitation": {
        "state": "active",
        "url": "https://links.example.com/lists/{token}"
      }
    },
    "created_at": 0,
    "updated_at": 0,
    "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  }
}
```

<h3 id="delete__members_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Member|[MemberContainer](#schemamembercontainer)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-expenses">expenses</h1>

Expenses Operations

<a href="https://example.com/">TODO: Link naar Wiki::Expense</a>

## get__lists_{list_id}_expenses

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists/{list_id}/expenses`

Fetch a collection of Expenses on a List. 

<h3 id="get__lists_{list_id}_expenses-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the List|

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "expense": {
        "id": "31365abb-325a-4d42-b452-3324a22d83b1",
        "name": "Burritos",
        "list_id": "77c5b285-05f0-4d5b-87d1-f040a89debdd",
        "settle_id": "f6872a02-b892-4e74-85f6-a69c938adbfd",
        "payed_on": "YYYY-MM-DD",
        "created_at": 0,
        "updated_at": 0,
        "status": "active",
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "payed_by_id": "7256ce38-42ba-4125-a70d-2c812ddb718c",
        "participant_names": [
          "irene",
          "Daan"
        ],
        "image": {
          "links": {
            "update": {
              "href": "/api/lists/{list_id}/image"
            },
            "delete": {
              "href": "/api/lists/{list_id}/image"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png",
            "micro": "http://example.com/images/micro_image.png"
          }
        }
      }
    }
  ]
}
```

<h3 id="get__lists_{list_id}_expenses-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Expenses|[ExpenseCollection](#schemaexpensecollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__lists_{list_id}_expenses

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/expenses',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /lists/{list_id}/expenses`

Create an Expense

> Body parameter

```json
{
  "expense": {
    "name": "Burritos",
    "payed_by_id": "25d2391f-45dd-4d21-9e80-be54e04ea07c",
    "payed_on": "2016-02-18",
    "amount": 1337,
    "shares_attributes": [
      {
        "amount": 1337,
        "id": "7bb760f0-8a7a-434d-aabc-d7925631806a",
        "member_id": "a70c1aa6-10fd-4ccb-85be-77f70fe1430c",
        "multiplier": 0,
        "_destroy": 0
      }
    ]
  }
}
```

<h3 id="post__lists_{list_id}_expenses-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the List|
|body|body|[ExpenseInput](#schemaexpenseinput)|true|Expense to be generated|

> Example responses

> 200 Response

```json
{
  "expense": {
    "id": "27f6b16d-93fe-409c-b5e4-844b376da9a4",
    "name": "Burritos",
    "list_id": "4950b38b-c187-4668-a760-ed86663076c3",
    "settle_id": "c6ab38e9-ae89-4171-9e02-d3545ae687c2",
    "payed_on": "YYYY-MM-DD",
    "created_at": 0,
    "updated_at": 0,
    "status": "active",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payed_by_id": "b762d775-8ad0-4895-a209-40199c0f1154",
    "payed_by_member_instance_id": "0d2e3980-d8cc-475d-9fc5-aa1c588b4f80",
    "payed_by": {
      "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
      "nickname": "daantjepaantje",
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "state": "anonymous"
    },
    "shares": [
      {
        "share": {
          "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
          "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "multiplier": 0
        }
      }
    ],
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png",
        "micro": "http://example.com/images/micro_image.png"
      }
    }
  }
}
```

<h3 id="post__lists_{list_id}_expenses-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An Expense|[ExpenseContainer](#schemaexpensecontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__settles_{settle_id}_expenses

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/settles/{settle_id}/expenses HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/settles/{settle_id}/expenses");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/settles/{settle_id}/expenses',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/settles/{settle_id}/expenses \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/settles/{settle_id}/expenses',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /settles/{settle_id}/expenses`

Fetch a collection of Expenses on a Settle. 

<h3 id="get__settles_{settle_id}_expenses-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|settle_id|path|string|true|ID of the Settle|

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "expense": {
        "id": "31365abb-325a-4d42-b452-3324a22d83b1",
        "name": "Burritos",
        "list_id": "77c5b285-05f0-4d5b-87d1-f040a89debdd",
        "settle_id": "f6872a02-b892-4e74-85f6-a69c938adbfd",
        "payed_on": "YYYY-MM-DD",
        "created_at": 0,
        "updated_at": 0,
        "status": "active",
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "payed_by_id": "7256ce38-42ba-4125-a70d-2c812ddb718c",
        "participant_names": [
          "irene",
          "Daan"
        ],
        "image": {
          "links": {
            "update": {
              "href": "/api/lists/{list_id}/image"
            },
            "delete": {
              "href": "/api/lists/{list_id}/image"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png",
            "micro": "http://example.com/images/micro_image.png"
          }
        }
      }
    }
  ]
}
```

<h3 id="get__settles_{settle_id}_expenses-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Expenses|[ExpenseCollection](#schemaexpensecollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__expenses_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/expenses/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/expenses/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/expenses/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/expenses/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/expenses/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /expenses/{id}`

Show details of an Expense

<h3 id="get__expenses_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Expense|

> Example responses

> 200 Response

```json
{
  "expense": {
    "id": "27f6b16d-93fe-409c-b5e4-844b376da9a4",
    "name": "Burritos",
    "list_id": "4950b38b-c187-4668-a760-ed86663076c3",
    "settle_id": "c6ab38e9-ae89-4171-9e02-d3545ae687c2",
    "payed_on": "YYYY-MM-DD",
    "created_at": 0,
    "updated_at": 0,
    "status": "active",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payed_by_id": "b762d775-8ad0-4895-a209-40199c0f1154",
    "payed_by_member_instance_id": "0d2e3980-d8cc-475d-9fc5-aa1c588b4f80",
    "payed_by": {
      "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
      "nickname": "daantjepaantje",
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "state": "anonymous"
    },
    "shares": [
      {
        "share": {
          "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
          "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "multiplier": 0
        }
      }
    ],
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png",
        "micro": "http://example.com/images/micro_image.png"
      }
    }
  }
}
```

<h3 id="get__expenses_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An Expense|[ExpenseContainer](#schemaexpensecontainer)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## put__expenses_{id}

> Code samples

```http
PUT https://stagingwiebetaaltwat.nl/api/expenses/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/expenses/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/expenses/{id}',
  method: 'put',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PUT https://stagingwiebetaaltwat.nl/api/expenses/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.put 'https://stagingwiebetaaltwat.nl/api/expenses/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PUT /expenses/{id}`

Replace an Expense

All given expense properties and the shares present in the input
payload will replace the old ones. The shares are mapped by member_id,
so the input payload should never contain an actual id.

Patching shares was a bit tricky, since clients needed to determine the
difference between old and new, in order to submit any changes. This
led to sync problems: other clients could have changed the share.

Considering the before mentioned sync problem, always replacing the
shares solves this, because the 'last request wins'.

> Body parameter

```json
{
  "expense": {
    "name": "Burritos",
    "payed_by_id": "80937715-dee4-4c55-80c2-6845a66d6c1f",
    "payed_on": "2016-02-18",
    "amount": 1337,
    "shares": [
      {
        "amount": 1337,
        "member_id": "a89629ec-2457-48ae-a334-73522a113941",
        "multiplier": 0
      }
    ]
  }
}
```

<h3 id="put__expenses_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Expense|
|body|body|[ExpenseReplaceInput](#schemaexpensereplaceinput)|true|none|

> Example responses

> 200 Response

```json
{
  "expense": {
    "id": "27f6b16d-93fe-409c-b5e4-844b376da9a4",
    "name": "Burritos",
    "list_id": "4950b38b-c187-4668-a760-ed86663076c3",
    "settle_id": "c6ab38e9-ae89-4171-9e02-d3545ae687c2",
    "payed_on": "YYYY-MM-DD",
    "created_at": 0,
    "updated_at": 0,
    "status": "active",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payed_by_id": "b762d775-8ad0-4895-a209-40199c0f1154",
    "payed_by_member_instance_id": "0d2e3980-d8cc-475d-9fc5-aa1c588b4f80",
    "payed_by": {
      "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
      "nickname": "daantjepaantje",
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "state": "anonymous"
    },
    "shares": [
      {
        "share": {
          "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
          "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "multiplier": 0
        }
      }
    ],
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png",
        "micro": "http://example.com/images/micro_image.png"
      }
    }
  }
}
```

<h3 id="put__expenses_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An Expense|[ExpenseContainer](#schemaexpensecontainer)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__expenses_{id}

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/expenses/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/expenses/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/expenses/{id}',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/expenses/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/expenses/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /expenses/{id}`

Marks an Expense as deleted. Does **not** remove it.

<h3 id="delete__expenses_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Expense|

> Example responses

> 200 Response

```json
{
  "expense": {
    "id": "27f6b16d-93fe-409c-b5e4-844b376da9a4",
    "name": "Burritos",
    "list_id": "4950b38b-c187-4668-a760-ed86663076c3",
    "settle_id": "c6ab38e9-ae89-4171-9e02-d3545ae687c2",
    "payed_on": "YYYY-MM-DD",
    "created_at": 0,
    "updated_at": 0,
    "status": "active",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payed_by_id": "b762d775-8ad0-4895-a209-40199c0f1154",
    "payed_by_member_instance_id": "0d2e3980-d8cc-475d-9fc5-aa1c588b4f80",
    "payed_by": {
      "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
      "nickname": "daantjepaantje",
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "state": "anonymous"
    },
    "shares": [
      {
        "share": {
          "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
          "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "multiplier": 0
        }
      }
    ],
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png",
        "micro": "http://example.com/images/micro_image.png"
      }
    }
  }
}
```

<h3 id="delete__expenses_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An Expense|[ExpenseContainer](#schemaexpensecontainer)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__expenses_{id}_image

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/expenses/{id}/image HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: multipart/form-data
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/expenses/{id}/image");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'multipart/form-data',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/expenses/{id}/image',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/expenses/{id}/image \
  -H 'Content-Type: multipart/form-data' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'multipart/form-data',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/expenses/{id}/image',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /expenses/{id}/image`

Attach image to Expense

> Body parameter

```yaml
image: string

```

<h3 id="patch__expenses_{id}_image-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Expense|
|body|body|[patchExpenses_id_image](#schemapatchexpenses_id_image)|false|none|
|» image|body|string(binary)|true|none|

> Example responses

> 200 Response

```json
{
  "image": {
    "links": {
      "update": {
        "href": "/api/lists/{list_id}/image"
      },
      "delete": {
        "href": "/api/lists/{list_id}/image"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png",
      "micro": "http://example.com/images/micro_image.png"
    }
  }
}
```

<h3 id="patch__expenses_{id}_image-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Images for the Expense|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<h3 id="patch__expenses_{id}_image-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» image|object|false|none|An image on an Expense|
|»» links|object|false|none|none|
|»»» update|object|false|none|none|
|»»»» href|any|false|none|none|
|»»» delete|object|false|none|none|
|»»»» href|any|false|none|Removes the image from the list|
|»»» image|any|true|none|Contains links to variations of images, when               available. Empty, or NULL links mean that this variation has no               image, or that the expense has no image (yet).|

<aside class="success">
This operation does not require authentication
</aside>

## delete__expenses_{id}_image

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/expenses/{id}/image HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/expenses/{id}/image");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/expenses/{id}/image',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/expenses/{id}/image \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/expenses/{id}/image',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /expenses/{id}/image`

Delete Image from Expense, emptying the expense image

<h3 id="delete__expenses_{id}_image-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the List|

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__expenses_{id}_image-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A empty response|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-balances">balances</h1>

Balance on List

<a href="https://bitbucket.org/ttyams/wbw-ios/wiki/Balance">Balance on Wiki</a>

## get__lists_{list_id}_balance

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/balance HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/balance");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/balance',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/balance \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/balance',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists/{list_id}/balance`

Fetch a Balance representation of a List

<h3 id="get__lists_{list_id}_balance-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the balance. Is the same as the List-ID|

> Example responses

> 200 Response

```json
{
  "links": {
    "current": "/api/balance/01dbc53a-6447-48eb-a086-8b7bb0b34fd4"
  },
  "balance": {
    "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "total": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "member_totals": [
      {
        "links": {
          "member": "/api/balance/00e5120c-c06a-4539-bba0-3bc7f748aa8c"
        },
        "member_total": {
          "member": {
            "id": "00e5120c-c06a-4539-bba0-3bc7f748aa8c",
            "nickname": "daan",
            "is_current": false,
            "avatar_urls": {
              "links": {
                "update": {
                  "href": "/api/users/{user_id}/avatar"
                },
                "delete": {
                  "href": "/api/users/{user_id}/avatar"
                }
              },
              "image": {
                "original": "http://example.com/images/image.png",
                "large": "http://example.com/images/large_image.png",
                "medium": "http://example.com/images/medium_image.png",
                "small": "http://example.com/images/small_image.png"
              }
            }
          },
          "balance_total": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "balance_indicator": 0,
          "expense_total": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "cost_total": {
            "fractional": "10.00",
            "currency": "EUR"
          }
        }
      }
    ]
  }
}
```

<h3 id="get__lists_{list_id}_balance-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Balance Resource|[Balance](#schemabalance)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__balances_mine

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/balances/mine HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/balances/mine");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/balances/mine',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/balances/mine \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/balances/mine',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /balances/mine`

Fetch my total balance over all lists

> Example responses

> 200 Response

```json
{
  "balance": {
    "fractional": "10.00",
    "currency": "EUR"
  }
}
```

<h3 id="get__balances_mine-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Simple Balance Object|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<h3 id="get__balances_mine-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» balance|object|false|none|none|
|»» fractional|number(double)|true|none|none|
|»» currency|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-settlement_previews">settlement_previews</h1>

Settlement preview

## get__lists_{list_id}_settlement_preview

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settlement_preview HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settlement_preview");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settlement_preview',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settlement_preview \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settlement_preview',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists/{list_id}/settlement_preview`

Fetch a Settlement preview representation of a List

<h3 id="get__lists_{list_id}_settlement_preview-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the preview. Is the same as the List-ID|

> Example responses

> 200 Response

```json
{
  "links": {
    "current": "/api/list/01dbc53a-6447-48eb-a086-8b7bb0b34fd4/settlement_preview"
  },
  "settlement_preview": {
    "created_at": 0,
    "updated_at": 0,
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "expenses": {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/expenses"
        },
        "create": {
          "href": "/api/lists/{list_id}/expenses"
        }
      }
    },
    "list_commitments": [
      {
        "list_commitment": {
          "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
          "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          }
        }
      }
    ],
    "members": {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/members"
        },
        "create": {
          "href": "/api/lists/{list_id}/members"
        }
      }
    },
    "reports": {
      "pdf": {
        "en": "http://example.com/settlements/en/ts/ents.pdf",
        "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
      }
    }
  }
}
```

<h3 id="get__lists_{list_id}_settlement_preview-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Settlement Preview Resource|[SettlementPreview](#schemasettlementpreview)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-settles">settles</h1>

Settles on List

<a href="https://bitbucket.org/ttyams/wbw-ios/wiki/Settle">Settle on Wiki</a>

## get__lists_{list_id}_settles

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists/{list_id}/settles`

Fetch a collection of Settles on a List. 

<h3 id="get__lists_{list_id}_settles-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the List|

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "settle": {
        "id": "2e7a3940-ab1d-435d-88be-eeacffc1f75d",
        "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
        "created_by": {
          "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
          "created_at": 0,
          "updated_at": 0,
          "nickname": "string",
          "email": "string",
          "full_name": "string",
          "state": "string",
          "iban": "string",
          "is_current": true,
          "avatar_urls": {},
          "member_id": {}
        },
        "list_name": "Casa Crotta",
        "unique_name_attributes": {
          "date": "2019-04-01",
          "daily_number": 1
        },
        "created_at": 0,
        "updated_at": 0,
        "commitments": [
          {
            "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
            "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
            "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
            "state": "unpaid",
            "payment_requests": {
              "links": {
                "create": {
                  "href": "/api/commitments/{commitment_id}/payment_requests"
                }
              }
            },
            "amount": {
              "fractional": "10.00",
              "currency": "EUR"
            },
            "paid_at": 0
          }
        ],
        "member_instances": [
          {
            "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
            "created_at": 0,
            "updated_at": 0,
            "nickname": "string",
            "email": "string",
            "full_name": "string",
            "state": "string",
            "iban": "string",
            "is_current": true,
            "avatar_urls": {},
            "member_id": {}
          }
        ],
        "reports": {
          "pdf": {
            "en": "http://example.com/settlements/en/ts/ents.pdf",
            "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
          }
        }
      }
    }
  ]
}
```

<h3 id="get__lists_{list_id}_settles-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Settles|[SettleCollection](#schemasettlecollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__lists_{list_id}_settles

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/settles',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /lists/{list_id}/settles`

Create a Settle

<h3 id="post__lists_{list_id}_settles-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the List|

> Example responses

> 200 Response

```json
{
  "settle": {
    "id": "2e7a3940-ab1d-435d-88be-eeacffc1f75d",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "created_by": {
      "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
      "created_at": 0,
      "updated_at": 0,
      "nickname": "string",
      "email": "string",
      "full_name": "string",
      "state": "string",
      "iban": "string",
      "is_current": true,
      "avatar_urls": {},
      "member_id": {}
    },
    "list_name": "Casa Crotta",
    "unique_name_attributes": {
      "date": "2019-04-01",
      "daily_number": 1
    },
    "created_at": 0,
    "updated_at": 0,
    "commitments": [
      {
        "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
        "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
        "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
        "state": "unpaid",
        "payment_requests": {
          "links": {
            "create": {
              "href": "/api/commitments/{commitment_id}/payment_requests"
            }
          }
        },
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "paid_at": 0
      }
    ],
    "member_instances": [
      {
        "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
        "created_at": 0,
        "updated_at": 0,
        "nickname": "string",
        "email": "string",
        "full_name": "string",
        "state": "string",
        "iban": "string",
        "is_current": true,
        "avatar_urls": {},
        "member_id": {}
      }
    ],
    "reports": {
      "pdf": {
        "en": "http://example.com/settlements/en/ts/ents.pdf",
        "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
      }
    }
  }
}
```

<h3 id="post__lists_{list_id}_settles-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Settle|[SettleContainer](#schemasettlecontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__settles_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/settles/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/settles/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/settles/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/settles/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/settles/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /settles/{id}`

Fetch single Settle.

<h3 id="get__settles_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Settle|

> Example responses

> 200 Response

```json
{
  "settle": {
    "id": "2e7a3940-ab1d-435d-88be-eeacffc1f75d",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "created_by": {
      "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
      "created_at": 0,
      "updated_at": 0,
      "nickname": "string",
      "email": "string",
      "full_name": "string",
      "state": "string",
      "iban": "string",
      "is_current": true,
      "avatar_urls": {},
      "member_id": {}
    },
    "list_name": "Casa Crotta",
    "unique_name_attributes": {
      "date": "2019-04-01",
      "daily_number": 1
    },
    "created_at": 0,
    "updated_at": 0,
    "commitments": [
      {
        "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
        "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
        "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
        "state": "unpaid",
        "payment_requests": {
          "links": {
            "create": {
              "href": "/api/commitments/{commitment_id}/payment_requests"
            }
          }
        },
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "paid_at": 0
      }
    ],
    "member_instances": [
      {
        "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
        "created_at": 0,
        "updated_at": 0,
        "nickname": "string",
        "email": "string",
        "full_name": "string",
        "state": "string",
        "iban": "string",
        "is_current": true,
        "avatar_urls": {},
        "member_id": {}
      }
    ],
    "reports": {
      "pdf": {
        "en": "http://example.com/settlements/en/ts/ents.pdf",
        "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
      }
    }
  }
}
```

<h3 id="get__settles_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a Settles|[SettleContainer](#schemasettlecontainer)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__commitments_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/commitments/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/commitments/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/commitments/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/commitments/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/commitments/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /commitments/{id}`

Update a commitment: mark as (un)paid

<h3 id="patch__commitments_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the Commitment|

> Example responses

> 200 Response

```json
{
  "commitment": {
    "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
    "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
    "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
    "state": "unpaid",
    "payment_requests": {
      "links": {
        "create": {
          "href": "/api/commitments/{commitment_id}/payment_requests"
        }
      }
    },
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "paid_at": 0
  }
}
```

<h3 id="patch__commitments_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a Commitment|[CommitmentContainer](#schemacommitmentcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request. E.g. when marking a paid commitment as paid|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-lists">lists</h1>

Lists Operations

<a href="https://bitbucket.org/ttyams/wbw-ios/wiki/List">Lists on Wiki</a>

## get__lists

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists`

Lists are the the objects that represent a list of
expenses, which are shared by its members. E.g. a list would be the
'Lunch List at Acme HQ'. Its Members would be 'Employees that share
Lunch at Acme HQ'. And a lunch-expense, would be 'Pizza delivery to Acme
HQ'.

All Lists resources are only accessible to signed in users.

A collection of Lists is set up according to the the general Synching
idea. This means that it is paged, has ordering and allows filtering.

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "list": {
        "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
        "name": "Casa Crotta",
        "created_at": 0,
        "updated_at": 0,
        "currency": "EUR",
        "my_balance": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "lowest": {
          "member": {
            "nickname": "Daan"
          },
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          }
        },
        "image": {
          "links": {
            "update": {
              "href": "/api/lists/{list_id}/image"
            },
            "delete": {
              "href": "/api/lists/{list_id}/image"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png"
          }
        },
        "balance": {
          "links": {
            "current": {
              "href": "/lists/{list_id}/balance"
            }
          }
        },
        "configuration": {
          "links": {
            "current": {
              "href": "/lists/{list_id}/configuration"
            },
            "update": {
              "href": "/lists/{list_id}/configuration"
            }
          }
        },
        "members": [
          {
            "links": {
              "current": {
                "href": "/api/lists/{list_id}/members"
              },
              "create": {
                "href": "/api/lists/{list_id}/members"
              }
            }
          }
        ],
        "groups": [
          {
            "links": {
              "current": {
                "href": "/api/lists/{list_id}/groups"
              },
              "create": {
                "href": "/api/lists/{list_id}/groups"
              }
            }
          }
        ],
        "settles": [
          {
            "links": {
              "current": {
                "href": "/api/lists/{list_id}/settles"
              },
              "create": {
                "href": "/api/lists/{list_id}/settles"
              }
            }
          }
        ]
      }
    }
  ]
}
```

<h3 id="get__lists-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Lists|[ListCollection](#schemalistcollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__lists

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/lists HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/lists \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/lists',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /lists`

Creating a list requires a signed_in user. The new list will be
assigned to the current_user by the backend.

> Body parameter

```json
{
  "name": "string",
  "currency": "EUR"
}
```

<h3 id="post__lists-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ListInput](#schemalistinput)|true|Object to create a list|

> Example responses

> 200 Response

```json
{
  "list": {
    "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "name": "Casa Crotta",
    "created_at": 0,
    "updated_at": 0,
    "currency": "EUR",
    "my_balance": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "lowest": {
      "member": {
        "nickname": "Daan"
      },
      "amount": {
        "fractional": "10.00",
        "currency": "EUR"
      }
    },
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "balance": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/balance"
        }
      }
    },
    "configuration": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/configuration"
        },
        "update": {
          "href": "/lists/{list_id}/configuration"
        }
      }
    },
    "members": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/members"
          },
          "create": {
            "href": "/api/lists/{list_id}/members"
          }
        }
      }
    ],
    "groups": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/groups"
          },
          "create": {
            "href": "/api/lists/{list_id}/groups"
          }
        }
      }
    ],
    "settles": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/settles"
          },
          "create": {
            "href": "/api/lists/{list_id}/settles"
          }
        }
      }
    ]
  }
}
```

<h3 id="post__lists-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Lists|[ListContainer](#schemalistcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__lists_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists/{id}`

Returns the details for a list, as well as pointers to its relations.
The relations members, groups and settles contains a links section
like any collection, but it does not have the data; it is a
slim-collection: merely a pointer at the collection.

<h3 id="get__lists_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the List|

> Example responses

> 200 Response

```json
{
  "list": {
    "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "name": "Casa Crotta",
    "created_at": 0,
    "updated_at": 0,
    "currency": "EUR",
    "my_balance": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "lowest": {
      "member": {
        "nickname": "Daan"
      },
      "amount": {
        "fractional": "10.00",
        "currency": "EUR"
      }
    },
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "balance": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/balance"
        }
      }
    },
    "configuration": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/configuration"
        },
        "update": {
          "href": "/lists/{list_id}/configuration"
        }
      }
    },
    "members": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/members"
          },
          "create": {
            "href": "/api/lists/{list_id}/members"
          }
        }
      }
    ],
    "groups": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/groups"
          },
          "create": {
            "href": "/api/lists/{list_id}/groups"
          }
        }
      }
    ],
    "settles": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/settles"
          },
          "create": {
            "href": "/api/lists/{list_id}/settles"
          }
        }
      }
    ]
  }
}
```

<h3 id="get__lists_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A List|[ListContainer](#schemalistcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__lists_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/lists/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/lists/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/lists/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /lists/{id}`

Update a List

> Body parameter

```json
{
  "name": "string",
  "currency": "EUR"
}
```

<h3 id="patch__lists_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the List|
|body|body|[ListInput](#schemalistinput)|true|none|

> Example responses

> 200 Response

```json
{
  "list": {
    "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "name": "Casa Crotta",
    "created_at": 0,
    "updated_at": 0,
    "currency": "EUR",
    "my_balance": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "lowest": {
      "member": {
        "nickname": "Daan"
      },
      "amount": {
        "fractional": "10.00",
        "currency": "EUR"
      }
    },
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "balance": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/balance"
        }
      }
    },
    "configuration": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/configuration"
        },
        "update": {
          "href": "/lists/{list_id}/configuration"
        }
      }
    },
    "members": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/members"
          },
          "create": {
            "href": "/api/lists/{list_id}/members"
          }
        }
      }
    ],
    "groups": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/groups"
          },
          "create": {
            "href": "/api/lists/{list_id}/groups"
          }
        }
      }
    ],
    "settles": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/settles"
          },
          "create": {
            "href": "/api/lists/{list_id}/settles"
          }
        }
      }
    ]
  }
}
```

<h3 id="patch__lists_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A List|[ListContainer](#schemalistcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__lists_{id}_image

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/lists/{id}/image HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: multipart/form-data
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{id}/image");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'multipart/form-data',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{id}/image',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/lists/{id}/image \
  -H 'Content-Type: multipart/form-data' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'multipart/form-data',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/lists/{id}/image',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /lists/{id}/image`

Attach image to List

> Body parameter

```yaml
image: string

```

<h3 id="patch__lists_{id}_image-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the List|
|body|body|[patchExpenses_id_image](#schemapatchexpenses_id_image)|false|none|
|» image|body|string(binary)|true|none|

> Example responses

> 200 Response

```json
{
  "links": {
    "update": {
      "href": "/api/lists/{list_id}/image"
    },
    "delete": {
      "href": "/api/lists/{list_id}/image"
    }
  },
  "image": {
    "original": "http://example.com/images/image.png",
    "large": "http://example.com/images/large_image.png",
    "medium": "http://example.com/images/medium_image.png",
    "small": "http://example.com/images/small_image.png"
  }
}
```

<h3 id="patch__lists_{id}_image-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The Image object|[ListImage](#schemalistimage)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__lists_{id}_image

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/lists/{id}/image HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{id}/image");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{id}/image',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/lists/{id}/image \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/lists/{id}/image',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /lists/{id}/image`

Delete Image from list, emptying the list image

<h3 id="delete__lists_{id}_image-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the List|

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__lists_{id}_image-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A empty response|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-invitations">invitations</h1>

Invitations to join the list

## get__invitations_{token}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/invitations/{token} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/invitations/{token}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/invitations/{token}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/invitations/{token} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/invitations/{token}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /invitations/{token}`

Show Invitation

<h3 id="get__invitations_{token}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|path|string|true|Invitation Token|

> Example responses

> 200 Response

```json
{
  "links": null,
  "invitation": {
    "created_at": 0,
    "updated_at": 0,
    "invitation_type": "list",
    "state": "invited",
    "expires_at": 0,
    "url": "https://links.example.com/lists/{token}",
    "token": "FRLyMTp-d85qNxaHLgzs",
    "inviter_summary": {
      "name": "Daan de Jong",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "invitee_summary": {
      "id": "3678cace-d2f9-4e26-b16e-2d81f4040725",
      "email": "daan@example.com",
      "nickname": "dann",
      "state": "invited",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "list_summary": {
      "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
      "name": "My List",
      "currency": "EUR",
      "created_at": 0,
      "updated_at": 0,
      "image": {
        "links": {
          "update": {
            "href": "/api/lists/{list_id}/image"
          },
          "delete": {
            "href": "/api/lists/{list_id}/image"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "member_summaries": [
        {
          "links": {
            "first": {
              "href": "/api/link/to/current?page=1"
            },
            "last": {
              "href": "/api/link/to/current?&page=20"
            },
            "next": {
              "href": "/api/link/to/current?&page=3"
            },
            "previous": {
              "href": "/api/link/to/current?&page=1"
            },
            "current": {
              "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
              "meta": {
                "total_pages": 20,
                "current_page": 3,
                "offset": 60,
                "per_page": 30,
                "total_entries": 588,
                "sorted_by_fields": {
                  "name": "desc",
                  "created_at": "asc"
                },
                "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
                "sortable_fields": [
                  "name",
                  "created_at",
                  "modified_at"
                ]
              }
            }
          },
          "data": [
            {
              "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
              "nickname": "daantjepaantje",
              "avatar": {
                "links": {
                  "update": {
                    "href": "/api/users/{user_id}/avatar"
                  },
                  "delete": {
                    "href": "/api/users/{user_id}/avatar"
                  }
                },
                "image": {
                  "original": "http://example.com/images/image.png",
                  "large": "http://example.com/images/large_image.png",
                  "medium": "http://example.com/images/medium_image.png",
                  "small": "http://example.com/images/small_image.png"
                }
              },
              "state": "anonymous"
            }
          ]
        }
      ]
    }
  }
}
```

<h3 id="get__invitations_{token}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An invitation|[InvitationContainer](#schemainvitationcontainer)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not Found. When no invitation for this token exists|None|

<aside class="success">
This operation does not require authentication
</aside>

## patch__invitations_{token}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/invitations/{token} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/invitations/{token}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/invitations/{token}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/invitations/{token} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/invitations/{token}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /invitations/{token}`

Update Invitation: Accept invitation

> Body parameter

```json
{
  "invitation": {
    "member_id": "7e-035c-4f67-9f2f-2719b16e5094"
  }
}
```

<h3 id="patch__invitations_{token}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|path|string|true|ID of the invitation. Communicated as a secret token|
|body|body|[AcceptInvitationInputContainer](#schemaacceptinvitationinputcontainer)|true|none|

#### Detailed descriptions

**token**: ID of the invitation. Communicated as a secret token
          over the mail.

> Example responses

> 200 Response

```json
{
  "links": null,
  "invitation": {
    "created_at": 0,
    "updated_at": 0,
    "invitation_type": "list",
    "state": "invited",
    "expires_at": 0,
    "url": "https://links.example.com/lists/{token}",
    "token": "FRLyMTp-d85qNxaHLgzs",
    "inviter_summary": {
      "name": "Daan de Jong",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "invitee_summary": {
      "id": "3678cace-d2f9-4e26-b16e-2d81f4040725",
      "email": "daan@example.com",
      "nickname": "dann",
      "state": "invited",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "list_summary": {
      "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
      "name": "My List",
      "currency": "EUR",
      "created_at": 0,
      "updated_at": 0,
      "image": {
        "links": {
          "update": {
            "href": "/api/lists/{list_id}/image"
          },
          "delete": {
            "href": "/api/lists/{list_id}/image"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "member_summaries": [
        {
          "links": {
            "first": {
              "href": "/api/link/to/current?page=1"
            },
            "last": {
              "href": "/api/link/to/current?&page=20"
            },
            "next": {
              "href": "/api/link/to/current?&page=3"
            },
            "previous": {
              "href": "/api/link/to/current?&page=1"
            },
            "current": {
              "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
              "meta": {
                "total_pages": 20,
                "current_page": 3,
                "offset": 60,
                "per_page": 30,
                "total_entries": 588,
                "sorted_by_fields": {
                  "name": "desc",
                  "created_at": "asc"
                },
                "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
                "sortable_fields": [
                  "name",
                  "created_at",
                  "modified_at"
                ]
              }
            }
          },
          "data": [
            {
              "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
              "nickname": "daantjepaantje",
              "avatar": {
                "links": {
                  "update": {
                    "href": "/api/users/{user_id}/avatar"
                  },
                  "delete": {
                    "href": "/api/users/{user_id}/avatar"
                  }
                },
                "image": {
                  "original": "http://example.com/images/image.png",
                  "large": "http://example.com/images/large_image.png",
                  "medium": "http://example.com/images/medium_image.png",
                  "small": "http://example.com/images/small_image.png"
                }
              },
              "state": "anonymous"
            }
          ]
        }
      ]
    }
  }
}
```

<h3 id="patch__invitations_{token}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An invitation|[InvitationContainer](#schemainvitationcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__invitations_{token}

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/invitations/{token} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/invitations/{token}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/invitations/{token}',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/invitations/{token} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/invitations/{token}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /invitations/{token}`

Remove invitation: Decline an invitation as anonymous

<h3 id="delete__invitations_{token}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|token|path|string|true|ID of the invitation. Communicated as a secret token|

#### Detailed descriptions

**token**: ID of the invitation. Communicated as a secret token
          over the mail

> Example responses

> 401 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__invitations_{token}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Member invitation was removed|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## put__members_{member_id}_invitation

> Code samples

```http
PUT https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation',
  method: 'put',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PUT https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.put 'https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PUT /members/{member_id}/invitation`

Replace Invitation: Reinvite

> Body parameter

```json
{
  "invitation": {
    "email": "otherdaan@example.com"
  }
}
```

<h3 id="put__members_{member_id}_invitation-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|member_id|path|string|true|ID of the Member|
|body|body|[InvitationInputContainer](#schemainvitationinputcontainer)|true|none|

> Example responses

> 200 Response

```json
{
  "links": null,
  "invitation": {
    "created_at": 0,
    "updated_at": 0,
    "invitation_type": "list",
    "state": "invited",
    "expires_at": 0,
    "url": "https://links.example.com/lists/{token}",
    "token": "FRLyMTp-d85qNxaHLgzs",
    "inviter_summary": {
      "name": "Daan de Jong",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "invitee_summary": {
      "id": "3678cace-d2f9-4e26-b16e-2d81f4040725",
      "email": "daan@example.com",
      "nickname": "dann",
      "state": "invited",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "list_summary": {
      "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
      "name": "My List",
      "currency": "EUR",
      "created_at": 0,
      "updated_at": 0,
      "image": {
        "links": {
          "update": {
            "href": "/api/lists/{list_id}/image"
          },
          "delete": {
            "href": "/api/lists/{list_id}/image"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "member_summaries": [
        {
          "links": {
            "first": {
              "href": "/api/link/to/current?page=1"
            },
            "last": {
              "href": "/api/link/to/current?&page=20"
            },
            "next": {
              "href": "/api/link/to/current?&page=3"
            },
            "previous": {
              "href": "/api/link/to/current?&page=1"
            },
            "current": {
              "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
              "meta": {
                "total_pages": 20,
                "current_page": 3,
                "offset": 60,
                "per_page": 30,
                "total_entries": 588,
                "sorted_by_fields": {
                  "name": "desc",
                  "created_at": "asc"
                },
                "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
                "sortable_fields": [
                  "name",
                  "created_at",
                  "modified_at"
                ]
              }
            }
          },
          "data": [
            {
              "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
              "nickname": "daantjepaantje",
              "avatar": {
                "links": {
                  "update": {
                    "href": "/api/users/{user_id}/avatar"
                  },
                  "delete": {
                    "href": "/api/users/{user_id}/avatar"
                  }
                },
                "image": {
                  "original": "http://example.com/images/image.png",
                  "large": "http://example.com/images/large_image.png",
                  "medium": "http://example.com/images/medium_image.png",
                  "small": "http://example.com/images/small_image.png"
                }
              },
              "state": "anonymous"
            }
          ]
        }
      ]
    }
  }
}
```

<h3 id="put__members_{member_id}_invitation-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|An invitation|[InvitationContainer](#schemainvitationcontainer)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__members_{member_id}_invitation

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/members/{member_id}/invitation',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /members/{member_id}/invitation`

Remove Invitation: Authorized user retracts invitation

<h3 id="delete__members_{member_id}_invitation-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|member_id|path|string|true|ID of the Member|

> Example responses

> 400 Response

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}
```

<h3 id="delete__members_{member_id}_invitation-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Member invitation was removed|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__lists_{list_id}_invitations

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/lists/{list_id}/invitations HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/invitations");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/invitations',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/lists/{list_id}/invitations \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/invitations',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /lists/{list_id}/invitations`

Create a list invitation

<h3 id="post__lists_{list_id}_invitations-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|ID of the list|

> Example responses

> 201 Response

```json
{
  "links": null,
  "invitation": {
    "created_at": 0,
    "updated_at": 0,
    "invitation_type": "list",
    "state": "active",
    "expires_at": 0,
    "url": "https://links.example.com/lists/{token}",
    "token": "FRLyMTp-d85qNxaHLgzs",
    "inviter_summary": {
      "name": "Daan de Jong",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "invitee_summary": "null",
    "list_summary": {
      "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
      "name": "My List",
      "currency": "EUR",
      "created_at": 0,
      "updated_at": 0,
      "image": {
        "links": {
          "update": {
            "href": "/api/lists/{list_id}/image"
          },
          "delete": {
            "href": "/api/lists/{list_id}/image"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "member_summaries": [
        {
          "links": {
            "first": {
              "href": "/api/link/to/current?page=1"
            },
            "last": {
              "href": "/api/link/to/current?&page=20"
            },
            "next": {
              "href": "/api/link/to/current?&page=3"
            },
            "previous": {
              "href": "/api/link/to/current?&page=1"
            },
            "current": {
              "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
              "meta": {
                "total_pages": 20,
                "current_page": 3,
                "offset": 60,
                "per_page": 30,
                "total_entries": 588,
                "sorted_by_fields": {
                  "name": "desc",
                  "created_at": "asc"
                },
                "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
                "sortable_fields": [
                  "name",
                  "created_at",
                  "modified_at"
                ]
              }
            }
          },
          "data": [
            {
              "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
              "nickname": "daantjepaantje",
              "avatar": {
                "links": {
                  "update": {
                    "href": "/api/users/{user_id}/avatar"
                  },
                  "delete": {
                    "href": "/api/users/{user_id}/avatar"
                  }
                },
                "image": {
                  "original": "http://example.com/images/image.png",
                  "large": "http://example.com/images/large_image.png",
                  "medium": "http://example.com/images/medium_image.png",
                  "small": "http://example.com/images/small_image.png"
                }
              },
              "state": "anonymous"
            }
          ]
        }
      ]
    }
  }
}
```

<h3 id="post__lists_{list_id}_invitations-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|A list invitation|[ListInvitationContainer](#schemalistinvitationcontainer)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-push_notifications">push_notifications</h1>

Push Notification resources: Devices, settings etc.

## get__devices

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/devices HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/devices");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/devices',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/devices \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/devices',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /devices`

Devices are the objects that represent a device of a user.
A user can register multiple devices, ie. an 'iPhone 7S' and an
'iPad 4'. The name should reflect the name of the device.

Devices are uniquely identified by their id. The
id is the id as given by the devices OS (identifierForVendor on
iOS; Secure.ANDROID_ID on Android). So the IDs are NOT generated by
the backend. Therefore, no POSTs are allowed, only PUT/PATCHs.

Each device can hold an fcm_token (Firebase Cloud Message token).
When a message is pushed to the user it will be pushed to all the
registered devices for this user.

All Devices resources are only accessible to signed in users.

Each update action resets the user property (owner) of the device to the
logged in user.

A collection of Devices is set up according to the the general Synching
idea. This means that it is paged, has ordering and allows filtering.

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "links": {
        "current": {
          "href": "/api/devices/18036265-3e4f-4a3e-9f3b-211cddb0e7f9"
        },
        "update": {
          "href": "/api/devices/7b6725d8-96ae-4181-90b5-266c833ebead"
        }
      },
      "device": {
        "id": "e157f443-70f6-40d0-99f1-372e8f9e7c69",
        "name": "iPhone 7S",
        "fcm_token": "xxxx-yyyy-...",
        "user_id": "7c51c41e-5fa3-4ec1-9b80-37ddbd9b06a5",
        "created_at": 0,
        "updated_at": 0
      }
    }
  ]
}
```

<h3 id="get__devices-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Devices|[DeviceCollection](#schemadevicecollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__devices_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/devices/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/devices/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/devices/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/devices/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/devices/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /devices/{id}`

Returns the details for a device.

<h3 id="get__devices_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|External ID of the Device|

> Example responses

> 200 Response

```json
{
  "links": {
    "current": {
      "href": "/api/devices/18036265-3e4f-4a3e-9f3b-211cddb0e7f9"
    },
    "update": {
      "href": "/api/devices/7b6725d8-96ae-4181-90b5-266c833ebead"
    }
  },
  "device": {
    "id": "e157f443-70f6-40d0-99f1-372e8f9e7c69",
    "name": "iPhone 7S",
    "fcm_token": "xxxx-yyyy-...",
    "user_id": "7c51c41e-5fa3-4ec1-9b80-37ddbd9b06a5",
    "created_at": 0,
    "updated_at": 0
  }
}
```

<h3 id="get__devices_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Device|[DeviceContainer](#schemadevicecontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__devices_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/devices/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/devices/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/devices/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/devices/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/devices/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /devices/{id}`

Updating a device requires a signed_in user. The device will be
assigned to the current_user by the backend, even if the device belonged
to another user.

If {device_id} is given in the device payload; the
corresponding device is used (if it exists) instead of looking up the
device by the ID param. If both are given; {device_id} has precedence
over {id}.

> Body parameter

```json
{
  "device": {
    "name": "string",
    "fcm_token": "string"
  }
}
```

<h3 id="patch__devices_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|External ID of the Device|
|body|body|[DeviceInput](#schemadeviceinput)|true|none|

> Example responses

> 200 Response

```json
{
  "links": {
    "current": {
      "href": "/api/devices/18036265-3e4f-4a3e-9f3b-211cddb0e7f9"
    },
    "update": {
      "href": "/api/devices/7b6725d8-96ae-4181-90b5-266c833ebead"
    }
  },
  "device": {
    "id": "e157f443-70f6-40d0-99f1-372e8f9e7c69",
    "name": "iPhone 7S",
    "fcm_token": "xxxx-yyyy-...",
    "user_id": "7c51c41e-5fa3-4ec1-9b80-37ddbd9b06a5",
    "created_at": 0,
    "updated_at": 0
  }
}
```

<h3 id="patch__devices_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Device|[DeviceContainer](#schemadevicecontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__lists_{list_id}_configuration

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /lists/{list_id}/configuration`

Notification Settings on the list

<h3 id="get__lists_{list_id}_configuration-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|Id for the list the Configuration applies to|

> Example responses

> 200 Response

```json
{
  "configuration": {
    "id": "xxx-yyy-zzz",
    "list_id": "xxx-yyy-zzz",
    "notifications": "relevant"
  }
}
```

<h3 id="get__lists_{list_id}_configuration-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Configuration for the current_user on this list|[ListConfigurationContainer](#schemalistconfigurationcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__lists_{list_id}_configuration

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/lists/{list_id}/configuration',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /lists/{list_id}/configuration`

Update Notification Settings on the list

> Body parameter

```json
{
  "configuration": {
    "notifications": "relevant"
  }
}
```

<h3 id="patch__lists_{list_id}_configuration-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|list_id|path|string|true|Id for the list the Configuration applies to|
|body|body|[ListConfigurationInputContainer](#schemalistconfigurationinputcontainer)|true|none|

> Example responses

> 200 Response

```json
{
  "configuration": {
    "id": "xxx-yyy-zzz",
    "list_id": "xxx-yyy-zzz",
    "notifications": "relevant"
  }
}
```

<h3 id="patch__lists_{list_id}_configuration-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Configuration for the current_user on this list|[ListConfigurationContainer](#schemalistconfigurationcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-financial_accounts">financial_accounts</h1>

Financial Accounts

## get__financial_accounts

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/financial_accounts HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/financial_accounts");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/financial_accounts',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/financial_accounts \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/financial_accounts',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /financial_accounts`

Financial accounts are the the objects that represent the banking
accounts of a user. E.g. a financial account would be an IBAN.

All Financial accounts resources are only accessible to signed in users.

A collection of Financial accounts is set up according to the the
general Synching idea. This means that it is paged, has ordering and
allows filtering.

> Example responses

> 200 Response

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "financial_account": {
        "id": "5b3f19e5-b875-4550-8723-192799dd1091",
        "publish_details": true,
        "payment_method": "ManualIBAN",
        "iban": "NL39 RABO 0300 0652 64",
        "account_holder": "D.R.W. De Jong en Zonen B.V.",
        "created_at": 1554119930,
        "updated_at": 1554119930,
        "state": "pending",
        "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
        "oauth_intent": "splitser://example/status",
        "oauth_platform": "android",
        "external_account_id": "test@paypal.com"
      }
    }
  ]
}
```

<h3 id="get__financial_accounts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Collection of Financial Accounts|[FinancialAccountCollection](#schemafinancialaccountcollection)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__financial_accounts

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/financial_accounts HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/financial_accounts");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/financial_accounts',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/financial_accounts \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/financial_accounts',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /financial_accounts`

Register a new FinancialAccount for the current user.
If payment method is Paypal no other params are required.

> Body parameter

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android"
  }
}
```

<h3 id="post__financial_accounts-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[FinancialAccountContainer](#schemafinancialaccountcontainer)|true|FinancialAccount to be registered|

> Example responses

> 201 Response

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "created_at": 1554119930,
    "updated_at": 1554119930,
    "state": "pending",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "external_account_id": "test@paypal.com"
  }
}
```

<h3 id="post__financial_accounts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|A FinancialAccount object|[FinancialAccountContainer](#schemafinancialaccountcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__financial_accounts_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/financial_accounts/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/financial_accounts/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /financial_accounts/{id}`

Show details of a FinancialAccount. If the owner
publishes his or her details then IBAN and account holder will be
returned. If the current user is the owner of the financial account
then the details are always returned.

<h3 id="get__financial_accounts_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the FinancialAccount|

> Example responses

> 200 Response

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "created_at": 1554119930,
    "updated_at": 1554119930,
    "state": "pending",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "external_account_id": "test@paypal.com"
  }
}
```

<h3 id="get__financial_accounts_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A FinancialAccount|[FinancialAccountContainer](#schemafinancialaccountcontainer)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## patch__financial_accounts_{id}

> Code samples

```http
PATCH https://stagingwiebetaaltwat.nl/api/financial_accounts/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PATCH");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}',
  method: 'patch',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PATCH https://stagingwiebetaaltwat.nl/api/financial_accounts/{id} \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.patch 'https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PATCH /financial_accounts/{id}`

Update FinancialAccount

> Body parameter

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android"
  }
}
```

<h3 id="patch__financial_accounts_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the FinancialAccount|
|body|body|[FinancialAccountContainer](#schemafinancialaccountcontainer)|true|FinancialAccount to be changed|

> Example responses

> 200 Response

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "created_at": 1554119930,
    "updated_at": 1554119930,
    "state": "pending",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "external_account_id": "test@paypal.com"
  }
}
```

<h3 id="patch__financial_accounts_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A FinancialAccount object|[FinancialAccountContainer](#schemafinancialaccountcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## delete__financial_accounts_{id}

> Code samples

```http
DELETE https://stagingwiebetaaltwat.nl/api/financial_accounts/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}',
  method: 'delete',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X DELETE https://stagingwiebetaaltwat.nl/api/financial_accounts/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.delete 'https://stagingwiebetaaltwat.nl/api/financial_accounts/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`DELETE /financial_accounts/{id}`

Destroy FinancialAccount

<h3 id="delete__financial_accounts_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the FinancialAccount|

> Example responses

> 200 Response

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "created_at": 1554119930,
    "updated_at": 1554119930,
    "state": "pending",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "external_account_id": "test@paypal.com"
  }
}
```

<h3 id="delete__financial_accounts_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The destroyed financialAccount object|[FinancialAccountContainer](#schemafinancialaccountcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad request|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__paypal_financial_accounts_{id}_oauth_data

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_data HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_data");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_data',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_data \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_data',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /paypal/financial_accounts/{id}/oauth_data`

The endpoint is being used to fetch ouath data after the paypal
account has been verified.

The verification process may be conducted on any available
platform (android, ios, web), but it always end up on a web view.
Thus, frontend client needs to redirect to a proper platform
and a proper place (intent). This data is stored while the account
is being created. It needs to be fetched on a separate call, as
we are not able to authenticate the user on this call.

<h3 id="get__paypal_financial_accounts_{id}_oauth_data-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the FinancialAccount|

> Example responses

> 200 Response

```json
{
  "financial_account": {
    "id": "e14fc85c-7d29-46a2-a800-c93cca199f36",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "state": "pending"
  }
}
```

<h3 id="get__paypal_financial_accounts_{id}_oauth_data-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Financial Account oauth data|[FinancialAccountOauthDataContainer](#schemafinancialaccountoauthdatacontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|The account doesn't exist|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## put__paypal_financial_accounts_{id}_oauth_url

> Code samples

```http
PUT https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_url HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_url");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("PUT");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_url',
  method: 'put',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X PUT https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_url \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.put 'https://stagingwiebetaaltwat.nl/api/paypal/financial_accounts/{id}/oauth_url',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`PUT /paypal/financial_accounts/{id}/oauth_url`

The endpoint is being used to renew oauth url.

Once the client finds out the paypal account isn't active,
it has to conduct the LIPP process again. To start the process
the new oauth_url has to be created. After this the client
redirects user to the oauth_url to start the LIPP process.

<h3 id="put__paypal_financial_accounts_{id}_oauth_url-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|ID of the FinancialAccount|

> Example responses

> 200 Response

```json
{
  "financial_account": {
    "id": "fd7f20bb-bcff-4bf5-899b-91a9c0996cdb",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize"
  }
}
```

<h3 id="put__paypal_financial_accounts_{id}_oauth_url-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Financial Account oauth url|[FinancialAccountOauthUrlContainer](#schemafinancialaccountoauthurlcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|The account doesn't exist|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-payment_request">payment_request</h1>

Payment Requests

## post__commitments_{id}_payment_requests

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/commitments/{id}/payment_requests HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/commitments/{id}/payment_requests");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/commitments/{id}/payment_requests',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/commitments/{id}/payment_requests \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/commitments/{id}/payment_requests',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /commitments/{id}/payment_requests`

Create payment request

<h3 id="post__commitments_{id}_payment_requests-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Commitment's ID|

> Example responses

> 201 Response

```json
{
  "links": {
    "current": {
      "href": "/api/payment_requests/"
    }
  },
  "payment_request": {
    "id": null,
    "description": "My list 1",
    "payable_id": "3ddf4f23-f6c8-4c5c-b52e-15ad5c33e143",
    "payable_type": "Commitment",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payer": {
      "links": {
        "current": {
          "href": "/api/user/{id}"
        },
        "user": {
          "id": "b87b2083-8b9d-4c23-86e4-c734ea4d6ce3"
        }
      }
    },
    "beneficiary": {
      "links": {
        "current": {
          "href": "/api/user/{id}"
        },
        "user": {
          "id": "38afa928-cc67-4396-accf-f903b53039de"
        }
      }
    },
    "token": null,
    "url": "links.wiebetaaltwat.nl/payment_requests/",
    "state": "pending",
    "created_at": 1554119930,
    "updated_at": 1554119930
  }
}
```

<h3 id="post__commitments_{id}_payment_requests-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Payment Request which was created|[PaymentRequestContainer](#schemapaymentrequestcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden access|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## get__payment_requests_{id}

> Code samples

```http
GET https://stagingwiebetaaltwat.nl/api/payment_requests/{id} HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/payment_requests/{id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/payment_requests/{id}',
  method: 'get',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X GET https://stagingwiebetaaltwat.nl/api/payment_requests/{id} \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://stagingwiebetaaltwat.nl/api/payment_requests/{id}',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`GET /payment_requests/{id}`

Fetch payment request

<h3 id="get__payment_requests_{id}-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Payment request's ID|

> Example responses

> 200 Response

```json
{
  "links": {
    "current": {
      "href": "/api/payment_requests/"
    }
  },
  "payment_request": {
    "id": null,
    "description": "My list 1",
    "payable_id": "3ddf4f23-f6c8-4c5c-b52e-15ad5c33e143",
    "payable_type": "Commitment",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payer": {
      "links": {
        "current": {
          "href": "/api/user/{id}"
        },
        "user": {
          "id": "b87b2083-8b9d-4c23-86e4-c734ea4d6ce3"
        }
      }
    },
    "beneficiary": {
      "links": {
        "current": {
          "href": "/api/user/{id}"
        },
        "user": {
          "id": "38afa928-cc67-4396-accf-f903b53039de"
        }
      }
    },
    "token": null,
    "url": "links.wiebetaaltwat.nl/payment_requests/",
    "state": "pending",
    "created_at": 1554119930,
    "updated_at": 1554119930
  }
}
```

<h3 id="get__payment_requests_{id}-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Payment Request|[PaymentRequestContainer](#schemapaymentrequestcontainer)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden access|[ErrorModel](#schemaerrormodel)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not found|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="wie-betaalt-wat-api-payment">payment</h1>

## post__funding_options

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/funding_options HTTP/1.1
Host: stagingwiebetaaltwat.nl
Content-Type: application/json
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/funding_options");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Content-Type':'application/json',
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/funding_options',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/funding_options \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/funding_options',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /funding_options`

Create FundingOptions

> Body parameter

```json
{
  "funding_option": {
    "amount": {
      "fractional": "1000",
      "currency": "EUR"
    }
  }
}
```

<h3 id="post__funding_options-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[FundingOptionCreator](#schemafundingoptioncreator)|true|Payment information where funding options are for|

> Example responses

> 201 Response

```json
{
  "links": null,
  "data": [
    {
      "funding_option": {
        "id": "OPTION_ID_4"
      }
    }
  ]
}
```

<h3 id="post__funding_options-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Collection of FundingOptions for a payment|[FundingOptionCollection](#schemafundingoptioncollection)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden access|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

## post__payments

> Code samples

```http
POST https://stagingwiebetaaltwat.nl/api/payments HTTP/1.1
Host: stagingwiebetaaltwat.nl
Accept: application/json

```

```java
URL obj = new URL("https://stagingwiebetaaltwat.nl/api/payments");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```javascript
var headers = {
  'Accept':'application/json'

};

$.ajax({
  url: 'https://stagingwiebetaaltwat.nl/api/payments',
  method: 'post',

  headers: headers,
  success: function(data) {
    console.log(JSON.stringify(data));
  }
})

```

```shell
# You can also use wget
curl -X POST https://stagingwiebetaaltwat.nl/api/payments \
  -H 'Accept: application/json'

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.post 'https://stagingwiebetaaltwat.nl/api/payments',
  params: {
  }, headers: headers

p JSON.parse(result)

```

`POST /payments`

Create payment

> Example responses

> 201 Response

```json
{
  "links": {
    "current": {
      "href": "/api/payments/9246e1a8-ed90-4b38-adc8-a538b5371d25"
    }
  },
  "payment": {
    "id": "9246e1a8-ed90-4b38-adc8-a538b5371d25",
    "description": "My list 1",
    "payable_id": "bb9ddd77-cc37-4638-a746-b49d45b8aac5",
    "payable_type": "Settle::Commitment",
    "original_amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "exchange_rate": "0.124",
    "payer_id": "ee2e2a34-3f6b-4b3f-a42d-2896a0a67497",
    "beneficiary_id": "00e3a571-d384-4a44-84c0-0d67173c9be7",
    "funding_option_id": "45f25889ff02c3e5dbf27d575dbeaeea0df487ca9ffdb46e7fe4af0f457980e5",
    "payment_token": "01951436",
    "created_at": 1554119930,
    "updated_at": 1554119930
  }
}
```

<h3 id="post__payments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Payment which was created|[PaymentContainer](#schemapaymentcontainer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|[ErrorModel](#schemaerrormodel)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|[ErrorModel](#schemaerrormodel)|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|Forbidden access|[ErrorModel](#schemaerrormodel)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocSamount">Amount</h2>

<a id="schemaamount"></a>

```json
{
  "fractional": "10.00",
  "currency": "EUR"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fractional|number(double)|true|none|none|
|currency|string|true|none|none|

<h2 id="tocSamountinput">AmountInput</h2>

<a id="schemaamountinput"></a>

```json
1337

```

*Amount in whole units.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|Amount in whole units.|
|*anonymous*|any|false|none|none|

<h2 id="tocSbalance">Balance</h2>

<a id="schemabalance"></a>

```json
{
  "links": {
    "current": "/api/balance/01dbc53a-6447-48eb-a086-8b7bb0b34fd4"
  },
  "balance": {
    "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "total": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "member_totals": [
      {
        "links": {
          "member": "/api/balance/00e5120c-c06a-4539-bba0-3bc7f748aa8c"
        },
        "member_total": {
          "member": {
            "id": "00e5120c-c06a-4539-bba0-3bc7f748aa8c",
            "nickname": "daan",
            "is_current": false,
            "avatar_urls": {
              "links": {
                "update": {
                  "href": "/api/users/{user_id}/avatar"
                },
                "delete": {
                  "href": "/api/users/{user_id}/avatar"
                }
              },
              "image": {
                "original": "http://example.com/images/image.png",
                "large": "http://example.com/images/large_image.png",
                "medium": "http://example.com/images/medium_image.png",
                "small": "http://example.com/images/small_image.png"
              }
            }
          },
          "balance_total": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "balance_indicator": 0,
          "expense_total": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "cost_total": {
            "fractional": "10.00",
            "currency": "EUR"
          }
        }
      }
    ]
  }
}

```

*Balance model*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» current|any|false|none|Link to this Balance|
|balance|object|false|none|none|
|» id|string|false|none|Because this is a representation of a List, this               is the UUID of the List|
|» list_id|string|false|none|List ID this balance represents.|
|» total|[Amount](#schemaamount)|false|none|Total amount of all Balances|
|» member_totals|[[MemberTotal](#schemamembertotal)]|false|none|[MemberTotal. Stands in for a Member as represented on           a balance]|

<h2 id="tocSmembertotal">MemberTotal</h2>

<a id="schemamembertotal"></a>

```json
{
  "links": {
    "member": "/api/balance/00e5120c-c06a-4539-bba0-3bc7f748aa8c"
  },
  "member_total": {
    "member": {
      "id": "00e5120c-c06a-4539-bba0-3bc7f748aa8c",
      "nickname": "daan",
      "is_current": false,
      "avatar_urls": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "balance_total": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "balance_indicator": 0,
    "expense_total": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "cost_total": {
      "fractional": "10.00",
      "currency": "EUR"
    }
  }
}

```

*MemberTotal. Stands in for a Member as represented on
          a balance*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» member|any|false|none|Link to the Member who has this total|
|member_total|object|false|none|none|
|» member|object|false|none|none|
|»» id|string|false|none|ID of the member for this Member|
|»» nickname|string|false|none|nickname of the member for this Member|
|»» is_current|boolean|false|none|Indicates whether this member represents the                 currently logged in user.|
|»» avatar_urls|[Avatar](#schemaavatar)|false|none|none|
|» balance_total|[Amount](#schemaamount)|false|none|The balance of this member. This  basically is               expense_total minus cost_total|
|» balance_indicator|integer|false|none|The relative balance of this member. It shows               which part all the balances, this member_totals' balance_total is.               In the UI this is shown as a green (positive) or red (negative)               bar.               Since this is a rounded, whole number, the total may be a little               off from 100%.|
|» expense_total|[Amount](#schemaamount)|false|none|Total amount of expenses payed by this Member|
|» cost_total|[Amount](#schemaamount)|false|none|Total amount of the shares of expenses for this               Member.|

<h2 id="tocScollectionlinks">CollectionLinks</h2>

<a id="schemacollectionlinks"></a>

```json
{
  "first": {
    "href": "/api/link/to/current?page=1"
  },
  "last": {
    "href": "/api/link/to/current?&page=20"
  },
  "next": {
    "href": "/api/link/to/current?&page=3"
  },
  "previous": {
    "href": "/api/link/to/current?&page=1"
  },
  "current": {
    "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
    "meta": {
      "total_pages": 20,
      "current_page": 3,
      "offset": 60,
      "per_page": 30,
      "total_entries": 588,
      "sorted_by_fields": {
        "name": "desc",
        "created_at": "asc"
      },
      "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
      "sortable_fields": [
        "name",
        "created_at",
        "modified_at"
      ]
    }
  }
}

```

*Contains paths to actions that can be taken on this
          collection. When a path is missing, or nil/null, then you cannot
          take that action. E.g. when the *next* path is empty, there is no
          next page. E.g. when the *create* path is emtpy in an Expense
          Collection, you are not allowed to create a new expense.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|first|object|false|none|Path to the first page in the paged collection|
|last|object|false|none|Path to the last page in the paged collection.             might be the same as first.|
|next|object|false|none|Path to the next page in the paged collection.|
|previous|object|false|none|Path to the previous page in the paged collection.|
|current|object|false|none|Canonical path to self|
|» href|any|false|none|none|
|» meta|object|false|none|Metadata about this collection. Usefull when               synching paged collections with a local database or when building               a pager in your client.|
|»» total_pages|integer|false|none|The total amount of pages|
|»» current_page|integer|false|none|Number of the current page|
|»» offset|integer|false|none|Amount of items in the collection on the pages                 before this one. So when on page 3 with 30 items per page,                 a total of 60 items are on the pages 1 and 2.|
|»» per_page|integer|false|none|Amount of items on one page, when the page                 is full. E.g. the last page might contain less.|
|»» total_entries|integer|false|none|Total amount of entries in all the pages.|
|»» sorted_by_fields|object|false|none|Fields and direction of sorting of the current                 collection.|
|»» sorted_by_query|string|false|none|Fields and direction of sorting of the current                 collection. In string form|
|»» sortable_fields|any|false|none|List of fields that we can sort by|

<h2 id="tocSdevicecontainer">DeviceContainer</h2>

<a id="schemadevicecontainer"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/devices/18036265-3e4f-4a3e-9f3b-211cddb0e7f9"
    },
    "update": {
      "href": "/api/devices/7b6725d8-96ae-4181-90b5-266c833ebead"
    }
  },
  "device": {
    "id": "e157f443-70f6-40d0-99f1-372e8f9e7c69",
    "name": "iPhone 7S",
    "fcm_token": "xxxx-yyyy-...",
    "user_id": "7c51c41e-5fa3-4ec1-9b80-37ddbd9b06a5",
    "created_at": 0,
    "updated_at": 0
  }
}

```

*Device model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[Links](#schemalinks)|false|none|Links to a device|
|device|[Device](#schemadevice)|false|none|A device. Not to be confused with a Collection.           A Device is an actual model, and within WBW this is the entity that           the users manage.|

<h2 id="tocSdeviceinputcontainer">DeviceInputContainer</h2>

<a id="schemadeviceinputcontainer"></a>

```json
{
  "device": {
    "device": {
      "name": "string",
      "fcm_token": "string"
    }
  }
}

```

*Device model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|device|[DeviceInput](#schemadeviceinput)|false|none|none|

<h2 id="tocSdevicecollection">DeviceCollection</h2>

<a id="schemadevicecollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "links": {
        "current": {
          "href": "/api/devices/18036265-3e4f-4a3e-9f3b-211cddb0e7f9"
        },
        "update": {
          "href": "/api/devices/7b6725d8-96ae-4181-90b5-266c833ebead"
        }
      },
      "device": {
        "id": "e157f443-70f6-40d0-99f1-372e8f9e7c69",
        "name": "iPhone 7S",
        "fcm_token": "xxxx-yyyy-...",
        "user_id": "7c51c41e-5fa3-4ec1-9b80-37ddbd9b06a5",
        "created_at": 0,
        "updated_at": 0
      }
    }
  ]
}

```

*A Collection of Devices.
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[DeviceContainer](#schemadevicecontainer)]|false|none|[Device model container]|

<h2 id="tocSlinks">Links</h2>

<a id="schemalinks"></a>

```json
{
  "current": {
    "href": "/api/devices/18036265-3e4f-4a3e-9f3b-211cddb0e7f9"
  },
  "update": {
    "href": "/api/devices/7b6725d8-96ae-4181-90b5-266c833ebead"
  }
}

```

*Links to a device*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|current|object|false|none|Path to the current device|
|update|object|false|none|Path to the current device|

<h2 id="tocSdevice">Device</h2>

<a id="schemadevice"></a>

```json
{
  "id": "e157f443-70f6-40d0-99f1-372e8f9e7c69",
  "name": "iPhone 7S",
  "fcm_token": "xxxx-yyyy-...",
  "user_id": "7c51c41e-5fa3-4ec1-9b80-37ddbd9b06a5",
  "created_at": 0,
  "updated_at": 0
}

```

*A device. Not to be confused with a Collection.
          A Device is an actual model, and within WBW this is the entity that
          the users manage.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|name|string|true|none|none|
|fcm_token|string|false|none|none|
|user_id|string|false|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|

<h2 id="tocSdeviceinput">DeviceInput</h2>

<a id="schemadeviceinput"></a>

```json
{
  "device": {
    "name": "string",
    "fcm_token": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|device|object|false|none|none|
|» name|string|true|none|none|
|» fcm_token|string|false|none|none|

<h2 id="tocSerrormodel">ErrorModel</h2>

<a id="schemaerrormodel"></a>

```json
{
  "errors": {
    "name_of_errored_field": [
      "Name is too short",
      "Name is already taken"
    ],
    "name_of_another_errored_field": [
      "Member is not allowed"
    ]
  },
  "message": "Name is too short"
}

```

*An Error is presented whenever the client or user
          made a mistake. The error is JSON and should contain enough
          information to be able to fix the error.
          All response codes in the 4xx range should return such a response.
          When they do not, that is a bug and should be reported.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|errors|object|false|none|An object with name-value pairs representing the             errors on the fields. The *key* is the fieldname, the value, a list             of errors on that field.             A special case is the field *base*, which is not an actual field,             or property, but contains a list of errors on the generic request.|
|» name_of_errored_field|any|false|none|Name of the errored field|
|» name_of_another_errored_field|any|false|none|Another errored field|
|message|string|false|none|The first error from the error messages.|

<h2 id="tocSexpensecontainer">ExpenseContainer</h2>

<a id="schemaexpensecontainer"></a>

```json
{
  "expense": {
    "id": "27f6b16d-93fe-409c-b5e4-844b376da9a4",
    "name": "Burritos",
    "list_id": "4950b38b-c187-4668-a760-ed86663076c3",
    "settle_id": "c6ab38e9-ae89-4171-9e02-d3545ae687c2",
    "payed_on": "YYYY-MM-DD",
    "created_at": 0,
    "updated_at": 0,
    "status": "active",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payed_by_id": "b762d775-8ad0-4895-a209-40199c0f1154",
    "payed_by_member_instance_id": "0d2e3980-d8cc-475d-9fc5-aa1c588b4f80",
    "payed_by": {
      "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
      "nickname": "daantjepaantje",
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "state": "anonymous"
    },
    "shares": [
      {
        "share": {
          "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
          "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          },
          "multiplier": 0
        }
      }
    ],
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png",
        "micro": "http://example.com/images/micro_image.png"
      }
    }
  }
}

```

*Expense model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|expense|[Expense](#schemaexpense)|true|none|Expense model|

<h2 id="tocSexpense">Expense</h2>

<a id="schemaexpense"></a>

```json
{
  "id": "27f6b16d-93fe-409c-b5e4-844b376da9a4",
  "name": "Burritos",
  "list_id": "4950b38b-c187-4668-a760-ed86663076c3",
  "settle_id": "c6ab38e9-ae89-4171-9e02-d3545ae687c2",
  "payed_on": "YYYY-MM-DD",
  "created_at": 0,
  "updated_at": 0,
  "status": "active",
  "amount": {
    "fractional": "10.00",
    "currency": "EUR"
  },
  "payed_by_id": "b762d775-8ad0-4895-a209-40199c0f1154",
  "payed_by_member_instance_id": "0d2e3980-d8cc-475d-9fc5-aa1c588b4f80",
  "payed_by": {
    "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
    "nickname": "daantjepaantje",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "state": "anonymous"
  },
  "shares": [
    {
      "share": {
        "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
        "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "multiplier": 0
      }
    }
  ],
  "image": {
    "links": {
      "update": {
        "href": "/api/lists/{list_id}/image"
      },
      "delete": {
        "href": "/api/lists/{list_id}/image"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png",
      "micro": "http://example.com/images/micro_image.png"
    }
  }
}

```

*Expense model*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|name|string|true|none|none|
|list_id|string|true|none|none|
|settle_id|string|false|none|When settled, this id refers to the settle thatincludes this expense|
|payed_on|string|true|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|status|string|false|none|none|
|amount|[Amount](#schemaamount)|true|none|none|
|payed_by_id|string|true|none|ID of the member who payed this expense|
|payed_by_member_instance_id|string|false|none|ID of the member-instance who payed this expense when this expense is settled|
|payed_by|[MemberSummary](#schemamembersummary)|false|none|Summarized Member model|
|shares|[[Share](#schemashare)]|true|none|[Share model container]|
|image|[ExpenseImage](#schemaexpenseimage)|false|none|An image on an Expense|

#### Enumerated Values

|Property|Value|
|---|---|
|status|active|
|status|deleted|

<h2 id="tocSexpensesummary">ExpenseSummary</h2>

<a id="schemaexpensesummary"></a>

```json
{
  "expense": {
    "id": "31365abb-325a-4d42-b452-3324a22d83b1",
    "name": "Burritos",
    "list_id": "77c5b285-05f0-4d5b-87d1-f040a89debdd",
    "settle_id": "f6872a02-b892-4e74-85f6-a69c938adbfd",
    "payed_on": "YYYY-MM-DD",
    "created_at": 0,
    "updated_at": 0,
    "status": "active",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payed_by_id": "7256ce38-42ba-4125-a70d-2c812ddb718c",
    "participant_names": [
      "irene",
      "Daan"
    ],
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png",
        "micro": "http://example.com/images/micro_image.png"
      }
    }
  }
}

```

*Expense model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|expense|object|false|none|Expense model in a Collection. Does not contain all             attributes.|
|» id|string|false|none|none|
|» name|string|true|none|none|
|» list_id|string|false|none|none|
|» settle_id|string|false|none|When settled, this id refers to the settle thatincludes this expense|
|» payed_on|string|true|none|none|
|» created_at|integer|false|none|none|
|» updated_at|integer|false|none|none|
|» status|string|false|none|none|
|» amount|[Amount](#schemaamount)|true|none|none|
|» payed_by_id|string|true|none|ID of the member who payed this expense|
|» participant_names|[any]|false|none|none|
|» image|[ExpenseImage](#schemaexpenseimage)|false|none|An image on an Expense|

#### Enumerated Values

|Property|Value|
|---|---|
|status|active|
|status|deleted|

<h2 id="tocSexpensereplaceinput">ExpenseReplaceInput</h2>

<a id="schemaexpensereplaceinput"></a>

```json
{
  "expense": {
    "name": "Burritos",
    "payed_by_id": "80937715-dee4-4c55-80c2-6845a66d6c1f",
    "payed_on": "2016-02-18",
    "amount": 1337,
    "shares": [
      {
        "amount": 1337,
        "member_id": "a89629ec-2457-48ae-a334-73522a113941",
        "multiplier": 0
      }
    ]
  }
}

```

*Expense model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|expense|object|false|none|Expense input|
|» name|string|true|none|none|
|» payed_by_id|string|true|none|ID of the member who payed this expense|
|» payed_on|string|true|none|Date (not time) the expense was payed on.                 Thist must be a JSON date, which is an ISO 8601 with Julian                 year.|
|» amount|[AmountInput](#schemaamountinput)|true|none|Amount in whole units.|
|» shares|[[ShareReplaceInput](#schemasharereplaceinput)]|true|none|none|

<h2 id="tocSexpenseinput">ExpenseInput</h2>

<a id="schemaexpenseinput"></a>

```json
{
  "expense": {
    "name": "Burritos",
    "payed_by_id": "25d2391f-45dd-4d21-9e80-be54e04ea07c",
    "payed_on": "2016-02-18",
    "amount": 1337,
    "shares_attributes": [
      {
        "amount": 1337,
        "id": "7bb760f0-8a7a-434d-aabc-d7925631806a",
        "member_id": "a70c1aa6-10fd-4ccb-85be-77f70fe1430c",
        "multiplier": 0,
        "_destroy": 0
      }
    ]
  }
}

```

*Expense model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|expense|object|false|none|Expense input|
|» name|string|true|none|none|
|» payed_by_id|string|true|none|ID of the member who payed this expense|
|» payed_on|string|true|none|Date (not time) the expense was payed on.                 Thist must be a JSON date, which is an ISO 8601 with Julian                 year.|
|» amount|[AmountInput](#schemaamountinput)|true|none|Amount in whole units.|
|» shares_attributes|[[ShareInput](#schemashareinput)]|true|none|none|

<h2 id="tocSshare">Share</h2>

<a id="schemashare"></a>

```json
{
  "share": {
    "id": "0a06b817-9f64-4e61-beb2-75b183ae3f83",
    "member_id": "8ac56c6e-96ab-40af-8985-0757e1f5edf2",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "multiplier": 0
  }
}

```

*Share model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|share|object|false|none|A Share registers the Amount a Member shares in an             Expense|
|» id|string|false|none|ID of the Share. Provide in case you wish to               update an existing share instead of adding a new one.|
|» member_id|string|true|none|ID of the member who owns this Share. Each               Expense can have a member_id at most once.|
|» amount|[Amount](#schemaamount)|true|none|none|
|» multiplier|integer|false|none|An integer that represents how much times the               member participated with a share. This is not used in the backend.               The amount is only used. This is provided for use in the               interface|

<h2 id="tocSsharereplaceinput">ShareReplaceInput</h2>

<a id="schemasharereplaceinput"></a>

```json
{
  "amount": 1337,
  "member_id": "a89629ec-2457-48ae-a334-73522a113941",
  "multiplier": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|[AmountInput](#schemaamountinput)|true|none|Amount in whole units.|
|member_id|string|true|none|ID of the member who owns this Share. Each Expense             can have a member_id at most once.|
|multiplier|integer|false|none|An integer that represents how much times the             member participated with a share. This is not used in the backend.             The amount is only used. This is provided for use in the interface|

<h2 id="tocSshareinput">ShareInput</h2>

<a id="schemashareinput"></a>

```json
{
  "amount": 1337,
  "id": "7bb760f0-8a7a-434d-aabc-d7925631806a",
  "member_id": "a70c1aa6-10fd-4ccb-85be-77f70fe1430c",
  "multiplier": 0,
  "_destroy": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|amount|[AmountInput](#schemaamountinput)|true|none|Amount in whole units.|
|id|string|false|none|ID of the Share. Provide in case you wish to update            an existing share instead of adding a new one.|
|member_id|string|true|none|ID of the member who owns this Share. Each Expense             can have a member_id at most once.|
|multiplier|integer|false|none|An integer that represents how much times the             member participated with a share. This is not used in the backend.             The amount is only used. This is provided for use in the interface|
|_destroy|integer|false|none|Set to 1 in order to remove this Share from the             expense. In that case you *must* also provide the share ID.|

<h2 id="tocSexpensecollection">ExpenseCollection</h2>

<a id="schemaexpensecollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "expense": {
        "id": "31365abb-325a-4d42-b452-3324a22d83b1",
        "name": "Burritos",
        "list_id": "77c5b285-05f0-4d5b-87d1-f040a89debdd",
        "settle_id": "f6872a02-b892-4e74-85f6-a69c938adbfd",
        "payed_on": "YYYY-MM-DD",
        "created_at": 0,
        "updated_at": 0,
        "status": "active",
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "payed_by_id": "7256ce38-42ba-4125-a70d-2c812ddb718c",
        "participant_names": [
          "irene",
          "Daan"
        ],
        "image": {
          "links": {
            "update": {
              "href": "/api/lists/{list_id}/image"
            },
            "delete": {
              "href": "/api/lists/{list_id}/image"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png",
            "micro": "http://example.com/images/micro_image.png"
          }
        }
      }
    }
  ]
}

```

*A Collection of Expenses.
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[ExpenseSummary](#schemaexpensesummary)]|false|none|[Expense model container]|

<h2 id="tocSleangroupcollection">LeanGroupCollection</h2>

<a id="schemaleangroupcollection"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/lists/{list_id}/groups"
    },
    "create": {
      "href": "/api/lists/{list_id}/groups"
    }
  }
}

```

*A pointer to a Collection of Groups*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» current|object|false|none|none|
|»» href|any|false|none|none|
|» create|object|false|none|none|
|»» href|any|false|none|none|

<h2 id="tocSavatar">Avatar</h2>

<a id="schemaavatar"></a>

```json
{
  "links": {
    "update": {
      "href": "/api/users/{user_id}/avatar"
    },
    "delete": {
      "href": "/api/users/{user_id}/avatar"
    }
  },
  "image": {
    "original": "http://example.com/images/image.png",
    "large": "http://example.com/images/large_image.png",
    "medium": "http://example.com/images/medium_image.png",
    "small": "http://example.com/images/small_image.png"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» update|object|false|none|none|
|»» href|any|false|none|none|
|» delete|object|false|none|none|
|»» href|any|false|none|Removes the avatar and reverts to default                                 avatar|
|» image|any|true|none|Contains links to variations of images, when               available. Empty, or NULL links mean that this variation has no               image, or that the expense has no image (yet).               Avatars have defaults, so in case a user has not yet uploaded               an image or when she has deleted it, the links point to the               autogenerated default avatars.|

<h2 id="tocSlistimage">ListImage</h2>

<a id="schemalistimage"></a>

```json
{
  "links": {
    "update": {
      "href": "/api/lists/{list_id}/image"
    },
    "delete": {
      "href": "/api/lists/{list_id}/image"
    }
  },
  "image": {
    "original": "http://example.com/images/image.png",
    "large": "http://example.com/images/large_image.png",
    "medium": "http://example.com/images/medium_image.png",
    "small": "http://example.com/images/small_image.png"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» update|object|false|none|none|
|»» href|any|false|none|none|
|» delete|object|false|none|none|
|»» href|any|false|none|Removes the image from the list|
|» image|any|true|none|Contains links to variations of images, when               available. Empty, or NULL links mean that this variation has no               image, or that the expense has no image (yet).|

<h2 id="tocSexpenseimage">ExpenseImage</h2>

<a id="schemaexpenseimage"></a>

```json
{
  "links": {
    "update": {
      "href": "/api/lists/{list_id}/image"
    },
    "delete": {
      "href": "/api/lists/{list_id}/image"
    }
  },
  "image": {
    "original": "http://example.com/images/image.png",
    "large": "http://example.com/images/large_image.png",
    "medium": "http://example.com/images/medium_image.png",
    "small": "http://example.com/images/small_image.png",
    "micro": "http://example.com/images/micro_image.png"
  }
}

```

*An image on an Expense*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» update|object|false|none|none|
|»» href|any|false|none|none|
|» delete|object|false|none|none|
|»» href|any|false|none|Removes the image from the list|
|» image|any|true|none|Contains links to variations of images, when               available. Empty, or NULL links mean that this variation has no               image, or that the expense has no image (yet).|

<h2 id="tocSinvitationcontainer">InvitationContainer</h2>

<a id="schemainvitationcontainer"></a>

```json
{
  "links": null,
  "invitation": {
    "created_at": 0,
    "updated_at": 0,
    "invitation_type": "list",
    "state": "invited",
    "expires_at": 0,
    "url": "https://links.example.com/lists/{token}",
    "token": "FRLyMTp-d85qNxaHLgzs",
    "inviter_summary": {
      "name": "Daan de Jong",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "invitee_summary": {
      "id": "3678cace-d2f9-4e26-b16e-2d81f4040725",
      "email": "daan@example.com",
      "nickname": "dann",
      "state": "invited",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "list_summary": {
      "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
      "name": "My List",
      "currency": "EUR",
      "created_at": 0,
      "updated_at": 0,
      "image": {
        "links": {
          "update": {
            "href": "/api/lists/{list_id}/image"
          },
          "delete": {
            "href": "/api/lists/{list_id}/image"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "member_summaries": [
        {
          "links": {
            "first": {
              "href": "/api/link/to/current?page=1"
            },
            "last": {
              "href": "/api/link/to/current?&page=20"
            },
            "next": {
              "href": "/api/link/to/current?&page=3"
            },
            "previous": {
              "href": "/api/link/to/current?&page=1"
            },
            "current": {
              "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
              "meta": {
                "total_pages": 20,
                "current_page": 3,
                "offset": 60,
                "per_page": 30,
                "total_entries": 588,
                "sorted_by_fields": {
                  "name": "desc",
                  "created_at": "asc"
                },
                "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
                "sortable_fields": [
                  "name",
                  "created_at",
                  "modified_at"
                ]
              }
            }
          },
          "data": [
            {
              "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
              "nickname": "daantjepaantje",
              "avatar": {
                "links": {
                  "update": {
                    "href": "/api/users/{user_id}/avatar"
                  },
                  "delete": {
                    "href": "/api/users/{user_id}/avatar"
                  }
                },
                "image": {
                  "original": "http://example.com/images/image.png",
                  "large": "http://example.com/images/large_image.png",
                  "medium": "http://example.com/images/medium_image.png",
                  "small": "http://example.com/images/small_image.png"
                }
              },
              "state": "anonymous"
            }
          ]
        }
      ]
    }
  }
}

```

*Container for Invitation*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|any|false|none|Links for actions on this Resource|
|invitation|[Invitation](#schemainvitation)|false|none|Invitation|

<h2 id="tocSinvitation">Invitation</h2>

<a id="schemainvitation"></a>

```json
{
  "created_at": 0,
  "updated_at": 0,
  "invitation_type": "list",
  "state": "invited",
  "expires_at": 0,
  "url": "https://links.example.com/lists/{token}",
  "token": "FRLyMTp-d85qNxaHLgzs",
  "inviter_summary": {
    "name": "Daan de Jong",
    "created_at": 0,
    "updated_at": 0,
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  },
  "invitee_summary": {
    "id": "3678cace-d2f9-4e26-b16e-2d81f4040725",
    "email": "daan@example.com",
    "nickname": "dann",
    "state": "invited",
    "created_at": 0,
    "updated_at": 0,
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  },
  "list_summary": {
    "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
    "name": "My List",
    "currency": "EUR",
    "created_at": 0,
    "updated_at": 0,
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "member_summaries": [
      {
        "links": {
          "first": {
            "href": "/api/link/to/current?page=1"
          },
          "last": {
            "href": "/api/link/to/current?&page=20"
          },
          "next": {
            "href": "/api/link/to/current?&page=3"
          },
          "previous": {
            "href": "/api/link/to/current?&page=1"
          },
          "current": {
            "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
            "meta": {
              "total_pages": 20,
              "current_page": 3,
              "offset": 60,
              "per_page": 30,
              "total_entries": 588,
              "sorted_by_fields": {
                "name": "desc",
                "created_at": "asc"
              },
              "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
              "sortable_fields": [
                "name",
                "created_at",
                "modified_at"
              ]
            }
          }
        },
        "data": [
          {
            "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
            "nickname": "daantjepaantje",
            "avatar": {
              "links": {
                "update": {
                  "href": "/api/users/{user_id}/avatar"
                },
                "delete": {
                  "href": "/api/users/{user_id}/avatar"
                }
              },
              "image": {
                "original": "http://example.com/images/image.png",
                "large": "http://example.com/images/large_image.png",
                "medium": "http://example.com/images/medium_image.png",
                "small": "http://example.com/images/small_image.png"
              }
            },
            "state": "anonymous"
          }
        ]
      }
    ]
  }
}

```

*Invitation*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|invitation_type|string|false|none|Type of the invitation.|
|state|string|false|none|State of the invitation, depending on the kind of invitation.|
|expires_at|integer|false|none|Date up to wich invitation is active|
|url|string|false|none|url link to accept the list invitation|
|token|string|false|none|Invitation token|
|inviter_summary|[InviterSummary](#schemainvitersummary)|false|none|none|
|invitee_summary|[InviteeSummary](#schemainviteesummary)|false|none|none|
|list_summary|[ListSummary](#schemalistsummary)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|invitation_type|list|
|invitation_type|email|
|invitation_type|oob|
|state|anonymous|
|state|accepted|
|state|invited|
|state|active|
|state|expired|

<h2 id="tocSinvitersummary">InviterSummary</h2>

<a id="schemainvitersummary"></a>

```json
{
  "name": "Daan de Jong",
  "created_at": 0,
  "updated_at": 0,
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|false|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|avatar|[Avatar](#schemaavatar)|false|none|none|

<h2 id="tocSinviteesummary">InviteeSummary</h2>

<a id="schemainviteesummary"></a>

```json
{
  "id": "3678cace-d2f9-4e26-b16e-2d81f4040725",
  "email": "daan@example.com",
  "nickname": "dann",
  "state": "invited",
  "created_at": 0,
  "updated_at": 0,
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|email|string|false|none|Email address to which invitation has been sent|
|nickname|string|false|none|Invited member nickname|
|state|string|false|none|State of the member.|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|avatar|[Avatar](#schemaavatar)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|state|invited|
|state|anonymous|

<h2 id="tocSlistsummary">ListSummary</h2>

<a id="schemalistsummary"></a>

```json
{
  "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
  "name": "My List",
  "currency": "EUR",
  "created_at": 0,
  "updated_at": 0,
  "image": {
    "links": {
      "update": {
        "href": "/api/lists/{list_id}/image"
      },
      "delete": {
        "href": "/api/lists/{list_id}/image"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "member_summaries": [
    {
      "links": {
        "first": {
          "href": "/api/link/to/current?page=1"
        },
        "last": {
          "href": "/api/link/to/current?&page=20"
        },
        "next": {
          "href": "/api/link/to/current?&page=3"
        },
        "previous": {
          "href": "/api/link/to/current?&page=1"
        },
        "current": {
          "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
          "meta": {
            "total_pages": 20,
            "current_page": 3,
            "offset": 60,
            "per_page": 30,
            "total_entries": 588,
            "sorted_by_fields": {
              "name": "desc",
              "created_at": "asc"
            },
            "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
            "sortable_fields": [
              "name",
              "created_at",
              "modified_at"
            ]
          }
        }
      },
      "data": [
        {
          "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
          "nickname": "daantjepaantje",
          "avatar": {
            "links": {
              "update": {
                "href": "/api/users/{user_id}/avatar"
              },
              "delete": {
                "href": "/api/users/{user_id}/avatar"
              }
            },
            "image": {
              "original": "http://example.com/images/image.png",
              "large": "http://example.com/images/large_image.png",
              "medium": "http://example.com/images/medium_image.png",
              "small": "http://example.com/images/small_image.png"
            }
          },
          "state": "anonymous"
        }
      ]
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|name|string|false|none|none|
|currency|string|false|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|image|[ListImage](#schemalistimage)|false|none|none|
|member_summaries|[[MemberSummaryCollection](#schemamembersummarycollection)]|false|none|[A Collection of Member summaries         See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for         more information on Paging and Collections.]|

<h2 id="tocSinvitationinputcontainer">InvitationInputContainer</h2>

<a id="schemainvitationinputcontainer"></a>

```json
{
  "invitation": {
    "email": "otherdaan@example.com"
  }
}

```

*Container for Invitation Input.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|invitation|[InvitationInput](#schemainvitationinput)|false|none|Invitation input|

<h2 id="tocSinvitationinput">InvitationInput</h2>

<a id="schemainvitationinput"></a>

```json
{
  "email": "otherdaan@example.com"
}

```

*Invitation input*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|email|string|false|none|New email to reinvite this member on|

<h2 id="tocSacceptinvitationinputcontainer">AcceptInvitationInputContainer</h2>

<a id="schemaacceptinvitationinputcontainer"></a>

```json
{
  "invitation": {
    "member_id": "7e-035c-4f67-9f2f-2719b16e5094"
  }
}

```

*Container for Accept Invitation Input.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|invitation|[AcceptInvitationInput](#schemaacceptinvitationinput)|false|none|Accept Invitation input|

<h2 id="tocSacceptinvitationinput">AcceptInvitationInput</h2>

<a id="schemaacceptinvitationinput"></a>

```json
{
  "member_id": "7e-035c-4f67-9f2f-2719b16e5094"
}

```

*Accept Invitation input*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|member_id|string|false|none|Id of a member to bind to a user|

<h2 id="tocSlistcontainer">ListContainer</h2>

<a id="schemalistcontainer"></a>

```json
{
  "list": {
    "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "name": "Casa Crotta",
    "created_at": 0,
    "updated_at": 0,
    "currency": "EUR",
    "my_balance": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "lowest": {
      "member": {
        "nickname": "Daan"
      },
      "amount": {
        "fractional": "10.00",
        "currency": "EUR"
      }
    },
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "balance": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/balance"
        }
      }
    },
    "configuration": {
      "links": {
        "current": {
          "href": "/lists/{list_id}/configuration"
        },
        "update": {
          "href": "/lists/{list_id}/configuration"
        }
      }
    },
    "members": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/members"
          },
          "create": {
            "href": "/api/lists/{list_id}/members"
          }
        }
      }
    ],
    "groups": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/groups"
          },
          "create": {
            "href": "/api/lists/{list_id}/groups"
          }
        }
      }
    ],
    "settles": [
      {
        "links": {
          "current": {
            "href": "/api/lists/{list_id}/settles"
          },
          "create": {
            "href": "/api/lists/{list_id}/settles"
          }
        }
      }
    ]
  }
}

```

*List model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|list|[List](#schemalist)|false|none|A list. Not to be confused with a Collection. A List           is an actual model, and within WBW this is the entity that the users           manage.|

<h2 id="tocSlistinputcontainer">ListInputContainer</h2>

<a id="schemalistinputcontainer"></a>

```json
{
  "list": {
    "name": "string",
    "currency": "EUR"
  }
}

```

*List model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|list|[ListInput](#schemalistinput)|false|none|none|

<h2 id="tocSlistcollection">ListCollection</h2>

<a id="schemalistcollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "list": {
        "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
        "name": "Casa Crotta",
        "created_at": 0,
        "updated_at": 0,
        "currency": "EUR",
        "my_balance": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "lowest": {
          "member": {
            "nickname": "Daan"
          },
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          }
        },
        "image": {
          "links": {
            "update": {
              "href": "/api/lists/{list_id}/image"
            },
            "delete": {
              "href": "/api/lists/{list_id}/image"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png"
          }
        },
        "balance": {
          "links": {
            "current": {
              "href": "/lists/{list_id}/balance"
            }
          }
        },
        "configuration": {
          "links": {
            "current": {
              "href": "/lists/{list_id}/configuration"
            },
            "update": {
              "href": "/lists/{list_id}/configuration"
            }
          }
        },
        "members": [
          {
            "links": {
              "current": {
                "href": "/api/lists/{list_id}/members"
              },
              "create": {
                "href": "/api/lists/{list_id}/members"
              }
            }
          }
        ],
        "groups": [
          {
            "links": {
              "current": {
                "href": "/api/lists/{list_id}/groups"
              },
              "create": {
                "href": "/api/lists/{list_id}/groups"
              }
            }
          }
        ],
        "settles": [
          {
            "links": {
              "current": {
                "href": "/api/lists/{list_id}/settles"
              },
              "create": {
                "href": "/api/lists/{list_id}/settles"
              }
            }
          }
        ]
      }
    }
  ]
}

```

*A Collection of Lists.
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[ListContainer](#schemalistcontainer)]|false|none|[List model container]|

<h2 id="tocSlist">List</h2>

<a id="schemalist"></a>

```json
{
  "id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
  "name": "Casa Crotta",
  "created_at": 0,
  "updated_at": 0,
  "currency": "EUR",
  "my_balance": {
    "fractional": "10.00",
    "currency": "EUR"
  },
  "lowest": {
    "member": {
      "nickname": "Daan"
    },
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    }
  },
  "image": {
    "links": {
      "update": {
        "href": "/api/lists/{list_id}/image"
      },
      "delete": {
        "href": "/api/lists/{list_id}/image"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "balance": {
    "links": {
      "current": {
        "href": "/lists/{list_id}/balance"
      }
    }
  },
  "configuration": {
    "links": {
      "current": {
        "href": "/lists/{list_id}/configuration"
      },
      "update": {
        "href": "/lists/{list_id}/configuration"
      }
    }
  },
  "members": [
    {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/members"
        },
        "create": {
          "href": "/api/lists/{list_id}/members"
        }
      }
    }
  ],
  "groups": [
    {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/groups"
        },
        "create": {
          "href": "/api/lists/{list_id}/groups"
        }
      }
    }
  ],
  "settles": [
    {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/settles"
        },
        "create": {
          "href": "/api/lists/{list_id}/settles"
        }
      }
    }
  ]
}

```

*A list. Not to be confused with a Collection. A List
          is an actual model, and within WBW this is the entity that the users
          manage.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|name|string|true|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|currency|string|true|none|ISO 4217 currency code|
|my_balance|[Amount](#schemaamount)|false|none|none|
|lowest|object|false|none|none|
|» member|object|false|none|none|
|»» nickname|string|false|none|none|
|» amount|[Amount](#schemaamount)|false|none|none|
|image|[ListImage](#schemalistimage)|false|none|none|
|balance|any|false|none|Link to balance relation|
|configuration|any|false|none|Link to configuration relation|
|members|[[LeanMemberCollection](#schemaleanmembercollection)]|false|none|[A pointer to a Collection of Members]|
|groups|[[LeanGroupCollection](#schemaleangroupcollection)]|false|none|[A pointer to a Collection of Groups]|
|settles|[[LeanSettleCollection](#schemaleansettlecollection)]|false|none|[A pointer to a Collection of Settles]|

<h2 id="tocSleanexpensescollection">LeanExpensesCollection</h2>

<a id="schemaleanexpensescollection"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/lists/{list_id}/expenses"
    },
    "create": {
      "href": "/api/lists/{list_id}/expenses"
    }
  }
}

```

*A pointer to a Collection of expenses for the list*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» current|object|false|none|none|
|»» href|any|false|none|none|
|» create|object|false|none|none|
|»» href|any|false|none|none|

<h2 id="tocSlistinput">ListInput</h2>

<a id="schemalistinput"></a>

```json
{
  "name": "string",
  "currency": "EUR"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|currency|string|true|none|ISO 4217 currency code. See Wiki for allowed currencies|

<h2 id="tocSlistconfigurationcontainer">ListConfigurationContainer</h2>

<a id="schemalistconfigurationcontainer"></a>

```json
{
  "configuration": {
    "id": "xxx-yyy-zzz",
    "list_id": "xxx-yyy-zzz",
    "notifications": "relevant"
  }
}

```

*List Configuration model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|configuration|[ListConfiguration](#schemalistconfiguration)|false|none|List Configuration model|

<h2 id="tocSlistconfiguration">ListConfiguration</h2>

<a id="schemalistconfiguration"></a>

```json
{
  "id": "xxx-yyy-zzz",
  "list_id": "xxx-yyy-zzz",
  "notifications": "relevant"
}

```

*List Configuration model*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|list_id|string|false|none|none|
|notifications|string|false|none|How many notifications should the user recieve for                              this list? Defaults to `relevant`|

#### Enumerated Values

|Property|Value|
|---|---|
|notifications|none|
|notifications|relevant|
|notifications|all|

<h2 id="tocSlistconfigurationinputcontainer">ListConfigurationInputContainer</h2>

<a id="schemalistconfigurationinputcontainer"></a>

```json
{
  "configuration": {
    "notifications": "relevant"
  }
}

```

*List Configuration model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|configuration|[ListConfigurationInput](#schemalistconfigurationinput)|false|none|List Configuration model for input|

<h2 id="tocSlistconfigurationinput">ListConfigurationInput</h2>

<a id="schemalistconfigurationinput"></a>

```json
{
  "notifications": "relevant"
}

```

*List Configuration model for input*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|notifications|string|false|none|How many notifications should the user recieve for                              this list? Defaults to `relevant`|

#### Enumerated Values

|Property|Value|
|---|---|
|notifications|none|
|notifications|relevant|
|notifications|all|

<h2 id="tocSlistinvitationcontainer">ListInvitationContainer</h2>

<a id="schemalistinvitationcontainer"></a>

```json
{
  "links": null,
  "invitation": {
    "created_at": 0,
    "updated_at": 0,
    "invitation_type": "list",
    "state": "active",
    "expires_at": 0,
    "url": "https://links.example.com/lists/{token}",
    "token": "FRLyMTp-d85qNxaHLgzs",
    "inviter_summary": {
      "name": "Daan de Jong",
      "created_at": 0,
      "updated_at": 0,
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      }
    },
    "invitee_summary": "null",
    "list_summary": {
      "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
      "name": "My List",
      "currency": "EUR",
      "created_at": 0,
      "updated_at": 0,
      "image": {
        "links": {
          "update": {
            "href": "/api/lists/{list_id}/image"
          },
          "delete": {
            "href": "/api/lists/{list_id}/image"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "member_summaries": [
        {
          "links": {
            "first": {
              "href": "/api/link/to/current?page=1"
            },
            "last": {
              "href": "/api/link/to/current?&page=20"
            },
            "next": {
              "href": "/api/link/to/current?&page=3"
            },
            "previous": {
              "href": "/api/link/to/current?&page=1"
            },
            "current": {
              "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
              "meta": {
                "total_pages": 20,
                "current_page": 3,
                "offset": 60,
                "per_page": 30,
                "total_entries": 588,
                "sorted_by_fields": {
                  "name": "desc",
                  "created_at": "asc"
                },
                "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
                "sortable_fields": [
                  "name",
                  "created_at",
                  "modified_at"
                ]
              }
            }
          },
          "data": [
            {
              "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
              "nickname": "daantjepaantje",
              "avatar": {
                "links": {
                  "update": {
                    "href": "/api/users/{user_id}/avatar"
                  },
                  "delete": {
                    "href": "/api/users/{user_id}/avatar"
                  }
                },
                "image": {
                  "original": "http://example.com/images/image.png",
                  "large": "http://example.com/images/large_image.png",
                  "medium": "http://example.com/images/medium_image.png",
                  "small": "http://example.com/images/small_image.png"
                }
              },
              "state": "anonymous"
            }
          ]
        }
      ]
    }
  }
}

```

*Container for List Invitation*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|any|false|none|Links for actions on this Resource|
|invitation|[ListInvitation](#schemalistinvitation)|false|none|List Invitation|

<h2 id="tocSlistinvitation">ListInvitation</h2>

<a id="schemalistinvitation"></a>

```json
{
  "created_at": 0,
  "updated_at": 0,
  "invitation_type": "list",
  "state": "active",
  "expires_at": 0,
  "url": "https://links.example.com/lists/{token}",
  "token": "FRLyMTp-d85qNxaHLgzs",
  "inviter_summary": {
    "name": "Daan de Jong",
    "created_at": 0,
    "updated_at": 0,
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  },
  "invitee_summary": "null",
  "list_summary": {
    "id": "e14a739a-85ee-434d-93e3-eba3cc9cc7fd",
    "name": "My List",
    "currency": "EUR",
    "created_at": 0,
    "updated_at": 0,
    "image": {
      "links": {
        "update": {
          "href": "/api/lists/{list_id}/image"
        },
        "delete": {
          "href": "/api/lists/{list_id}/image"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "member_summaries": [
      {
        "links": {
          "first": {
            "href": "/api/link/to/current?page=1"
          },
          "last": {
            "href": "/api/link/to/current?&page=20"
          },
          "next": {
            "href": "/api/link/to/current?&page=3"
          },
          "previous": {
            "href": "/api/link/to/current?&page=1"
          },
          "current": {
            "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
            "meta": {
              "total_pages": 20,
              "current_page": 3,
              "offset": 60,
              "per_page": 30,
              "total_entries": 588,
              "sorted_by_fields": {
                "name": "desc",
                "created_at": "asc"
              },
              "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
              "sortable_fields": [
                "name",
                "created_at",
                "modified_at"
              ]
            }
          }
        },
        "data": [
          {
            "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
            "nickname": "daantjepaantje",
            "avatar": {
              "links": {
                "update": {
                  "href": "/api/users/{user_id}/avatar"
                },
                "delete": {
                  "href": "/api/users/{user_id}/avatar"
                }
              },
              "image": {
                "original": "http://example.com/images/image.png",
                "large": "http://example.com/images/large_image.png",
                "medium": "http://example.com/images/medium_image.png",
                "small": "http://example.com/images/small_image.png"
              }
            },
            "state": "anonymous"
          }
        ]
      }
    ]
  }
}

```

*List Invitation*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|invitation_type|string|false|none|Type of the invitation.|
|state|string|false|none|State of the invitation.|
|expires_at|integer|false|none|Date up to wich invitation is active|
|url|string|false|none|url link to accept the list invitation|
|token|string|false|none|Invitation token|
|inviter_summary|[InviterSummary](#schemainvitersummary)|false|none|none|
|invitee_summary|any|false|none|Invitee data should be null|
|list_summary|[ListSummary](#schemalistsummary)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|invitation_type|list|
|invitation_type|email|
|invitation_type|oob|
|state|active|
|state|expired|

<h2 id="tocSmembercontainer">MemberContainer</h2>

<a id="schemamembercontainer"></a>

```json
{
  "member": {
    "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com",
    "is_current": false,
    "invitation": {
      "links": null,
      "invitation": {
        "state": "active",
        "url": "https://links.example.com/lists/{token}"
      }
    },
    "created_at": 0,
    "updated_at": 0,
    "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    }
  }
}

```

*Member model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|member|[Member](#schemamember)|false|none|Member model|

<h2 id="tocSmemberinputcontainer">MemberInputContainer</h2>

<a id="schemamemberinputcontainer"></a>

```json
{
  "member": {
    "nickname": "daantjepaantje",
    "email": "daantjepaantje@example.com"
  }
}

```

*Member model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|member|[MemberInput](#schemamemberinput)|false|none|none|

<h2 id="tocSmember">Member</h2>

<a id="schemamember"></a>

```json
{
  "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
  "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
  "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
  "nickname": "daantjepaantje",
  "email": "daantjepaantje@example.com",
  "is_current": false,
  "invitation": {
    "links": null,
    "invitation": {
      "state": "active",
      "url": "https://links.example.com/lists/{token}"
    }
  },
  "created_at": 0,
  "updated_at": 0,
  "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  }
}

```

*Member model*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|list_id|string|false|none|none|
|user_id|string|false|none|When attached to a user (accepted), this contains             the ID of that user.|
|nickname|string|true|none|Nickname this user is known by on this list|
|email|string|false|none|Email for this member on this list. For accepted             members, this is the email for their users. For invited members,             the mail they were invited on and for other members it is empty.|
|is_current|boolean|false|none|Indicates whether this member represents the             currently logged in user.|
|invitation|[MemberInvitationContainer](#schemamemberinvitationcontainer)|false|none|Container for Member Invitation|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|deleted_at|integer|false|none|none|
|avatar|[Avatar](#schemaavatar)|false|none|none|

<h2 id="tocSmembersummary">MemberSummary</h2>

<a id="schemamembersummary"></a>

```json
{
  "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
  "nickname": "daantjepaantje",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "state": "anonymous"
}

```

*Summarized Member model*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|nickname|string|false|none|Nickname this user is known by on this list|
|avatar|[Avatar](#schemaavatar)|false|none|none|
|state|string|false|none|State of the user.             Anonymous means you can pick him             while joining the list|

<h2 id="tocSmemberinput">MemberInput</h2>

<a id="schemamemberinput"></a>

```json
{
  "nickname": "daantjepaantje",
  "email": "daantjepaantje@example.com"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|nickname|string|true|none|Nickname this user is known by on this list|
|email|string|false|none|Email for this member on this list. When omitted             the member will not be invited, but only added to the list as             unsubscribed|

<h2 id="tocSmemberupdateinput">MemberUpdateInput</h2>

<a id="schemamemberupdateinput"></a>

```json
{
  "nickname": "daantjepaantje"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|nickname|string|true|none|Nickname this user is known by on this list|

<h2 id="tocSmembercollection">MemberCollection</h2>

<a id="schemamembercollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "member": {
        "id": "c79a07c3-097e-42e2-804c-ed96712e0941",
        "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
        "user_id": "bf91434f-b21c-4c6a-be43-52a5a33cdd1c",
        "nickname": "daantjepaantje",
        "email": "daantjepaantje@example.com",
        "is_current": false,
        "invitation": {
          "links": null,
          "invitation": {
            "state": "active",
            "url": "https://links.example.com/lists/{token}"
          }
        },
        "created_at": 0,
        "updated_at": 0,
        "deleted_at": "When the member is deleted, this contains the timestamp\n            at which the member was deleted",
        "avatar": {
          "links": {
            "update": {
              "href": "/api/users/{user_id}/avatar"
            },
            "delete": {
              "href": "/api/users/{user_id}/avatar"
            }
          },
          "image": {
            "original": "http://example.com/images/image.png",
            "large": "http://example.com/images/large_image.png",
            "medium": "http://example.com/images/medium_image.png",
            "small": "http://example.com/images/small_image.png"
          }
        }
      }
    }
  ]
}

```

*A Collection of Members
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[MemberContainer](#schemamembercontainer)]|false|none|[Member model container]|

<h2 id="tocSmembersummarycollection">MemberSummaryCollection</h2>

<a id="schemamembersummarycollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "id": "4fd2c3a6-788c-49a2-9703-13131f47fea1",
      "nickname": "daantjepaantje",
      "avatar": {
        "links": {
          "update": {
            "href": "/api/users/{user_id}/avatar"
          },
          "delete": {
            "href": "/api/users/{user_id}/avatar"
          }
        },
        "image": {
          "original": "http://example.com/images/image.png",
          "large": "http://example.com/images/large_image.png",
          "medium": "http://example.com/images/medium_image.png",
          "small": "http://example.com/images/small_image.png"
        }
      },
      "state": "anonymous"
    }
  ]
}

```

*A Collection of Member summaries
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[MemberSummary](#schemamembersummary)]|false|none|[Summarized Member model]|

<h2 id="tocSleanmembercollection">LeanMemberCollection</h2>

<a id="schemaleanmembercollection"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/lists/{list_id}/members"
    },
    "create": {
      "href": "/api/lists/{list_id}/members"
    }
  }
}

```

*A pointer to a Collection of Members*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» current|object|false|none|none|
|»» href|any|false|none|none|
|» create|object|false|none|none|
|»» href|any|false|none|none|

<h2 id="tocSmemberinvitationcontainer">MemberInvitationContainer</h2>

<a id="schemamemberinvitationcontainer"></a>

```json
{
  "links": null,
  "invitation": {
    "state": "active",
    "url": "https://links.example.com/lists/{token}"
  }
}

```

*Container for Member Invitation*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|any|false|none|Links for actions on this Resource|
|invitation|[MemberInvitation](#schemamemberinvitation)|false|none|none|

<h2 id="tocSmemberinvitation">MemberInvitation</h2>

<a id="schemamemberinvitation"></a>

```json
{
  "state": "active",
  "url": "https://links.example.com/lists/{token}"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|state|string|false|none|State of the invitation.|
|url|string|false|none|url link to accept the list invitation|

#### Enumerated Values

|Property|Value|
|---|---|
|state|active|
|state|expired|

<h2 id="tocSsettlecontainer">SettleContainer</h2>

<a id="schemasettlecontainer"></a>

```json
{
  "settle": {
    "id": "2e7a3940-ab1d-435d-88be-eeacffc1f75d",
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "created_by": {
      "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
      "created_at": 0,
      "updated_at": 0,
      "nickname": "string",
      "email": "string",
      "full_name": "string",
      "state": "string",
      "iban": "string",
      "is_current": true,
      "avatar_urls": {},
      "member_id": {}
    },
    "list_name": "Casa Crotta",
    "unique_name_attributes": {
      "date": "2019-04-01",
      "daily_number": 1
    },
    "created_at": 0,
    "updated_at": 0,
    "commitments": [
      {
        "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
        "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
        "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
        "state": "unpaid",
        "payment_requests": {
          "links": {
            "create": {
              "href": "/api/commitments/{commitment_id}/payment_requests"
            }
          }
        },
        "amount": {
          "fractional": "10.00",
          "currency": "EUR"
        },
        "paid_at": 0
      }
    ],
    "member_instances": [
      {
        "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
        "created_at": 0,
        "updated_at": 0,
        "nickname": "string",
        "email": "string",
        "full_name": "string",
        "state": "string",
        "iban": "string",
        "is_current": true,
        "avatar_urls": {},
        "member_id": {}
      }
    ],
    "reports": {
      "pdf": {
        "en": "http://example.com/settlements/en/ts/ents.pdf",
        "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
      }
    }
  }
}

```

*Settle model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|settle|[Settle](#schemasettle)|false|none|Settle model in a Collection.|

<h2 id="tocSsettle">Settle</h2>

<a id="schemasettle"></a>

```json
{
  "id": "2e7a3940-ab1d-435d-88be-eeacffc1f75d",
  "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
  "created_by": {
    "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
    "created_at": 0,
    "updated_at": 0,
    "nickname": "string",
    "email": "string",
    "full_name": "string",
    "state": "string",
    "iban": "string",
    "is_current": true,
    "avatar_urls": {},
    "member_id": {}
  },
  "list_name": "Casa Crotta",
  "unique_name_attributes": {
    "date": "2019-04-01",
    "daily_number": 1
  },
  "created_at": 0,
  "updated_at": 0,
  "commitments": [
    {
      "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
      "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
      "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
      "state": "unpaid",
      "payment_requests": {
        "links": {
          "create": {
            "href": "/api/commitments/{commitment_id}/payment_requests"
          }
        }
      },
      "amount": {
        "fractional": "10.00",
        "currency": "EUR"
      },
      "paid_at": 0
    }
  ],
  "member_instances": [
    {
      "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
      "created_at": 0,
      "updated_at": 0,
      "nickname": "string",
      "email": "string",
      "full_name": "string",
      "state": "string",
      "iban": "string",
      "is_current": true,
      "avatar_urls": {},
      "member_id": {}
    }
  ],
  "reports": {
    "pdf": {
      "en": "http://example.com/settlements/en/ts/ents.pdf",
      "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
    }
  }
}

```

*Settle model in a Collection.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|list_id|string|false|none|none|
|created_by|[MemberInstance](#schemamemberinstance)|true|none|none|
|list_name|string|false|none|none|
|unique_name_attributes|[UniqueNameAttributes](#schemauniquenameattributes)|false|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|commitments|[[Commitment](#schemacommitment)]|false|none|List of commitments. May be empty when the Settle               is not finished being calculated.|
|member_instances|[[MemberInstance](#schemamemberinstance)]|false|none|none|
|reports|[Reports](#schemareports)|false|none|none|

<h2 id="tocSsettlecollection">SettleCollection</h2>

<a id="schemasettlecollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "settle": {
        "id": "2e7a3940-ab1d-435d-88be-eeacffc1f75d",
        "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
        "created_by": {
          "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
          "created_at": 0,
          "updated_at": 0,
          "nickname": "string",
          "email": "string",
          "full_name": "string",
          "state": "string",
          "iban": "string",
          "is_current": true,
          "avatar_urls": {},
          "member_id": {}
        },
        "list_name": "Casa Crotta",
        "unique_name_attributes": {
          "date": "2019-04-01",
          "daily_number": 1
        },
        "created_at": 0,
        "updated_at": 0,
        "commitments": [
          {
            "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
            "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
            "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
            "state": "unpaid",
            "payment_requests": {
              "links": {
                "create": {
                  "href": "/api/commitments/{commitment_id}/payment_requests"
                }
              }
            },
            "amount": {
              "fractional": "10.00",
              "currency": "EUR"
            },
            "paid_at": 0
          }
        ],
        "member_instances": [
          {
            "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
            "created_at": 0,
            "updated_at": 0,
            "nickname": "string",
            "email": "string",
            "full_name": "string",
            "state": "string",
            "iban": "string",
            "is_current": true,
            "avatar_urls": {},
            "member_id": {}
          }
        ],
        "reports": {
          "pdf": {
            "en": "http://example.com/settlements/en/ts/ents.pdf",
            "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
          }
        }
      }
    }
  ]
}

```

*A Collection of Expenses.
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[SettleContainer](#schemasettlecontainer)]|false|none|[Settle model container]|

<h2 id="tocSleansettlecollection">LeanSettleCollection</h2>

<a id="schemaleansettlecollection"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/lists/{list_id}/settles"
    },
    "create": {
      "href": "/api/lists/{list_id}/settles"
    }
  }
}

```

*A pointer to a Collection of Settles*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» current|object|false|none|none|
|»» href|any|false|none|none|
|» create|object|false|none|none|
|»» href|any|false|none|none|

<h2 id="tocScommitmentcontainer">CommitmentContainer</h2>

<a id="schemacommitmentcontainer"></a>

```json
{
  "commitment": {
    "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
    "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
    "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
    "state": "unpaid",
    "payment_requests": {
      "links": {
        "create": {
          "href": "/api/commitments/{commitment_id}/payment_requests"
        }
      }
    },
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "paid_at": 0
  }
}

```

*Commitment model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|commitment|[Commitment](#schemacommitment)|false|none|none|

<h2 id="tocScommitment">Commitment</h2>

<a id="schemacommitment"></a>

```json
{
  "id": "5fa1cc38-1ff2-4eb9-91af-70f98390c4f5",
  "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
  "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
  "state": "unpaid",
  "payment_requests": {
    "links": {
      "create": {
        "href": "/api/commitments/{commitment_id}/payment_requests"
      }
    }
  },
  "amount": {
    "fractional": "10.00",
    "currency": "EUR"
  },
  "paid_at": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|payer_id|string|true|none|The ID of the **MemberInstance** who has to pay               this amount to beneficiary|
|beneficiary_id|string|true|none|The ID of the **MemberInstance** who will receive               this amount from the payer|
|state|string|false|none|payment state|
|payment_requests|object|false|none|none|
|» links|object|false|none|none|
|»» create|object|false|none|none|
|»»» href|any|false|none|Discover if and where a payment_request can                     be made for this Commitment|
|»» amount|[Amount](#schemaamount)|true|none|none|
|»» paid_at|integer|false|none|indicates when the actual payment has been done|

#### Enumerated Values

|Property|Value|
|---|---|
|state|paid|
|state|unpaid|

<h2 id="tocScommitmentinput">CommitmentInput</h2>

<a id="schemacommitmentinput"></a>

```json
{
  "expense": {
    "state": "paid"
  }
}

```

*Commitment input model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|expense|object|false|none|Commitment input|
|» state|string|true|none|payment state|

#### Enumerated Values

|Property|Value|
|---|---|
|state|paid|
|state|unpaid|

<h2 id="tocSmemberinstancecontainer">MemberInstanceContainer</h2>

<a id="schemamemberinstancecontainer"></a>

```json
{
  "member_instance": {
    "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
    "created_at": 0,
    "updated_at": 0,
    "nickname": "string",
    "email": "string",
    "full_name": "string",
    "state": "string",
    "iban": "string",
    "is_current": true,
    "avatar_urls": {},
    "member_id": {}
  }
}

```

*MemberInstance model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|member_instance|[MemberInstance](#schemamemberinstance)|false|none|none|

<h2 id="tocSmemberinstance">MemberInstance</h2>

<a id="schemamemberinstance"></a>

```json
{
  "id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
  "created_at": 0,
  "updated_at": 0,
  "nickname": "string",
  "email": "string",
  "full_name": "string",
  "state": "string",
  "iban": "string",
  "is_current": true,
  "avatar_urls": {},
  "member_id": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|
|nickname|string|true|none|Deferred to member if that exists|
|email|string|false|none|Deferred to member if that exists|
|full_name|string|false|none|Full name, first and lastname when user                              has this provided and member has a user|
|state|string|false|none|Deferred to member if that exists|
|iban|string|false|none|Deferred to member if that exists|
|is_current|boolean|false|none|Deferred to member if that exists|
|avatar_urls|object|false|none|Deferred to member if that exists|
|member_id|object|false|none|ID of a member assosiated to member instance|

<h2 id="tocSuniquenameattributes">UniqueNameAttributes</h2>

<a id="schemauniquenameattributes"></a>

```json
{
  "date": "2019-04-01",
  "daily_number": 1
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|date|string|false|none|Creation Date Iso 8601 formatted|
|daily_number|integer|false|none|Unique (sequential) number per creation date per             list|

<h2 id="tocSreports">Reports</h2>

<a id="schemareports"></a>

```json
{
  "pdf": {
    "en": "http://example.com/settlements/en/ts/ents.pdf",
    "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|pdf|[PDF](#schemapdf)|false|none|none|

<h2 id="tocSpdf">PDF</h2>

<a id="schemapdf"></a>

```json
{
  "en": "http://example.com/settlements/en/ts/ents.pdf",
  "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|en|any|false|none|Link to English PDF report|
|nl|any|false|none|Link to Dutch PDF report              NOTE: The PDF links are generated asynchronously, so             they might not yet exist right after settling.|

<h2 id="tocScurrentusercontainer">CurrentUserContainer</h2>

<a id="schemacurrentusercontainer"></a>

```json
{
  "current_user": {
    "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
    "email": "daan@example.com",
    "unconfirmed_email": "otherdaan@example.com",
    "first_name": "Daan",
    "last_name": "de Jong",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "city": "Rotterdam",
    "state": "new",
    "locale": "null,",
    "avatar": {
      "links": {
        "update": {
          "href": "/api/users/{user_id}/avatar"
        },
        "delete": {
          "href": "/api/users/{user_id}/avatar"
        }
      },
      "image": {
        "original": "http://example.com/images/image.png",
        "large": "http://example.com/images/large_image.png",
        "medium": "http://example.com/images/medium_image.png",
        "small": "http://example.com/images/small_image.png"
      }
    },
    "terms_accepted": true,
    "subscribed_for_updates": true,
    "intent_url": "invitations/{token}",
    "intent_url_updated_at": 0,
    "created_at": 0,
    "updated_at": 0
  }
}

```

*Current User model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|current_user|[CurrentUser](#schemacurrentuser)|false|none|A Current user is the entity currently logged in and interacting with the API. Most of the times the user logged in, is the user interacting with the site. But some anonymous endpoints, like logging in, logging out and so forth, do have a current user, but an anonymous user. ## Anonymous Anonymous current_user is a normal current_user object, but with all attributes null and defaulting.|

<h2 id="tocScurrentuser">CurrentUser</h2>

<a id="schemacurrentuser"></a>

```json
{
  "id": "3f626fca-e449-4d27-9e2f-8c0d10405b94",
  "email": "daan@example.com",
  "unconfirmed_email": "otherdaan@example.com",
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "state": "new",
  "locale": "null,",
  "avatar": {
    "links": {
      "update": {
        "href": "/api/users/{user_id}/avatar"
      },
      "delete": {
        "href": "/api/users/{user_id}/avatar"
      }
    },
    "image": {
      "original": "http://example.com/images/image.png",
      "large": "http://example.com/images/large_image.png",
      "medium": "http://example.com/images/medium_image.png",
      "small": "http://example.com/images/small_image.png"
    }
  },
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}",
  "intent_url_updated_at": 0,
  "created_at": 0,
  "updated_at": 0
}

```

*A Current user is the entity currently logged in and interacting with
the API. Most of the times the user logged in, is the user interacting
with the site. But some anonymous endpoints, like logging in,
logging out and so forth, do have a current user, but an anonymous
user.
## Anonymous
Anonymous current_user is a normal current_user object, but with
all attributes null and defaulting.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|email|string|false|none|Emailaddress the user registered with.After registration, a confirmation mail is sent too.|
|unconfirmed_email|string|false|none|Changing the emailaddress means a confirmation mail is sent out to that emailaddress. Once that is confirmed (by following the confirmation-URL in that email) this is the new emailaddress, any old emailaddress is now lost. Untill the moment the email is confirmed, it appears in the current user object as unconfirmed_email. When a user with a pending email confirmation changes the email (again), the old unconfirmed email becomes invalid. Following the confirmation-link sent to that emailaddress has no effect. A new confirmation email is sent to the new emailaddress.|
|first_name|string|false|none|none|
|last_name|string|false|none|none|
|iban|string|false|none|REMOVED in API v4 see financial accounts|
|account_holder|string|false|none|REMOVED in API v4 see financial accounts|
|city|string|false|none|none|
|state|string|false|none|The current state of the account.  * new: account is not yet persisted. Should never occur when   interacting via the API. * unconfirmed: account is persisted, but not confirmed by the user. * confirmed: account is persisted and confirmed by the user.  NOTE: Using such enum or flag fields in business logic in the client is discouraged. All information regarding business logic should be communicated through meta-fields and link fields instead by the backend. When some crucial information is missing, that should be considered a missing feature in the backend. A field such as this should best only be used for display purposes.|
|locale|string|false|none|The language that the user wants to communicate in. This language will be the locale for all interaction, unless the client provides an Accept-Language header to override this.  Some events are not triggered by a client, or are triggered by a client that is irrelevant for the user. E.g. a user with a client, set in English, can trigger a Settle event, which will email all members. The emails going to the members should be in their preferred language, and not in the language of the Client.  Possible values can be: * null: we set a default on registering, but there is no guarantee         that all users have a language at all time. `null` indicates         that this user has no preference. * nl: Dutch * en: English (British!)  Note: eventhough the value may be null, and clients should be able to handle this, in practice it will hardly ever happen, with the current flows.|
|avatar|[Avatar](#schemaavatar)|false|none|none|
|terms_accepted|boolean|false|none|The indicator saying if user has accepted new terms of service (GDPR). It may be true or false. Null value means an existing user hasn't decide about the terms yet.|
|subscribed_for_updates|boolean|false|none|The indicator saying if user has accepted subscription for email updates. It may be true, false or null. Null value means it's unknown if user accepted or decliend subscription.|
|intent_url|string|false|none|Url indicating that the user should be redirected to a specific part of application (i.e. list invitation)|
|intent_url_updated_at|integer|false|none|Indicates when intnet_url has been set, so that client is able to decide if the redirection is still valid|
|created_at|integer|false|none|none|
|updated_at|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|state|new|
|state|unconfirmed|
|state|confirmed|
|locale|null,|
|locale|en,|
|locale|nl|

<h2 id="tocSuserasregistrantcontainer">UserAsRegistrantContainer</h2>

<a id="schemauserasregistrantcontainer"></a>

```json
{
  "user": {
    "email": "daan@example.com",
    "password": "d81dfccc5cccde34fb44a37c",
    "first_name": "Daan",
    "last_name": "de Jong",
    "terms_accepted": true,
    "subscribed_for_updates": true,
    "intent_url": "invitations/{token}"
  }
}

```

*User Container for user to be registered*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|user|[UserAsRegistrant](#schemauserasregistrant)|false|none|User to be registered|

<h2 id="tocSuserasregistrant">UserAsRegistrant</h2>

<a id="schemauserasregistrant"></a>

```json
{
  "email": "daan@example.com",
  "password": "d81dfccc5cccde34fb44a37c",
  "first_name": "Daan",
  "last_name": "de Jong",
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}"
}

```

*User to be registered*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|email|string|true|none|none|
|password|string|true|none|none|
|first_name|string|true|none|none|
|last_name|string|true|none|none|
|terms_accepted|boolean|true|none|It represents user decision about new terms acceptance (GDPR)|
|subscribed_for_updates|boolean|false|none|It represents user decision about subscription for email updates|
|intent_url|string|false|none|Url indicating that the user should be redirected to a specific part of application (i.e. list invitation)|

<h2 id="tocSuserassignin">UserAsSignIn</h2>

<a id="schemauserassignin"></a>

```json
{
  "email": "daan@example.com",
  "password": "5755f8cd04bb1369c7c09126"
}

```

*Details to sign in a user*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|email|string|true|none|none|
|password|string|true|none|none|

<h2 id="tocSuserasprofilecontainer">UserAsProfileContainer</h2>

<a id="schemauserasprofilecontainer"></a>

```json
{
  "user": {
    "first_name": "Daan",
    "last_name": "de Jong",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "[DEPRECATED in API v4 see financial accounts] D.R.W. De Jong en Zonen B.V.",
    "city": "Rotterdam",
    "locale": "nl",
    "terms_accepted": true,
    "subscribed_for_updates": true
  }
}

```

*User Profile Container.
## Profile
A Profile is a representation of the user details that can be changed
at any time. This data is simple metadata such as the names, iban,
city etc.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|user|[UserAsProfile](#schemauserasprofile)|false|none|User to be registered|

<h2 id="tocSuserasprofile">UserAsProfile</h2>

<a id="schemauserasprofile"></a>

```json
{
  "first_name": "Daan",
  "last_name": "de Jong",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "[DEPRECATED in API v4 see financial accounts] D.R.W. De Jong en Zonen B.V.",
  "city": "Rotterdam",
  "locale": "nl",
  "terms_accepted": true,
  "subscribed_for_updates": true
}

```

*User to be registered*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|first_name|string|false|none|none|
|last_name|string|false|none|none|
|iban|string|false|none|A [valid IBAN](https://en.wikipedia.org/wiki/International_Bank_Account_Number#Validating_the_IBAN)  DEPRECATED in API v4 see financial accounts|
|account_holder|string|false|none|none|
|city|string|false|none|none|
|locale|string|false|none|A simple locale string representing the locale for this user.|
|terms_accepted|boolean|false|none|It represents user decision about new terms acceptance (GDPR)|
|subscribed_for_updates|boolean|false|none|It represents user decision about subscription for email updates|

#### Enumerated Values

|Property|Value|
|---|---|
|locale|en|
|locale|nl|

<h2 id="tocSuserasaccountcontainer">UserAsAccountContainer</h2>

<a id="schemauserasaccountcontainer"></a>

```json
{
  "user": {
    "old_password": "8e0b4eb84e6472f4830bc85d",
    "password": "c320011fc8375f46f1dac8ae",
    "email": "otherdaan@example.com"
  }
}

```

*User Account Container.
## Account
An Account is a representation of the user details that can be changed
at any time. This data is simple metadata such as the names, iban,
city etc.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|user|[UserAsAccount](#schemauserasaccount)|false|none|User to be registered|

<h2 id="tocSuserasaccount">UserAsAccount</h2>

<a id="schemauserasaccount"></a>

```json
{
  "old_password": "8e0b4eb84e6472f4830bc85d",
  "password": "c320011fc8375f46f1dac8ae",
  "email": "otherdaan@example.com"
}

```

*User to be registered*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|old_password|string|false|none|none|
|password|string|false|none|none|
|email|string|false|none|none|

<h2 id="tocSuserasinviteecontainer">UserAsInviteeContainer</h2>

<a id="schemauserasinviteecontainer"></a>

```json
{
  "user": {
    "email": "daan@example.com",
    "password": "87bd29c584ae5223aa8a96b4",
    "first_name": "Daan",
    "last_name": "de Jong",
    "invitation_token": "20f936a0",
    "terms_accepted": true,
    "subscribed_for_updates": true,
    "intent_url": "invitations/{token}"
  }
}

```

*User Container for user to be registered as invitee*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|user|[UserAsInvitee](#schemauserasinvitee)|false|none|User to be registered|

<h2 id="tocSuserasinvitee">UserAsInvitee</h2>

<a id="schemauserasinvitee"></a>

```json
{
  "email": "daan@example.com",
  "password": "87bd29c584ae5223aa8a96b4",
  "first_name": "Daan",
  "last_name": "de Jong",
  "invitation_token": "20f936a0",
  "terms_accepted": true,
  "subscribed_for_updates": true,
  "intent_url": "invitations/{token}"
}

```

*User to be registered*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|email|string|true|none|none|
|password|string|true|none|none|
|first_name|string|true|none|none|
|last_name|string|true|none|none|
|invitation_token|string|true|none|The secret token as emailed to the user when               inviting that user to become a member of the list.               Has to be an invitation token that is not used yet.|
|terms_accepted|boolean|true|none|It represents user decision about new terms acceptance (GDPR)|
|subscribed_for_updates|boolean|false|none|It represents user decision about subscription for email updates|
|intent_url|string|false|none|Url indicating that the user should be redirected to a specific part of application (i.e. list invitation)|

<h2 id="tocSuserintentcontainer">UserIntentContainer</h2>

<a id="schemauserintentcontainer"></a>

```json
{
  "user": {
    "intent_url": "http://example.com"
  }
}

```

*User Intent Container.
## Intent
User intent is a representation of the url
user should be redirected to after registration
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|user|[UserIntent](#schemauserintent)|false|none|User intent to be updated. It may be also null.|

<h2 id="tocSuserintent">UserIntent</h2>

<a id="schemauserintent"></a>

```json
{
  "intent_url": "http://example.com"
}

```

*User intent to be updated. It may be also null.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|intent_url|string|false|none|none|

<h2 id="tocSsettlementpreview">SettlementPreview</h2>

<a id="schemasettlementpreview"></a>

```json
{
  "links": {
    "current": "/api/list/01dbc53a-6447-48eb-a086-8b7bb0b34fd4/settlement_preview"
  },
  "settlement_preview": {
    "created_at": 0,
    "updated_at": 0,
    "list_id": "01dbc53a-6447-48eb-a086-8b7bb0b34fd4",
    "expenses": {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/expenses"
        },
        "create": {
          "href": "/api/lists/{list_id}/expenses"
        }
      }
    },
    "list_commitments": [
      {
        "list_commitment": {
          "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
          "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
          "amount": {
            "fractional": "10.00",
            "currency": "EUR"
          }
        }
      }
    ],
    "members": {
      "links": {
        "current": {
          "href": "/api/lists/{list_id}/members"
        },
        "create": {
          "href": "/api/lists/{list_id}/members"
        }
      }
    },
    "reports": {
      "pdf": {
        "en": "http://example.com/settlements/en/ts/ents.pdf",
        "nl": "http://example.com/settlements/xy/ts/xyts.pdf"
      }
    }
  }
}

```

*SettlementPreview model*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|object|false|none|none|
|» current|any|false|none|Link to this SettlementPreview|
|settlement_preview|object|false|none|none|
|» created_at|integer|false|none|none|
|» updated_at|integer|false|none|none|
|» list_id|string|false|none|List ID this settlement preview represents.|
|» expenses|[LeanExpensesCollection](#schemaleanexpensescollection)|false|none|A pointer to a Collection of expenses for the list|
|» list_commitments|[[ListCommitmentContainer](#schemalistcommitmentcontainer)]|false|none|List of list_commitments.|
|» members|[LeanMemberCollection](#schemaleanmembercollection)|false|none|A pointer to a Collection of Members|
|» reports|[Reports](#schemareports)|false|none|none|

<h2 id="tocSlistcommitmentcontainer">ListCommitmentContainer</h2>

<a id="schemalistcommitmentcontainer"></a>

```json
{
  "list_commitment": {
    "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
    "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    }
  }
}

```

*ListCommitment model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|list_commitment|[ListCommitment](#schemalistcommitment)|false|none|none|

<h2 id="tocSlistcommitment">ListCommitment</h2>

<a id="schemalistcommitment"></a>

```json
{
  "payer_id": "fleurd7e-035c-4f67-9f2f-2719b16e5094",
  "beneficiary_id": "daan1d7e-035c-4f67-9f2f-2719b16e5094",
  "amount": {
    "fractional": "10.00",
    "currency": "EUR"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|payer_id|string|false|none|The ID of the **Member** who has to pay               this amount to beneficiary|
|beneficiary_id|string|false|none|The ID of the **Member** who will receive               this amount from the payer|
|amount|[Amount](#schemaamount)|false|none|none|

<h2 id="tocSfinancialaccountcollection">FinancialAccountCollection</h2>

<a id="schemafinancialaccountcollection"></a>

```json
{
  "links": {
    "first": {
      "href": "/api/link/to/current?page=1"
    },
    "last": {
      "href": "/api/link/to/current?&page=20"
    },
    "next": {
      "href": "/api/link/to/current?&page=3"
    },
    "previous": {
      "href": "/api/link/to/current?&page=1"
    },
    "current": {
      "href": "/api/current?sort[name]=desc&sort[created_at]=asc&page=2",
      "meta": {
        "total_pages": 20,
        "current_page": 3,
        "offset": 60,
        "per_page": 30,
        "total_entries": 588,
        "sorted_by_fields": {
          "name": "desc",
          "created_at": "asc"
        },
        "sorted_by_query": "sort[name]=desc&sort[created_at]=asc",
        "sortable_fields": [
          "name",
          "created_at",
          "modified_at"
        ]
      }
    }
  },
  "data": [
    {
      "financial_account": {
        "id": "5b3f19e5-b875-4550-8723-192799dd1091",
        "publish_details": true,
        "payment_method": "ManualIBAN",
        "iban": "NL39 RABO 0300 0652 64",
        "account_holder": "D.R.W. De Jong en Zonen B.V.",
        "created_at": 1554119930,
        "updated_at": 1554119930,
        "state": "pending",
        "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
        "oauth_intent": "splitser://example/status",
        "oauth_platform": "android",
        "external_account_id": "test@paypal.com"
      }
    }
  ]
}

```

*A Collection of Financial accounts.
        See [Synching](https://bitbucket.org/ttyams/wbw-ios/wiki/Synching) for
        more information on Paging and Collections.*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[CollectionLinks](#schemacollectionlinks)|false|none|Contains paths to actions that can be taken on this           collection. When a path is missing, or nil/null, then you cannot           take that action. E.g. when the *next* path is empty, there is no           next page. E.g. when the *create* path is emtpy in an Expense           Collection, you are not allowed to create a new expense.|
|data|[[FinancialAccountContainer](#schemafinancialaccountcontainer)]|false|none|[FinancialAccount model container]|

<h2 id="tocSfinancialaccountcontainer">FinancialAccountContainer</h2>

<a id="schemafinancialaccountcontainer"></a>

```json
{
  "financial_account": {
    "id": "5b3f19e5-b875-4550-8723-192799dd1091",
    "publish_details": true,
    "payment_method": "ManualIBAN",
    "iban": "NL39 RABO 0300 0652 64",
    "account_holder": "D.R.W. De Jong en Zonen B.V.",
    "created_at": 1554119930,
    "updated_at": 1554119930,
    "state": "pending",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "external_account_id": "test@paypal.com"
  }
}

```

*FinancialAccount model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|financial_account|[FinancialAccount](#schemafinancialaccount)|false|none|A financial account is a financial account of a user. If payment_method is "Paypal" then `oauth_intent` and `oauth_platform` are required.|

<h2 id="tocSfinancialaccountoauthdatacontainer">FinancialAccountOauthDataContainer</h2>

<a id="schemafinancialaccountoauthdatacontainer"></a>

```json
{
  "financial_account": {
    "id": "e14fc85c-7d29-46a2-a800-c93cca199f36",
    "oauth_intent": "splitser://example/status",
    "oauth_platform": "android",
    "state": "pending"
  }
}

```

*FinancialAccount oauth data model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|financial_account|[FinancialAccountOauthData](#schemafinancialaccountoauthdata)|false|none|A financial account oauth data. It refers only to a paypal account.|

<h2 id="tocSfinancialaccountoauthurlcontainer">FinancialAccountOauthUrlContainer</h2>

<a id="schemafinancialaccountoauthurlcontainer"></a>

```json
{
  "financial_account": {
    "id": "fd7f20bb-bcff-4bf5-899b-91a9c0996cdb",
    "oauth_url": "https://www.sandbox.paypal.com/signin/authorize"
  }
}

```

*FinancialAccount oauth url model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|financial_account|[FinancialAccountOauthUrl](#schemafinancialaccountoauthurl)|false|none|A financial account oauth url. It refers only to a paypal account.|

<h2 id="tocSfinancialaccount">FinancialAccount</h2>

<a id="schemafinancialaccount"></a>

```json
{
  "id": "5b3f19e5-b875-4550-8723-192799dd1091",
  "publish_details": true,
  "payment_method": "ManualIBAN",
  "iban": "NL39 RABO 0300 0652 64",
  "account_holder": "D.R.W. De Jong en Zonen B.V.",
  "created_at": 1554119930,
  "updated_at": 1554119930,
  "state": "pending",
  "oauth_url": "https://www.sandbox.paypal.com/signin/authorize",
  "oauth_intent": "splitser://example/status",
  "oauth_platform": "android",
  "external_account_id": "test@paypal.com"
}

```

*A financial account is a financial account of a user.
If payment_method is "Paypal" then `oauth_intent` and `oauth_platform` are required.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|publish_details|boolean|true|none|Determines whether or not the user of the financial account publishes the details of this financial account. If the user does not publish the details, IBAN and account holder will be null for all other users.|
|payment_method|string|true|none|The payment method of the financial account. You can pass "ManualIBAN" or "Paypal". In case of ManualIBAN you need to provide "iban" as well. In case of "Paypal" no other params are required. Account holder is optional.|
|iban|string|true|none|An IBAN. Required if type is ManualIBAN|
|account_holder|string|false|none|The account holder of the corresponding IBAN|
|created_at|integer|false|read-only|none|
|updated_at|integer|false|read-only|none|
|state|string|false|read-only|It applies only to external accounts (only Paypal as for now). It gives the information if the account has been verified and is ready to be used. If "inactive", no operation is allowed.|
|oauth_url|string|false|read-only|Redirect to this url to start the LIPP process.|
|oauth_intent|string|true|none|Internal url to redirect to after the LIPP process has been finished.|
|oauth_platform|string|true|none|Indication on which platform the LIPP process has been started.|
|external_account_id|string|false|read-only|User's identifier used in the external payment service (i.e. paypal email)|

#### Enumerated Values

|Property|Value|
|---|---|
|payment_method|ManualIBAN|
|payment_method|Paypal|
|state|pending|
|state|active|
|state|inactive|
|oauth_platform|android|
|oauth_platform|ios|
|oauth_platform|web|

<h2 id="tocSfinancialaccountoauthdata">FinancialAccountOauthData</h2>

<a id="schemafinancialaccountoauthdata"></a>

```json
{
  "id": "e14fc85c-7d29-46a2-a800-c93cca199f36",
  "oauth_intent": "splitser://example/status",
  "oauth_platform": "android",
  "state": "pending"
}

```

*A financial account oauth data. It refers only to a paypal account.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|oauth_intent|string|true|none|Internal url to redirect to after the LIPP process has been finished.|
|oauth_platform|string|true|none|Indication on which platform the LIPP process has been started.|
|state|string|true|read-only|It applies only to external accounts (only Paypal as for now). It gives the information if the account has been verified and is ready to be used. If "inactive", no operation is allowed.|

#### Enumerated Values

|Property|Value|
|---|---|
|oauth_platform|android|
|oauth_platform|ios|
|oauth_platform|web|
|state|pending|
|state|active|
|state|inactive|

<h2 id="tocSfinancialaccountoauthurl">FinancialAccountOauthUrl</h2>

<a id="schemafinancialaccountoauthurl"></a>

```json
{
  "id": "fd7f20bb-bcff-4bf5-899b-91a9c0996cdb",
  "oauth_url": "https://www.sandbox.paypal.com/signin/authorize"
}

```

*A financial account oauth url. It refers only to a paypal account.
*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|oauth_url|string|true|read-only|Redirect to this url to start the LIPP process.|

<h2 id="tocSfundingoptioncontainer">FundingOptionContainer</h2>

<a id="schemafundingoptioncontainer"></a>

```json
{
  "funding_option": {
    "id": "OPTION_ID_4"
  }
}

```

*single funding option container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|funding_option|[FundingOption](#schemafundingoption)|false|none|dynamic object with different attributes per option|

<h2 id="tocSfundingoptioncreator">FundingOptionCreator</h2>

<a id="schemafundingoptioncreator"></a>

```json
{
  "funding_option": {
    "amount": {
      "fractional": "1000",
      "currency": "EUR"
    }
  }
}

```

*Create model for funding options*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|funding_option|object|false|none|none|
|» amount|object|true|none|none|
|»» fractional|number|true|none|none|
|»» currency|string|true|none|none|

<h2 id="tocSfundingoption">FundingOption</h2>

<a id="schemafundingoption"></a>

```json
{
  "id": "OPTION_ID_4"
}

```

*dynamic object with different attributes per option*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Funding Option ID. Not a UUID.|

<h2 id="tocSfundingoptioncollection">FundingOptionCollection</h2>

<a id="schemafundingoptioncollection"></a>

```json
{
  "links": null,
  "data": [
    {
      "funding_option": {
        "id": "OPTION_ID_4"
      }
    }
  ]
}

```

*A set of fundingoptions. Is not paged. Cannot be
        (re)fetched: only returned on create*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|any|false|none|Empty links container. For consistency.|
|data|[[FundingOptionContainer](#schemafundingoptioncontainer)]|false|none|[single funding option container]|

<h2 id="tocSpaymentrequestcontainer">PaymentRequestContainer</h2>

<a id="schemapaymentrequestcontainer"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/payment_requests/"
    }
  },
  "payment_request": {
    "id": null,
    "description": "My list 1",
    "payable_id": "3ddf4f23-f6c8-4c5c-b52e-15ad5c33e143",
    "payable_type": "Commitment",
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "payer": {
      "links": {
        "current": {
          "href": "/api/user/{id}"
        },
        "user": {
          "id": "b87b2083-8b9d-4c23-86e4-c734ea4d6ce3"
        }
      }
    },
    "beneficiary": {
      "links": {
        "current": {
          "href": "/api/user/{id}"
        },
        "user": {
          "id": "38afa928-cc67-4396-accf-f903b53039de"
        }
      }
    },
    "token": null,
    "url": "links.wiebetaaltwat.nl/payment_requests/",
    "state": "pending",
    "created_at": 1554119930,
    "updated_at": 1554119930
  }
}

```

*Payment Request model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[PaymentRequestLinks](#schemapaymentrequestlinks)|false|none|none|
|payment_request|[PaymentRequest](#schemapaymentrequest)|false|none|none|

<h2 id="tocSpaymentrequestlinks">PaymentRequestLinks</h2>

<a id="schemapaymentrequestlinks"></a>

```json
{
  "current": {
    "href": "/api/payment_requests/"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|current|[CurrentPaymentRequestLink](#schemacurrentpaymentrequestlink)|true|none|none|

<h2 id="tocScurrentpaymentrequestlink">CurrentPaymentRequestLink</h2>

<a id="schemacurrentpaymentrequestlink"></a>

```json
{
  "href": "/api/payment_requests/"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|href|string|true|none|none|

<h2 id="tocSpaymentrequest">PaymentRequest</h2>

<a id="schemapaymentrequest"></a>

```json
{
  "id": null,
  "description": "My list 1",
  "payable_id": "3ddf4f23-f6c8-4c5c-b52e-15ad5c33e143",
  "payable_type": "Commitment",
  "amount": {
    "fractional": "10.00",
    "currency": "EUR"
  },
  "payer": {
    "links": {
      "current": {
        "href": "/api/user/{id}"
      },
      "user": {
        "id": "b87b2083-8b9d-4c23-86e4-c734ea4d6ce3"
      }
    }
  },
  "beneficiary": {
    "links": {
      "current": {
        "href": "/api/user/{id}"
      },
      "user": {
        "id": "38afa928-cc67-4396-accf-f903b53039de"
      }
    }
  },
  "token": null,
  "url": "links.wiebetaaltwat.nl/payment_requests/",
  "state": "pending",
  "created_at": 1554119930,
  "updated_at": 1554119930
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|Payment request's ID|
|description|string|true|none|Payment request's description|
|payable_id|string|true|none|Payable object's ID|
|payable_type|string|true|none|none|
|amount|[Amount](#schemaamount)|true|none|none|
|payer|object|false|none|none|
|» links|object|false|none|none|
|»» current|object|false|none|none|
|»»» href|string|false|none|Links to the user that is to pay the                                    PaymentRequest|
|»» user|object|false|none|none|
|»»» id|string|false|none|Id of the user that is to pay the                                   PaymentRequest|
|»» beneficiary|object|false|none|none|
|»»» links|object|false|none|none|
|»»»» current|object|false|none|none|
|»»»»» href|string|false|none|Links to the user that is to receive the                                   payment for this PaymentRequest|
|»»»» user|object|false|none|none|
|»»»»» id|string|false|none|ID of the user that is to receive the                                   payment for this PaymentRequest|
|»»»» token|string|false|none|Unique token by which to fetch and find this                             PaymentRequest|
|»»»» url|string|true|none|Public URL for a user to pay his part of the commitment|
|»»»» state|string|true|none|It gives the information if the payment has been processed correctly or not.|
|»»»» created_at|integer|true|read-only|none|
|»»»» updated_at|integer|true|read-only|none|

#### Enumerated Values

|Property|Value|
|---|---|
|state|pending|
|state|fulfilled|

<h2 id="tocSpaymentcontainer">PaymentContainer</h2>

<a id="schemapaymentcontainer"></a>

```json
{
  "links": {
    "current": {
      "href": "/api/payments/9246e1a8-ed90-4b38-adc8-a538b5371d25"
    }
  },
  "payment": {
    "id": "9246e1a8-ed90-4b38-adc8-a538b5371d25",
    "description": "My list 1",
    "payable_id": "bb9ddd77-cc37-4638-a746-b49d45b8aac5",
    "payable_type": "Settle::Commitment",
    "original_amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "amount": {
      "fractional": "10.00",
      "currency": "EUR"
    },
    "exchange_rate": "0.124",
    "payer_id": "ee2e2a34-3f6b-4b3f-a42d-2896a0a67497",
    "beneficiary_id": "00e3a571-d384-4a44-84c0-0d67173c9be7",
    "funding_option_id": "45f25889ff02c3e5dbf27d575dbeaeea0df487ca9ffdb46e7fe4af0f457980e5",
    "payment_token": "01951436",
    "created_at": 1554119930,
    "updated_at": 1554119930
  }
}

```

*Payment model container*

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|links|[PaymentLinks](#schemapaymentlinks)|false|none|none|
|payment|[Payment](#schemapayment)|false|none|none|

<h2 id="tocSpaymentlinks">PaymentLinks</h2>

<a id="schemapaymentlinks"></a>

```json
{
  "current": {
    "href": "/api/payments/9246e1a8-ed90-4b38-adc8-a538b5371d25"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|current|[CurrentPaymentLink](#schemacurrentpaymentlink)|true|none|none|

<h2 id="tocScurrentpaymentlink">CurrentPaymentLink</h2>

<a id="schemacurrentpaymentlink"></a>

```json
{
  "href": "/api/payments/9246e1a8-ed90-4b38-adc8-a538b5371d25"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|href|string|true|none|none|

<h2 id="tocSpayment">Payment</h2>

<a id="schemapayment"></a>

```json
{
  "id": "9246e1a8-ed90-4b38-adc8-a538b5371d25",
  "description": "My list 1",
  "payable_id": "bb9ddd77-cc37-4638-a746-b49d45b8aac5",
  "payable_type": "Settle::Commitment",
  "original_amount": {
    "fractional": "10.00",
    "currency": "EUR"
  },
  "amount": {
    "fractional": "10.00",
    "currency": "EUR"
  },
  "exchange_rate": "0.124",
  "payer_id": "ee2e2a34-3f6b-4b3f-a42d-2896a0a67497",
  "beneficiary_id": "00e3a571-d384-4a44-84c0-0d67173c9be7",
  "funding_option_id": "45f25889ff02c3e5dbf27d575dbeaeea0df487ca9ffdb46e7fe4af0f457980e5",
  "payment_token": "01951436",
  "created_at": 1554119930,
  "updated_at": 1554119930
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|Payment's ID|
|description|string|true|none|Payment's description|
|payable_id|string|true|none|Payable object's ID|
|payable_type|string|true|none|none|
|original_amount|[Amount](#schemaamount)|false|none|none|
|amount|[Amount](#schemaamount)|true|none|none|
|exchange_rate|string|false|none|Exchange rate between original_amount and amount|
|payer_id|string|true|none|Payer's ID|
|beneficiary_id|string|true|none|Beneficiary's ID|
|funding_option_id|string|false|none|Funding Option ID as chosen by the user|
|payment_token|string|false|none|Visible identifier, unique for this payment.|
|created_at|integer|true|read-only|none|
|updated_at|integer|true|read-only|none|

