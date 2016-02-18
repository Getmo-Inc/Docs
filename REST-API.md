# Smartpush
---
## REST API Documentation
This document describes the services exposed by the SMARTPUSH REST API platform. The SMARTPUSH platform is a layer of Getmos Company push service.

To access the service you must have the credentials: **`devid`**, **`appid`**. Ask our support team via e-mail: suporte@getmo.com.br.

### API Endpoint
```
https://api.getmo.com.br

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
    
    - **platform**: (required) **`string`** | `"IOS"`, `"ANDROID"`, `"WINDOWS"`, `"CHROME"`, `"SAFARI"`, `"FIREFOX"`
    
    - **params**: (required) **`object`**
    
      The `params` object hold the information that you will send through push notification. Also know as `payload`, every platform has his own `payload` properties schema. See below:
      
      - IOS:
      
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
            "url": "http://www.getmo.com.br",
            "your-custom-property-2": "you custom value 2",
            "your-custom-property-3": "you custom value 3",
            "your-custom-property-4": "you custom value 4"
        }
        ```
        
        See complete docs on [APNS Notification Payload](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/TheNotificationPayload.html).
      
      - ANDROID:
      
        In progress...
      
      - WINDOWS:
      
        In progress...
      
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
            "text": "Big Saving, No Waiting. Mega Deals!",
        }
        ```
        Complete Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "text": "Big Saving, No Waiting. Mega Deals!",
            "icon": "https://admin.getmo.com.br/assets/img/logo-getmo.png",
            "clickUrl": "http://www.getmo.com.br",
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
            "clickUrl": "http://www.getmo.com.br",
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
            "text": "Big Saving, No Waiting. Mega Deals!",
        }
        ```
        Complete Example:
        ```json
        {
            "title": "Getmo Smartpush",
            "text": "Big Saving, No Waiting. Mega Deals!",
            "icon": "https://admin.getmo.com.br/assets/img/logo-getmo.png",
            "clickUrl": "http://www.getmo.com.br",
        }
        ```
    
  - **filter**:
  
    In progress...
     
  - **Full Example: (in progress)**
  ```json
  {}
  ```

- **Success Response**
    
    - Code: 200
    - Content: `{"status":true,"code":200,"message":"Success","pushid":"..."}`
       - status: `true`
       - code: `200`
       - message: `Success`
       - pushid: `pushid`. Unique token for this request. Ex: `84375893c8eadfabd79d81592f244ed2`

- **Error Response**

    - Code: 404, 406, 422 or 500
    - Content: `{"status":false,"code":"...","message":"..."}`
        - status: `false`
        - code: `404`, `406`, `422`, `500
        - message: `Custom message with more details about the error.`

Docs In progress