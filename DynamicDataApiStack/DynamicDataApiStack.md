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
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig.png?raw=true)


##### Here is a more complex example of Firebase's Remote Config where we are passing json as the value:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig-json.png?raw=true)


##### Here is an example of Firebase's Remote Config Dashboard demonstrating grouping of parameters
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig-Groups.png?raw=true)


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


## Togather in Action


![Togather in Action](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Flow-Diag-1.png?raw=true)
##### Flow Diagram

Step 1. It indicates fetching of Remote Configuration Data on App Start and storing it on local storage. As this data will be cached, a time threshold can also be added. Say Fire the API on every app start if last data was fetched (Say THRESHOLD = 15 mins) earlier.
> As it is a network call and it can take some time or even fail, this configuration data will always be backed by a default data that will be stored on the device itself.
&nbsp;
<b>Note: This will be configured from the Remote Config Web Portal</b>

Step 2. It indicates fetching of User Settings Data. There may be certain events when this API will be triggered, one being App Start.
> When we say Data Syncing, it depends on how tight syncing we are expecting. Some Apps prefer tight syncing in which a session is maintained and data is immediately synced to and fro the server. Some Apps prefer light syncing in which on certain events the data is synced. Like App Start, Time Interval, Triggered on a push command (Push Notification - Data Message for command purpose only.).. 
Using a realtime database can be considered here.


Step 3. It indicates fetching of Map Configuration Data. Configuration data basically control the Map Layers section of Move App. It will dynamically provide the Map Layers, Report Data, etc.
Product can even ask to fetch the data on basis of some paramteres like User's Current Location. Say: Show "Street Map" layer to only those in Banglore Area.
&nbsp;
<b>Note: This will be configured from the a Config Web Portal, where the actor (Product Owner) can control what specific data to show.</b>

Step 4. It indicates that the Map Styles or Layer that we have to apply or operate on must be synced with the 'Map Style Cloud'. <b>ie. The Configuration API should only return those styles that are deployed on the Map Style Cloud and are supported by the Map SDK.
Also Map SDK should always keep the options OPEN to apply style and operate on Map Layers and other Geometry objects by Move App</b>
