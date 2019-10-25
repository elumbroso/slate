---
title: AT Internet - API v3 documentation

language_tabs:
  - javascript
  - shell
  - python
  - ruby

footer:
  - <a href='https://user-profile.atinternet-solutions.com/#/apikeys' target="_blank">Create my API key</a>

includes:
  - errors

search: false

attachments:
  - "./logo.png"
---

# Introduction

<aside class="warning">
This API is always in beta, please don't use it in production.
</aside>

This example API documentation page was created with [Greenboard](https://github.com/shamin/greenboard). Feel free to edit it and use it as a base for your own API's documentation.

# Basic example

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.atinternet.io/data/getData")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["x-api-key"] = 'YOURAPIKEY'
request.body = "{\n \"columns\": [\n   \"visit_device_type\",\n   \"m_visits\",\n   \"m_users\"\n ],\n \"sort\": [\n   \"-m_visits\"\n ],\n\"space\": {\n   \"s\": [\n     429023\n   ]\n },\n \"period\": {\n   \"p1\": [\n     {\n       \"type\": \"D\",\n       \"start\": \"2019-10-24\",\n       \"end\": \"2019-10-24\"\n     }\n   ]\n },\n \"max-results\": 50,\n \"page-num\": 1\n}"

response = http.request(request)
puts response.read_body
```

```python
import http.client

conn = http.client.HTTPConnection("api,atinternet,io")
payload = "{\n \"columns\": [\n   \"visit_device_type\",\n   \"m_visits\",\n   \"m_users\"\n ],\n \"sort\": [\n   \"-m_visits\"\n ],\n\"space\": {\n   \"s\": [\n     429023\n   ]\n },\n \"period\": {\n   \"p1\": [\n     {\n       \"type\": \"D\",\n       \"start\": \"2019-10-24\",\n       \"end\": \"2019-10-24\"\n     }\n   ]\n },\n \"max-results\": 50,\n \"page-num\": 1\n}"

headers = {
    'x-api-key': "YOURAPIKEY",
}

conn.request("POST", "data,getData", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```shell
curl -X POST \
  https://api.atinternet.io/data/getData \
  -H 'x-api-key: YOURAPIKEY' \
  -H 'Content-Type: application/json' \
  -d '{
 "columns": [
   "visit_device_type",
   "m_visits",
   "m_users"
 ],
 "sort": [
   "-m_visits"
 ],
 "space": {
   "s": [
     429023
   ]
 },
 "period": {
   "p1": [
     {
       "type": "D",
       "start": "2019-10-24",
       "end": "2019-10-24"
     }
   ]
 },
 "max-results": 50,
 "page-num": 1
}'
```

```javascript
var data = JSON.stringify({
  "columns": [
    "visit_device_type",
    "m_visits",
    "m_users"
  ],
  "sort": [
    "-m_visits"
  ],
  "space": {
    "s": [
     429023
   ]
  },
  "period": {
    "p1": [
      {
        "type": "D",
        "start": "2019-10-24",
        "end": "2019-10-24"
      }
    ]
  },
  "max-results": 50,
  "page-num": 1
});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://api.atinternet.io/data/getData");
xhr.setRequestHeader("x-api-key", "YOURAPIKEY");

xhr.send(data);
```

The v3 API doesn't accept any `GET` request anymore, you have to use a `POST` request with a json body.

| Parameter                          | Required | Description                                                                |
| ---------------------------------- | -------- | -------------------------------------------------------------------------- |
| [space](#sites--level-2-sites)     | true     | Analysis space (site, level2, multi-sites).                                |
| [columns](#properties-and-metrics) | true     | List of properties/metrics to retrieve (separated by a comma in the call). |
| [period](#period)                  | true     | Analysis period (single, multiple, relative).                              |
| [filter](#filter)                  | false    | Filters to be applied on properties/metrics.                               |
| [evo](#evolution)                  | false    | To obtain the evolution of a group of metrics  over a certain time period. |
| [sort](#sort)                      | true     | List of properties/ metrics according to which the results will be sorted. |
| max-results                        | true     | Number of results in the results page.                                     |
| page-num                           | true     | Page number of the result set.                                             |


# Authentication

> To authorize, use this code:

```ruby
request["x-api-key"] = 'YOURAPIKEY'
request.body = "{\n \"columns\": [\n   \"visit_device_type\",\n   \"m_visits\",\n   \"m_users\"\n ],\n \"sort\": [\n   \"-m_visits\"\n ],\n\"space\": {\n   \"s\": [\n     429023\n   ]\n },\n \"period\": {\n   \"p1\": [\n     {\n       \"type\": \"D\",\n       \"start\": \"2019-10-24\",\n       \"end\": \"2019-10-24\"\n     }\n   ]\n },\n \"max-results\": 50,\n \"page-num\": 1\n}"

response = http.request(request)
puts response.read_body
```

```python
headers = {
    'x-api-key': "YOURAPIKEY",
}
```

```shell
curl "https://api.atinternet.io/data/v3"
  -H "x-api-key: YOURAPIKEY"
```

```javascript
xhr.setRequestHeader("x-api-key", "YOURAPIKEY");
```

AT Internet uses API keys to allow access to the API. You can register a new API key on your [profil page](https://user-profile.atinternet-solutions.com/#/apikeys).

The access rights Advanced analyst is mandatory to use the API.

API key has to be included in all API requests to the server in a header that looks like the following:

`x-api-key: YOURAPIKEY`

<aside class="notice">
You must replace <code>YOURAPIKEY</code> with your personal API key.
</aside>


# Sites / Level 2 sites

> Retrieve 1 site :

```json
"space": {
    "s":[547656]
},
```

> Retrieve multiple sites :

```json
"space": {
    "s":[547656, 547657]
},
```

> Retrieve one level 2 site :

```json
 "space": {
   "l2s": {
     "s": 547656,
     "l2name": "Data Query 3"
   }
 },
```

The `space` parameter allows you to get data relative to your site.


# Properties and metrics

> Body with one period :

```json
"columns": [ 
  "d_device_type", 
  "m_visits" 
] 
```

> Body with multiple periods :

```json
"columns": [  
  "d_device_type",
  "p1.m_visits",
  "p2.m_visits"
] 
```

In the `columns` parameter you can enter all properties and metrics that might be available in the call. To obtain the properties and metrics labels, you can go to your [Data Model](https://management.atinternet-solutions.com/#/data-model/properties) and look for the poperty key, or go to [Data Query 3](https://dataquery.atinternet-solutions.com/) and hover the (i) next to name :

![Image of Yaktocat](https://helpcentre.atinternet-solutions.com/hc/article_attachments/360005494580/preview-menu.gif)



# Period

`p1` object is mandatory, it is the main period of analysis.
`p2` object is optional, it describes the comparison period.

| Parameter   | Description | Possible values                           |
| ----------- | ----------- | ----------------------------------------- |
| type        |             | R(elative), D (absolute)                  |


## Absolute periods

```json
"period": {
  "p1": [
    {
      "type": "D",
      "start": "2019-10-20",
      "end": "2019-10-24"
    }
  ],
  "p2": [
    {
      "type": "D",
      "start": "2019-10-15",
      "end": "2019-10-19"
    }
  ]
}
```

Date format : yyyy-mm-dd

## Relative periods

```json
"period": {
    "p1": [
      {
        "type": "R",
        "granularity": "D",
        "offSet": "-1"
      }
    ],
    "p2": [
      {
        "type": "R",
        "granularity": "D",
        "offSet": "-2"
      }
    ]
  }
```


| Parameter   | Description | Possible values                           |
| ----------- | ----------- | ----------------------------------------- |
| granularity | -           | Y(ear), Q(uarter), M(onth), W(eek), D(ay) |
| offSet      | -           | Number                                    |


# Filter

> Example : (visit **equal to** 19) *AND* (device type **equal to** Tablet *or* Desktop) *AND* (source **start** by Referrer )

```json
"filter": {
  "metric": {
    "$AND":[
      {
        "m_visits": {
          "$eq": 19
        }
      }
    ]
  },
  "property": {
    "$AND": [
      {
        "$OR": [{
          "visit_device_type": {
            "$eq": "Tablet"
          }
        }, {
          "visit_device_type": {
            "$eq": "Desktop"
          }
        }
        ]
      }, 
      {
        "visit_src": {
          "$start": "Referrer"
        }
      }

    ]
  }
}

```

If you want to filter your dataset, you have to use the `filter` object. You can filter on properties or metrics.

### Filter on number

| Parameter  | Description                                      |
| ---------- | ------------------------------------------------ |
| $eq        | Equals to                                        |
| $neq       | Does not equal                                   |
| $in        | Array                                            |
| $gt        | Greater than                                     |
| $gte       | Greater or equal to                              |
| $lt        | Lower than                                       |
| $lte       | Lower or equal to                                |
| $na        | true or false - NULL value                       |
| $undefined | true or false - undefined                        |
| $empty     | true or false - combinaison de $na et $undefined |

### Filter on string

| Parameter  | Description                                      |
| ---------- | ------------------------------------------------ |
| $eq        | Equals to                                        |
| $neq       | Does not equal                                   |
| $in        | Array                                            |
| $lk        | Contains                                         |
| $lk        | Does not contain                                 |
| $start     | Start with                                       |
| $nstart    | Does not start with                              |
| $end       | End with                                         |
| $nend      | Does not end with                                |
| $na        | true or false - NULL value                       |
| $undefined | true or false - undefined                        |
| $empty     | true or false - combinaison de $na et $undefined |

### Filter on date

| Parameter | Description         |
| --------- | ------------------- |
| $eq       | Equals to           |
| $gt       | Greater than        |
| $gte      | Greater or equal to |
| $lt       | Lower than          |
| $lte      | Lower or equal to   |

### Filter on boolean

| Parameter  | Description                                      |
| ---------- | ------------------------------------------------ |
| $eq        | Equals to                                        |
| $neq       | Does not equal                                   |
| $na        | true or false - NULL value                       |
| $undefined | true or false - undefined                        |
| $empty     | true or false - combinaison de $na et $undefined |

# Evolution

```json
"evo": { 
  "granularity": "D",  // mandatory
  "top": { 
    "max-results": 5, 
    "page-num": 1, 
    "sort": ["-m_visits"], 
    "period": { 
      "p1": [ 
        { 
          "type": "D", 
          "start": "2017-05-12", 
          "end": "2017-05-16" 
        } 
      ] 
    } 
  } 
} 
```

Le paramètre « evo » décrit le contexte du calcul de l’évolution de l’analyse. Il est composé de différents paramètres obligatoires : 

| Parameter   | Description | Possibles values               |
| ----------- | ----------- | ------------------------------ |
| granularity | Granularité de l'évolution                                                                                                                                       | M(onth), W(eek), D(ay), H(our) |
| top         | Description des éléments sélectionnés, top des éléments en fonction de la période, de la pagination et du tri, le nombre d’éléments est dépendant des paramètres | Voir en dessous                |


| Top parameter | Description | Valeurs possibles |
| ------------- | ----------- | ----------------- |
| max-results   | -           | any number        |
| page-num      | -           | any number        |



# Sort by

```json
"sort": [
  "m_visits", 
  "-p1.m_uv"
]
```

The "-" allows you to sort in a decreasing way.

It is possible to sort on as many columns as desired.

It is not possible to sort on a property, the sorting is only done on one or more metrics.

It is not possible to sort on a column not present in the "columns" parameter of the URL.
