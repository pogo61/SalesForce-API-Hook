# Akana API HOOK
![Image of Akana] 
(https://www.akana.com/img/formerlyLOGO8.png) 
[Akana.com](http://akana.com)

## SalesForce API 
### About the API
- Utilise the features of SalesForce.com
- Home Page: [SalesForce.com] (http://www.salesforce.com/)
- API Documentation: [SalesForce.com API docs] (https://developer.salesforce.com)

### Pre-Reqs
- you must ensure that your salesforce login is extenally usable.
    + *add Details*

### Getting Started Instructions
#### Download and Import
- Download SalesForceAPIHook.zip
- Download the migrations.properties file, and edit it to replace the <replace this with your key> text with the "Container Key" of the ND or ND cluster in your target PM.
    - the container key is found by going to the "Deatils Tab" of the ND cluster, or ND defined in the Policy Manager Console, then looking at the " Container Overview" tab on that page, and copying the "Container Key:" value. ![container key screenshot](https://github.com/pogo61/Google-Sheets-API-Integration/blob/master/Screen%20Shot%202015-03-18%20at%2011.24.45%20am.png "ND Container Key")
- Login to PolicyManager  example: http://localhost:9900
- Select the root "Registry" organisation and click on the "Import Package" from the Actions navigation window on the right side of the screen
  - click on button to browse for the DropboxAPIHook.zip archive file 
  - make sure select the migrations.properties file 
  - click Okay to start the importation of the hook.
- this will create a SalesForce API Hook Organisation with the requisite artefacts needed to run the API.
- you can use the API as is calling http://"URL of the Listener of your ND"/sf_hook, or you can create an API in CM and expost it.
    - if you chose to create an API in CM, you must create a Service in your CM tenant which is a Virtual Service of the SalesforceV20_API_vs0 VS that was created by the import. Then Go to CM and create a API from an existing Service.

#### Verify Import
- Expand the services folder in the Google Sheets API Hook you imported and find SalesforceV20_API_vs0 VS

#### Activate Anonymous Contract
- Expand the contracts folder in the Google Sheets API Hook you imported and find the "Anonymous" contract under the "Provided Contracts" folder
- click on the "Activate Contract" workflow activity in the righ-hand Activities portlet
- ensure that the status changes to "Workflow Is Completed"

#### Configure Security
- N/A

#### Verify Connectivity
- Using curl -H "authKey:<the value authKey>" http://"URL of the Listener of your ND"/dropbox_hook/helloworld
-  the response should be similar to the below, listing your spreadsheets:  
    ```{
    "country": "AU",
    "display_name": "Paul Pogonoski",
    "email": "paulpog@japarasolutions.com",
    "email_verified": true,
    "is_paired": false,
    "locale": "en",
    "name_details": {
        "familiar_name": "Paul",
        "given_name": "Paul",
        "surname": "Pogonoski"
    },
    "quota_info": {
        "datastores": 0,
        "normal": 57400105957,
        "quota": 1106222514176,
        "shared": 83305025
    },
    "referral_link": "https://db.tt/ldX0aQUQ",
    "team": null,
    "uid": 12450803
}```

*Note: the authKey in the curl request, above, is retrieved by using the process in the [Dropbox 3-legged OAuth Client.pdf] (https://github.com/pogo61/Dropbox-API-Hook/blob/master/src/Dropbox%203-legged%20OAuth%20Client.pdf) file in the /src directory*


### How Hello World Works
#### An Akana Integration Primer
The Dropbox API Hook is a "Virtual Service". That is, its interface is not that of a real service implementation. It can be a proxy to a "real" implementation, or it can be an aggregate (a combination) of a number of "real" implementations. In Policy Manager a "real" implementation is called a "Physical Service".
Apart from offering a different interface to the Physical Service, a Virtual Service offers the ability to attach Policies for security, logging, QoS, and a number of other non-functional capabilities.
Virtual Services also have the ability to have Custom Process and Scripts run before the Physical Service is called. Here is where a lot of the magic of Integration occurs.

#### Hello World
To create the helloworld operation the following was added to a base RAML document to create the [Dropbox Helloworld.raml] (https://github.com/pogo61/Dropbox-API-Hook/blob/master/src/Dropbox%20Helloworld.raml)  document:  
    /helloworld:  
      &nbsp;get:  
        &nbsp;&nbsp;description: "returns details about the authorised user"  
        &nbsp;&nbsp;&nbsp;responses:  
          &nbsp;&nbsp;&nbsp;&nbsp;200:  
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;body:  
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;application/atom+xml:  

Then a VS was created by using the RAML as the definition source.
Then the /helloworld Operation in the VS was mapped to the GET /account/info operation in the Dropbox_Core_API PS.

Go to the Dropbox_API_Helloworld VS -> Operations Tab -> GET /hellowworld operation -> Process tab you'll see this image:
![Helloworld process] 
(https://github.com/pogo61/Dropbox-API-Hook/blob/master/Screen%20Shot.png)

Double click on the invoke activity to see how these work to make the Hello World operation call successful.


### Create Your Own Integration with the Google Sheets API
The Hello World operation is one simple way of integrating or extending your API's.
Take a look at the [Dropbox API Integration](https://github.com/pogo61/Dropbox-API-Integration). This will give you a deeper inderstanding of the richness of our gateway product in integrating to API's    

### Modify and Build
In the event you need to change the API Hook.   Here are the instructions to do so. 

### License
Put a link to an open source license

