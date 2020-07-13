# Dynamic Data API Stack

These APIs are based on LESS* Principle.
*Less Complexity
Less Dependency
Less Maintenance*
###### *Concept by Sahil Sharma

## 1.  Remote Config
This module contains 

 1. Dashboard for managing the keys
 2. API to get the configurations

The dashboard will have provision to input following from the user:

 - Description (String) (Optional)
 - Key Name (String) (No whitespaces allowed) (Syntax: a_b_c)
 - Key Value (String)

> Here 'KeyValue' will be a string that can represent any text,
> integer, double, boolean or a json.

And the API will simply return the KeyName and its KeyValue.
Description should not be returned. It is just for the dashboard.

##### Here is an example of what Firebase's Remote Config Dashboard looks like:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/Screenshot-Firebase-RemoteConfig.png?raw=true)

##### Here is a more complex example of Firebase's Remote Config where we are passing json as the value:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/Screenshot-Firebase-RemoteConfig-json.png?raw=true)


##### Here is an example of Firebase's Remote Config Dashboard demonstrating grouping of parameters
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/Screenshot-Firebase-RemoteConfig-Groups.png?raw=true)


## 2.  User Settings
This module contains:

 1. API to write the configurations
 2. API to get the configurations

The API will accept "Keys" with their "values" with basic validation and return as it is. 
All the platforms will decide the key on the basis of some common naming convention and make a patch call to the api for writing/ updating the keys to the server.
Subsequently a get call will be made to fetch all the settings. 

> As a user can be on any platform like Andorid, iOS and Web hence platform identifier will NOT be present. These settings would be shared by all the platforms.

## 3.  Map Configurations

This module contains:

 1. API to get the configurations
```
    {
  "layers": [
    {
      "title": "MapmyIndia Night Style",
      "thumbnail": "https://mmi.com/night/thumbnail.png",
      "styleUrls": {
        "styleUrl": "https://mmi.com/night/style.json",
        "nightStyleUrl": "https://mmi.com/night/style-night.json"
      }
    }
  ],
  "events": [
    {
      "parentId": 100,
      "parentName": "Traffic",
      "childrens": [
        {
          "name": "Traffic Jam",
          "id": 1001,
          "icons": {
            "smallIcon": "https://cdn.mmi.com/icon/123.png",
            "largeIcon": "https://cdn.mmi.com/icon/123.png"
          },
          "subChildrens": [
            {
              "name": "Traffic Jam",
              "id": 100002,
              "icons": {
                "smallIcon": "https://cdn.mmi.com/icon/123.png",
                "largeIcon": "https://cdn.mmi.com/icon/123.png"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

This API only serves the purpose of giving the configuration data of Map Objects and Map Layers.
When this API has discrepancy, refresh the User Settings API.

