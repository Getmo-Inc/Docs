# Smartpush
---
## REST API Documentation
This document describes the services exposed by the SMARTPUSH REST API platform. The SMARTPUSH platform is a layer of Getmos Company push service.

To access the service you must have the credentials: **`devid`**, **`appid`**. Ask our support team via e-mail: suporte@getmo.com.br.

## Libraries

- [PHP Connector](https://github.com/Getmo-Inc/Smartpush-Php-Connector)
- [Java Connector](https://github.com/Getmo-Inc/Smartpush-Java-Connector)

### API Endpoint
```
https://api.getmo.com.br (recomended)

or

http://api.getmo.com.br
```

### Send Push Notification

- **URL**

    /push

- **Method**

    POST

- **Params**

  data: (required) **`JSON string`** | `"{}"`
  
  Examle:
  ```
  data={...}
  ```
    
  **JSON String Data:**

  - **alias**: (optional) **`string`** | `"Smartpush Campaign"`
    
    Custom identifier for this push notification.
      
  - **prod**: (optional) **`number`** | `0`, `1`
    
    Environment to be used.
    - `0` for Sandbox.
    - `1` for Production.
    
  - **when**: (required) **`string`** | `"now"`, `"0000-00-00 00:00:00"`, `"00/00/0000 00:00:00"`
    
    Define a date and time to send the push request.
    
    - `now` will send the push request as soon as possible.
    - If you choose one of the two date patterns the system will send the push notification based on your timezone configured in [admin.getmo.com.br/profile](https://admin.getmo.com.br/profile).
  
  - **devid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Developer Identifier.
    
    - After register and activate your developer account, you find the `devid` in your profile page: [admin.getmo.com.br/profile](https://admin.getmo.com.br/profile)
  
  - **notifications**: (required) **`array`** 

    An array of `objects` with the following properties: `appid`, `platform`, `params`.
    
    - **appid**: (required) **`string`** | `"000000000000000"`
    
      Unique Smartpush Application Identifier.
      
      - After register your application, you find the `appid` in the apps list page: [admin.getmo.com.br/apps](https://admin.getmo.com.br/apps)
    
    - **platform**: (required) **`string`** | `"iOS"`, `"ANDROID"`, `"WINDOWS"`, `"CHROME"`, `"SAFARI"`, `"FIREFOX"`
    
    - **params**: (required) **`object`**
    
      The `params` object hold the information that you will send through push notification. Also know as `payload`, every platform has his own `payload` properties schema. See below:
      
      - iOS:
      
        - **aps**: (required) **`object`**
               
          - **alert**: (required) **`string`** | `"Getmo Smartpush. Big Saving, No Waiting. Mega Deals!"`
          
            The alert property is where you put in the text that will show on push notification.
            
          - **badge**: (optional) **`number`** | `1`, `2`, `...`
            
          - **sound**: (optional) **`string`** | `"default"`
          
        Example:
        ```json
        {
            "aps": {
                "alert": "Getmo Smartpush. Big Saving, No Waiting. Mega Deals!"
            }
        }
        ```  
          
        Custom properties can be send through, adding extra entries outside the `aps` object.
        ```json
        {
            "aps": {
                "alert": "Getmo Smartpush. Big Saving, No Waiting. Mega Deals!"
            },
            "your-custom-url": "http://www.getmo.com.br",
            "your-custom-property-2": "you custom value 2",
            "your-custom-property-3": "you custom value 3",
            "your-custom-property-4": "you custom value 4"
        }
        ```
        
        See complete docs on [APNS Notification Payload](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/TheNotificationPayload.html).
      
      - ANDROID:
      
        Android gives you flexibility, so you don't need to follow conventions. The payload schema is you that defines.
        ```json
        {
            "your-custom-title": "Getmo Smartpush",
            "your-custom-text": "Big Saving, No Waiting. Mega Deals!",
            "your-custom-url": "http://www.getmo.com.br",
            "your-custom-icon": "7",
            "your-custom-property-2": "you custom value 2",
            "your-custom-property-3": "you custom value 3",
            "your-custom-property-4": "you custom value 4"
        }
        ```
      
      - WINDOWS:
      
        Documentation in progress...
      
      - CHROME:
        
        - **title**: (required) **`string`** | `"Getmo Smartpush"`
         
          Title of the notification in one line.
        
        - **text**: (required) **`string`** | `"Big Saving, No Waiting. Mega Deals!"`
          
          The text can be longer, if necessary Chrome will show it in multi-lines.
        
        - **icon**: (optional) **`string`** | `"https://admin.getmo.com.br/assets/img/logo-getmo.png"` 
        
          To show a custom icon insert the absolute URL to the image that you will show on the notification.
          
        - **clickUrl**: (optional) **`string`** | `"http://www.getmo.com.br"`
         
          To make the notification clickable, you need to insert a absolute URL the page that you will target.  
      
        Simple Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "text": "Big Saving, No Waiting. Mega Deals!"
        }
        ```
        Complete Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "text": "Big Saving, No Waiting. Mega Deals!",
            "icon": "https://admin.getmo.com.br/assets/img/logo-getmo.png",
            "clickUrl": "http://www.getmo.com.br"
        }
        ```

      - SAFARI:
      
        - **title**: (required) **`string`** | `"Getmo Smartpush"`
                 
          Title of the notification in one line.
            
        - **body**: (required) **`string`** | `"Big Saving, No Waiting. Mega Deals!"`
              
          The text (body) can be longer, if necessary Safari will show it in multi-lines.
            
        - **action**: (optional) **`string`** | `"See More"` 
            
          The label of the action button.
              
        - **clickUrl**: (required) **`string`** | `"http://www.getmo.com.br"`
             
          **Attention:** For safari web push notification the `clickUrl` is required. If you don't inform a correct absolute URL, the request will be rejected.
        
        Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "body": "Big Saving, No Waiting. Mega Deals!",
            "clickUrl": "http://www.getmo.com.br"
        }
        ```
        See complete docs on [Apple Notification for Websites](https://developer.apple.com/library/mac/documentation/NetworkingInternet/Conceptual/NotificationProgrammingGuideForWebsites/PushNotifications/PushNotifications.html#//apple_ref/doc/uid/TP40013225-CH3-SW12).
         
      - FIREFOX:
      
        - title: (required) **`string`** | `"Getmo Smartpush"`
       
          Title of the notification in one line.
      
        - text: (required) **`string`** | `"Big Saving, No Waiting. Mega Deals!"`
        
          The text can be longer, if necessary Firefox will show it in multi-lines.
      
        - icon: (optional) **`string`** | `"https://admin.getmo.com.br/assets/img/logo-getmo.png"` 
      
          To show a custom icon insert the absolute URL to the image that you will show on the notification.
        
        - clickUrl: (optional) **`string`** | `"http://www.getmo.com.br"`
       
          To make the notification clickable, you need to insert a absolute URL the page that you will target.  
    
        Simple Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "text": "Big Saving, No Waiting. Mega Deals!"
        }
        ```
        Complete Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "text": "Big Saving, No Waiting. Mega Deals!",
            "icon": "https://admin.getmo.com.br/assets/img/logo-getmo.png",
            "clickUrl": "http://www.getmo.com.br"
        }
        ```
    
  - **filter**: (required) **`object`**
  
    - **type**: (required) **`string`** | `"TAG"`
    
      The type of the filter.
      
    - **range**: (optional) **`string`** | `"all"`
          
      The range of days that the filter will use when the job is processed. The entry can be one of these: `7`, `15`, `30`, `60`, `90`, `all` (default).
      
    - **operator**: (optional) **`string`** | `"AND"`
                
      The operator that the filter will use to do the comparisons. The entry can be one of these: `AND` (default), `OR`.
    
    - **rules**: (required) **`array`**
                
      An array of `arrays` with the following entries: `["tag", "comparator", "value"]`.
      
      - **[0] = tag**: (required) **`string`** | `"..."`
      
        There are five types of tags that you can register on the Administrator, `STRING`, `NUMERIC`, `TIMESTAMP`, `BOOLEAN`, `LIST`, and it must be done before usage. Visit [admin.getmo.com.br/tags](https://admin.getmo.com.br/apps). 
      
      - **[1] = comparator** (required) **`string`** | `"="`, `"<"`, `">"`, `"<="`, `">="`, `"IN"`.
      
        Comparators diferent than `"="` and `"IN"` can be used only on `NUMERIC` and `TIMESTAMP` tag types
        
      - **[2] = value** (required) **`string`** or **`array`**

        If you choose `"IN"` as comparator, the **value** must be an **`array`** of strings. Otherwise the value must be a **`string`**.
        
      Example with `"="` comparator:
      ```json
      ["NOTIFICATION_CONTROL", "=", "UPDATES"]
      ```
      
      Example with `"IN"` comparator:
      ```json
      ["NOTIFICATION_CONTROL", "IN", ["UPDATES", "TECHNOLOGY", "COMMERCIAL"]]
      ```
     
  - **Full Example:**
  ```json
  {
      "alias": "Smartpush Campaign",
      "prod": 1,
      "when": "01/03/2016 14:30:00",
      "devid": "000000000000000",
      "notifications": [{
          "appid": "000000000000000",
          "platform": "IOS",
          "params": {
              "aps": {
                  "alert": "Getmo Smartpush. Big Saving, No Waiting. Mega Deals!",
                  "badge": "1",
                  "sound": "default"
              },
              "url": "http://www.getmo.com.br"
          }
      },{
          "appid": "000000000000000",
          "platform": "ANDROID",
          "params": {
              "title": "Getmo Smartpush",
              "detail": "Big Saving, No Waiting. Mega Deals!",
              "url": "http://www.getmo.com.br"
          }
      },{
          "appid": "000000000000000",
          "platform": "CHROME",
          "params": {
              "title": "Getmo Smartpush",
              "text": "Big Saving, No Waiting. Mega Deals!",
              "icon": "https://admin.getmo.com.br/assets/img/logo-getmo.png",
              "clickUrl": "http://www.getmo.com.br"
          }
      },{
          "appid": "000000000000000",
          "platform": "SAFARI",
          "params": {
              "title": "Getmo Smartpush",
              "text": "Big Saving, No Waiting. Mega Deals!",
              "action": "See More",
              "clickUrl": "http://www.getmo.com.br"
          }
      },{
         "appid": "000000000000000",
         "platform": "FIREFOX",
         "params": {
             "title": "Getmo Smartpush",
             "text": "Big Saving, No Waiting. Mega Deals!",
             "icon": "https://admin.getmo.com.br/assets/img/logo-getmo.png",
             "clickUrl": "http://www.getmo.com.br"
         }
     }],
     "filter": {
        "type": "TAG",
        "rules": [
            ["STATUS", "=", "ON-BOARD"],
            ["NOTIFICATION_CONTROL", "IN", ["UPDATES", "TECHNOLOGY", "COMMERCIAL"]]
        ]
     }
  }
  ```
  
  Simple Example:
  ```json
  {
      "alias": "Smartpush Campaign",
      "prod": 1,
      "when": "now",
      "devid": "000000000000000",
      "notifications": [{
          "appid": "000000000000000",
          "platform": "ANDROID",
          "params": {
              "title": "Getmo Smartpush",
              "detail": "Big Saving, No Waiting. Mega Deals!"
          }
      }],
      "filter": {
          "type": "TAG",
          "range": "30",
          "operator": "OR",
          "rules": [
              ["LAST_ORDER_DATE", ">=", "1467401769"],
              ["SMARTPUSH_ID", "IN", ["ONAX", "EST", "SERUM", "CTHULHU"]]
          ]
      }
  }
  ```

- **Success Response**
    
    - Code: 200
    - Response:
      - `pushid`: Unique token generated for this push notification request.
    ```json
    {
        "status": true,
        "code": 200,
        "message": "Success",
        "pushid": "00000000000000000000000000000000"
    }
    ```
      

- **Error Response**

    - Code: 404, 406, 422 or 500
    - Response:
      - `message`: Custom message with more details about the error.
    ```json
    {
        "status": false,
        "code": "404",
        "message": "devid not found."
    }
    ```      

### Get Information about a Push Notification

- **URL**

    /push/{devid}/{pushid}

- **Method**

    GET

- **Success Response**
    
    - Code: 200
    - Response with notification **status** = `WAITING`
    ```json
    {
        "status": true,
        "code": 200,
        "message": "Success",
        "time-left": "7 minute from now",
        "date": "15/02/2016 21:30:00",
        "pushid":"00000000000000000000000000000000",
        "notifications":[{
            "appid": "000000000000000",
            "status":"WAITING"
        }]
    }
    ```
    - Response with notification **status** = `SENT`
    ```json
    {
        "status": true,
        "code": 200,
        "message": "Success",
        "pushid": "00000000000000000000000000000000",
        "notifications": [{
            "appid": "000000000000000",
            "status": "SENT",
            "sent_at": "15/02/2016 21:30:03"
        }]
    }
    ```


### Cancel a Push Notification

- **URL**

    /push/{devid}/{pushid}/cancel

- **Method**

    PUT

- **Success Response**

    - Code: 200
    - Response with notification **status** = `WAITING`
    ```json
    {
        "status": true,
        "code": 200,
        "message": "Success",
        "pushid": "00000000000000000000000000000000",
        "notifications": [{
            "appid": "000000000000000",
            "status": "CANCELED"
        }]
    }
    ```
    - Response with notification **status** = `SENT`
    ```json
    {
        "status": true,
        "code": 200,
        "message": "Success",
        "pushid": "00000000000000000000000000000000",
        "notifications": [{
            "appid": "000000000000000",
            "status": "SENT",
            "message": "The status of this Notification could not be changed because it has already been sent."
        }]
    }
    ```
    - Response with notification **status** = `CANCELED`
    ```json
    {
        "status": true,
        "code": 200,
        "message": "Success",
        "pushid": "00000000000000000000000000000000",
        "notifications": [{
            "appid": "000000000000000",
            "status": "CANCELED",
            "message": "The status of this Notification could not be changed because it has already been canceled."
        }]
    }
    ```
    
    
### Hide a Push Notification

- **URL**

    /push/{devid}/{pushid}/hidden

- **Method**

    PUT

- **Success Response**

    - Code: 200
    - Response
    ```json
    {
      "status": true,
      "message": "Success",
      "pushid": "00000000000000000000000000000000",
      "hidden": 1
    }
    ```
    
    
### Hide a Push Notification for a specific Device 

- **URL**

    /push/{devid}/{pushid}/hidden/{appid}/{hwid}

- **Method**

    PUT

- **Success Response**

    - Code: 200
    - Response
    ```json
    {
      "status": true,
      "message": "Success",
      "devid": "000000000000000",
      "pushid": "00000000000000000000000000000000",
      "appid": "000000000000000",
      "hwid": "000000000000000",
      "hidden": 1
    }
    ```


### Get Last ten (10) Notifications

- **URL**

    /notifications/last

- **Method**

    POST

- **Params**

  - **devid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Developer Identifier.
      
  - **appid**: (required) **`string`** | `"000000000000000"`
  
    Unique Smartpush Application Identifier.
    
  - **hwid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Device Identifier.
    
  - **platform**: (required) **`string`** | `"iOS"`, `"ANDROID"`, `"WINDOWS"`, `"CHROME"`, `"SAFARI"`, `"FIREFOX"`
  
  - **startingDate**: (optional) **`datetime`** | `"Y-m-d H:i:s"`
  
    With this optional parameter you can define a start point in time to get the notifications.
    
  - **dateFormat**: (optional) **`string`** | `"Y-m-d H:i:s"` (default), `"d/m/Y H:i:s"`, `"d-m-Y H:i:s"`, `"d.m.Y H:i:s"` 
      
    With the dateFormat parameter you can choose one of the templates for the output date format.

- **Success Response**

  **Important:** All dates returned are in UTC.

    - Code: 200
    - Response
    ```json
    [
      {
        "pushid": "00000000000000000000000000000000",
        "clicked": 0,
        "payload": {
          "title": "Example Title",
          "message": "Example Message"
        },
        "extra": {
          "title": "Example Title",
          "message": "Example Message"
        },
        "created_at": "0000-00-00 00:00:00",
        "sent_at": "0000-00-00 00:00:00"
      },
      {
        "pushid": "00000000000000000000000000000000",
        "clicked": 0,
        "payload": {
          "title": "Example Title 2",
          "message": "Example Message 2"
        },
        "extra": {
          "title": "Example Title 2",
          "message": "Example Message 2"
        },
        "created_at": "0000-00-00 00:00:00",
        "sent_at": "0000-00-00 00:00:00"
      }
    ]
    ```


### Get Last ten (10) Unread Notifications

- **URL**

    /notifications/unread

- **Method**

    POST

- **Params**

  - **devid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Developer Identifier.
      
  - **appid**: (required) **`string`** | `"000000000000000"`
  
    Unique Smartpush Application Identifier.
    
  - **hwid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Device Identifier.
    
  - **platform**: (required) **`string`** | `"iOS"`, `"ANDROID"`, `"WINDOWS"`, `"CHROME"`, `"SAFARI"`, `"FIREFOX"`
  
  - **startingDate**: (optional) **`datetime`** | `"Y-m-d H:i:s"`
  
    With this optional parameter you can define a start point in time to get the notifications.
    
  - **dateFormat**: (optional) **`string`** | `"Y-m-d H:i:s"` (default), `"d/m/Y H:i:s"`, `"d-m-Y H:i:s"`, `"d.m.Y H:i:s"` 
      
    With the dateFormat parameter you can choose one of the templates for the output date format.

- **Success Response**

  **Important:** All dates returned are in UTC.

    - Code: 200
    - Response
    ```json
    [
      {
        "pushid": "00000000000000000000000000000000",
        "payload": {
          "title": "Example Title",
          "message": "Example Message"
        },
        "extra": {
          "title": "Example Title",
          "message": "Example Message"
        },
        "created_at": "0000-00-00 00:00:00",
        "sent_at": "0000-00-00 00:00:00"
      },
      {
        "pushid": "00000000000000000000000000000000",
        "payload": {
          "title": "Example Title 2",
          "message": "Example Message 2"
        },
        "extra": {
          "title": "Example Title 2",
          "message": "Example Message 2"
        },
        "created_at": "0000-00-00 00:00:00",
        "sent_at": "0000-00-00 00:00:00"
      }
    ]
    ```


### Get the Extra payload of a specific Push Notification

- **URL**

    /notifications/extra

- **Method**

    POST

- **Params**

  - **devid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Developer Identifier.
      
  - **appid**: (required) **`string`** | `"000000000000000"`
  
    Unique Smartpush Application Identifier.
    
  - **pushid**: (required) **`string`** | `"00000000000000000000000000000000"`
    
    Unique Smartpush Push Identifier.
    
- **Success Response**

    - Code: 200
    - Response
    ```json
    {
      "title": "Example Title",
      "message": "Example Message"
    }
    ```  


### Mark one Push Notification as Read

- **URL**

    /notifications/read-one

- **Method**

    DELETE

- **Params**

  - **devid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Developer Identifier.
      
  - **appid**: (required) **`string`** | `"000000000000000"`
  
    Unique Smartpush Application Identifier.
    
  - **hwid**: (required) **`string`** | `"000000000000000"`
        
    Unique Smartpush Device Identifier.
    
  - **pushid**: (required) **`string`** | `"00000000000000000000000000000000"`
      
    Unique Smartpush Push Identifier.  
    
- **Success Response**

  **Important:** This request is processed in background, so it will always return success.

    - Code: 200
    - Response
    ```json
    {
      "status": true,
      "message": "Success"
    }
    ```  
    
    
### Mark all Push Notifications as Read

- **URL**

    /notifications/read-all

- **Method**

    DELETE

- **Params**

  - **devid**: (required) **`string`** | `"000000000000000"`
    
    Unique Smartpush Developer Identifier.
      
  - **appid**: (required) **`string`** | `"000000000000000"`
  
    Unique Smartpush Application Identifier.
    
  - **hwid**: (required) **`string`** | `"000000000000000"`
        
    Unique Smartpush Device Identifier.
    
- **Success Response**

  **Important:** This request is processed in background, so it will always return success.

    - Code: 200
    - Response
    ```json
    {
      "status": true,
      "message": "Success"
    }
    ```  


### Support
Jonathan Martins
webmaster@getmo.com.br
---

> Developed by Getmo