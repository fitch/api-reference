## Basics

Houm.io provides a REST/Websocket for controlling and configuring lights.

A `site` is conceptually equal to a home, office etc. It contains your lights, scenes, button configurations and so on. A `sitekey` is a key for accessing a site.

Contact Houm.io for a sitekey, if you don't have one.

Once you have a sitekey, for example `yoursecretsitekey`, the web UI for your site is at https://houm.herokuapp.com/site/yoursecretsitekey.

## Using the REST API

All API calls are prefixed by `https://houm.herokuapp.com/api/site/yoursecretsitekey`.

We use [curl](http://curl.haxx.se/) to execute the api calls in our examples.

### Get all lights

Execute the following command:

`curl -X GET https://houm.herokuapp.com/site/yoursecretsitekey/light`

The response is similar to:

```
[
  {
    "vendor": "enocean",
    "_id": "5400d38340396f02001bf573",
    "siteid": "yoursecretkey",
    "name": "Eteinen: Kattovalot",
    "type": "dimmable",
    "enoceanid": "unknown",
    "subdef": "ff:ee:33:00",
    "on": true,
    "bri": 255
  },
  {
    "vendor": "enocean",
    "_id": "5400e45a6f77220200232516",
    "siteid": "yoursecretkey",
    "name": "Kylpyhuone: Etuvalot",
    "enoceanid": "unknown",
    "subdef": "ff:ee:33:05",
    "type": "dimmable",
    "on": true,
    "bri": 255
  },
  {
    "vendor": "enocean",
    "_id": "5400e3e06f77220200232515",
    "siteid": "yoursecretkey",
    "name": "Kylpyhuone: Takavalot",
    "enoceanid": "unknown",
    "subdef": "ff:ee:33:04",
    "type": "dimmable",
    "on": true,
    "bri": 255
  }
]
```

Essential parts are:

* `_id`: This is what you use to refer to this light (see below).
* `name`: This is the name of the light.
* `type`: This is either `dimmable` (dimmable light) or `binary` (on/off light)
* `on`: Whether the light is on or off.
* `bri`: The brightness of a light. For dimmable lights, the value is in the range 0–255. 0 means off, 255 means full steam. For binary lights, this is always 0 or 255.

### Set light state

Get the `_id` of the light you want to set.

To set a dimmable light to 50%, execute the following command.

```
curl \
-X PUT \
-d '{ "_id": "yourlightid", "on": true, "bri": 127 }' \
-H "Content-Type: application/json" \
https://houm.herokuapp.com/site/yoursecretsitekey/light/state
```

To set a dimmable light at full power, set data to

`'{ "_id": "yourlightid", "on": true, "bri": 255 }'`.

To switch off a dimmable light, set data to

`'{ "_id": "yourlightid", "on": false, "bri": 0 }'`.

### Get all scenes

`curl -X GET https://houm.herokuapp.com/site/yoursecretsitekey/scene`

The response is similar to:

```
[
    {
        "_id": "53fe861087b294c57dca3920",
        "name": "Iltavalot",
        "siteid": "yoursecretsitekey",
        "state": [
            {
                "_id": "53fbbd7a8cfc4a98a86e54bf",
                "bri": 133,
                "on": true
            },
            {
                "_id": "53fbc2abcf9f7a42c93337ee",
                "bri": 213,
                "on": true
            },
            {
                "_id": "53fbc61c0944c1dcce607ed5",
                "bri": 84,
                "on": true
            }
        ]
    },
    {
        "_id": "53fc52092cf918c209bdefea",
        "name": "Kaikki pois",
        "siteid": "yoursecretsitekey",
        "state": [
            {
                "_id": "53fbbd7a8cfc4a98a86e54bf",
                "bri": 0,
                "on": false
            },
            {
                "_id": "53fbc2abcf9f7a42c93337ee",
                "bri": 0,
                "on": false
            },
            {
                "_id": "53fbc61c0944c1dcce607ed5",
                "bri": 0,
                "on": false
            }
        ]
    }
]
```

### Activate a scene

Get the `_id` of the scene you want to activate.

```
curl \
-X PUT \
-d '{ "_id": "yoursceneid" }' \
-H "Content-Type: application/json" \
https://houm.herokuapp.com/api/site/yoursecretsitekey/scene/apply
```

### Undocumented API methods

- Sites
  - Get site data
  - Modify site data
  - Get site connections
  - Get next site subdef for EnOcean devices
- Lights
  - Get light groups
  - Create new light
  - Delete light
  - Modify light data (for example, name)
  - Toggle a light group
- Scenes
  - Create new scene
  - Delete scene
  - Modify scene data
- Sensors
  - Get sensors
  - Add sensor
  - Delete sensor
  - Modify sensor data
  - Add an action to a sensor event

If you need one or more of these to be documented, please create an issue.

## Using the Websocket API

If you need documentation on the Websocket API, please create an issue.
