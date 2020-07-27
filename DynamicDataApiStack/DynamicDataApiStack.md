<h1> Dynamic Data API Stack </h1>
<h6>Dynamic Data with Less Complexity, Less Dependency and Less Maintaince</h6>

![Photo by Lukas from Pexels](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Cover-Photo.jpg?raw=true)
<i>[Photo by Lukas from Pexels](https://www.pexels.com/photo/code-coding-computer-data-574077/)</i>

<br><br>


## Introduction

#### About Mobile Applications

Before we first jump on the API and requirement, I would like to first state that Mobile Development is very different than a Web Development. Whereas a Web Page waits for the Dynamic Page from the server to load on the client machine, mobile app draws the UI first and waits for the content to be loaded in its respective containers.
That is why whenever you open any application like Zomato, Swiggy, Spotify, etc You first see the branding, its UI components, and a container with a loader waiting to fetch the data and populate there.

Hence it is very clear that the <b>objective of these APIs</b> are to:
1. Display Dynamic Data with fixed Contract (into a List type format)
2. or to change the state/ property of a view.
   >(Like a Checkbox UI has properties like Text to show on it, Clickable or not, Checked or not, etc)


Also it is very clear that the <b>incoming Data will always be bind to a contract.</b>
We cannot just input any type of data and expect it to work.


Another important point worth mentioning is that Mobile Platforms like Android runs on devices ranging from Rs. 5,000 to even Rs. 55,000 and above. And hence the capabilities of these devices are different. 
In order to be quick and responsive, Android applications plays a major role in rendering the data and handling of data (models like storing the data and syncing with the server) to also parsing and conversion of data. 
Do note that more the processing of a Application, more will be its battery consumption and this plays a significant role in Background processing.
<b>By this we can conclude that data filteration for the APIs should be avoided at Mobile end.</b>

#### Requirement
An Eagle-View of the requirement would be:
- To make our APIs so smart that based on the factors like User, Attributes of a User, Geography, preferences that are set by the Product Owner, etc -- The APIs would curate the data accordingly.
- The APIs are expected to provide data according to the UI. API may have to provide data by combining results from multiple tables, data sources and even filtering them if required. The Base structure and contracts of the API modules should be consistent throughtout. <i>Checkout Public APIs for few top grossing applications like Zomato, Google, Sygic, etc.</i>
- The APIs should be Dynamic. The Terms Dynamic is used at multiple places in these documents and it means that given a <b>fixed contract/ schema</b> the data of the API can easily be controlled (toggled on/off, etc) by the Admin <b>wherever required</b>.

<b>Read more about scaling Mobile App Development and patterns like Backend for Frontends:</b>
- [Scaling Move Development by Eyle on Medium](https://medium.com/better-programming/scaling-mobile-development-365cab84781e)
- [Backend For Frontend (BFF) Pattern by Jose Maria Elias](https://medium.com/zinklar-tech/backend-for-frontend-bff-pattern-5e8810779d9f)
- [Microservices design patterns for CTOs: API Gateway, Backend for Frontend and more](https://tsh.io/blog/design-patterns-in-microservices-api-gateway-bff-and-more/)
- [Backend-For-Frontend using GraphQL under Microservices](https://medium.com/tech-tajawal/backend-for-frontend-using-graphql-under-microservices-5b63bbfcd7d9)

In this Dynamic APIs Stack, we are talking about 3 things:
A Remote Config API which is similar to what we do with Firebase's Remote Config feature, 
An API to store and get the User's settings so that it can be syned on multiple platforms,
And how we will want all our feature dedicated APIs (like Reports, Map Layers, Place Details). Here we have given example of Map Configuration API.
Below we have further discussed each API in detail.

<br>

## 1.  Remote Config

This module contains 

 1. Dashboard for managing the keys
 2. API to get the configurations

The dashboard will have provision to input following from the user:

 - Description (String) (Optional)
 - Key Name (String) (Unique) (No whitespaces allowed) (Syntax: a_b_c)
 - Key Value (String)

> Here 'KeyValue' will be a string that can represent any text,
> integer, double, boolean or a json.

And the API will simply return the KeyName and its KeyValue.
Description should not be returned. It is just for the dashboard.

#### About this API:
This is similar to the Firebase's Remote Config feature, and we may except it to be even better than that.
This API would be completely managed by the Admin of its portal. ie. The Admin of its portal that can either be a Lead Developer or a Product Owner.
The Admin will decide what Key and what value to choose and the same should be returned the the getter API (without any modification in the data).

<b> This API may also have conditions based value, like value of a key may be depending upon certain conditions like Start Date, End Date, Platform, Geography, etc.
 > This requirement may be further explained in detail by Product Team </b>

#### What will it be used for:
This API would be used for Misc. settings and controlling some behavior of the App. 
Like Controlling whether to show AR Button on the Home Screen or not.
This is used for some quick tweakings and 

Do note that this API will NOT be the replacement screen/ feature specific logics, only there dedicated API will. 
For example: On Place Details Page, Should we filter out Trip Advisor Reviews from all the review, its own Place Details API should decide by providing the filtered data.

For Advance Use, in the above example even the API can bind to these configs.

#### How this API will be consumed:
This APIs data would be used as a Map/ Dictionary. ie. Key Value Pair.
Each Key may be bind to a UI Widget Property, we will query the Key and apply its value. 

<br>


##### Here is an example of what Firebase's Remote Config Dashboard looks like:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig.png?raw=true)


##### Here is a more complex example of Firebase's Remote Config where we are passing json as the value:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig-json.png?raw=true)


##### Here is an example of Firebase's Remote Config Dashboard demonstrating grouping of parameters:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig-Groups.png?raw=true)

##### Here is an example of Firebase's Remote Config Dashboard demonstrating condition based values of the parameters:
![Firebase Remote Config](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Screenshot-Firebase-RemoteConfig-Conditions.png?raw=true)

<br> <br>


## 2.  User Settings
This module contains:

 1. API to write the configurations
 2. API to get the configurations

The API will accept "Keys" with their "values" with basic validation and return as it is. 
All the platforms will decide the key on the basis of some common naming convention and make a patch call to the api for writing/ updating the keys to the server.
Subsequently a get call will be made to fetch all the settings. 

> As a user can be on any platform like Andorid, iOS and Web hence platform identifier will NOT be present. These settings would be shared by all the platforms.


#### About this API:
This API would be only used to store Users configuration. Developers/ Product Owners are the best judge of what values to store and how to store them and hence will only be modified by them.
The Key and its value should be decided by the developers/ product owners and do not need any other team to control or modify the data.

> ⚠️ This APIs schema and control flow is yet to be finalized. But the bigger picture is provided here.

<br> <br>


## 3.  Custom Dedicated API for a feature/ UI Component:
### Map Configurations:

This module contains:

 1. API to get the configurations
 
 Here is an sample json just as an example
 > ⚠️ This APIs schema and control flow is yet to be finalized. But the bigger picture is provided here.
```json
    {
  "layers": [
    {
      "title": "MapmyIndia Satellite Map",
      "thumbnail": "https://mmi.com/night/thumbnail.png",
      "styleUrls": {
        "styleUrl": "https://mmi.com/night/style.json",
        
      }
    },
    {
      "title": "MapmyIndia Street Map",
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
> ⚠️ This APIs schema and control flow is yet to be finalized. But the bigger picture is provided here.

This API only serves the purpose of giving the configuration data of Map Objects and Map Layers.
When this API has discrepancy, refresh the User Settings API.

</br> <br>

## Togather in Action

##### Flow Diagram
![Togather in Action](https://github.com/imagineDev/imagineDev.github.io/blob/master/DynamicDataApiStack/assets/Flow-Diag-1.png?raw=true)

</br>

##### Explanation

<strong><em>Step 1.</em></strong> It indicates fetching of Remote Configuration Data on App Start and storing it on local storage. As this data will be cached, a time threshold can also be added. Say Fire the API on every app start if last data was fetched (Say THRESHOLD = 15 mins) earlier.
> As it is a network call and it can take some time or even fail, this configuration data will always be backed by a default data that will be stored on the device itself.
&nbsp;
<b>Note: This will be configured from the Remote Config Web Portal</b>

<strong><em>Step 2.</em></strong> It indicates fetching of User Settings Data. There may be certain events when this API will be triggered, one being App Start.
> When we say Data Syncing, it depends on how tight syncing we are expecting. Some Apps prefer tight syncing in which a session is maintained and data is immediately synced to and fro the server. Some Apps prefer light syncing in which on certain events the data is synced. Like App Start, Time Interval, Triggered on a push command (Push Notification - Data Message for command purpose only.).. 
Using a realtime database can be considered here.


<strong><em>Step 3.</em></strong> It indicates fetching of Map Configuration Data. Configuration data basically control the Map Layers section of Move App. It will dynamically provide the Map Layers, Report Data, etc.
Product can even ask to fetch the data on basis of some paramteres like User's Current Location. Say: Show "Street Map" layer to only those in Banglore Area.
&nbsp;
<b>Note: This will be configured from the a Config Web Portal, where the actor (Product Owner) can control what specific data to show.</b>

<strong><em>Step 4.</em></strong> It indicates that the Map Styles or Layer that we have to apply or operate on must be synced with the 'Map Style Cloud'. <b>ie. The Configuration API should only return those styles that are deployed on the Map Style Cloud and are supported by the Map SDK.
Also Map SDK should always keep the options OPEN to apply style and operate on Map Layers and other Geometry objects by Move App</b>

</br></br></br>
## Thanks
