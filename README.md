
# Anypoint Template: Workday to Salesforce Sales Item to Product Migration

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [Salesforce Considerations](#salesforceconsiderations)
	* [Workday Considerations](#workdayconsiderations)
	* [Workday Financials Considerations](#workdayfinancialsconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
As a Workday admin I want to migrate Sales Items to Salesforce Products.

This Template should serve as a foundation for the process of migrating Sales items from Workday.

As implemented, this template leverages the [Batch Module](http://www.mulesoft.org/documentation/display/current/Batch+Processing).
The batch job is divided into *Process* and *On Complete* stages.

Migration process starts from querying all the existing Sales items at Workday.
In the *Process* stage, the template queries the Salesforce for already existing Products based on the Product Names retrieved from Workday.
The Products are inserted or updated in the Salesforce system based on the results of these queries. Afterwards, the Salesforce system is queried for Pricebook Entry, which belongs to the Product.
If Pricebook Entry was found, it is updated, otherwise the Pricebook Entry for the Product is created.

Finally during the *On Complete* stage the Template will both output statistics data into the console and send a notification email with the results of the batch execution.

# Considerations <a name="considerations"/>

There are certain pre-requisites that must be considered to run this Anypoint Template. All of them deal with the preparations in both source and destination systems, that must be made in order for all to run smoothly. **Failing to do so could lead to unexpected behavior of the template.**



## Salesforce Considerations <a name="salesforceconsiderations"/>

There may be a few things that you need to know regarding Salesforce, in order for this template to work.

In order to have this template working as expected, you should be aware of your own Salesforce field configuration.

### FAQ

 - Where can I check that the field configuration for my Salesforce instance is the right one?

    [Salesforce: Checking Field Accessibility for a Particular Field][1]

- Can I modify the Field Access Settings? How?

    [Salesforce: Modifying Field Access Settings][2]


[1]: https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US
[2]: https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US


### As destination of data

There are no particular considerations for this Anypoint Template regarding Salesforce as data destination.


## Workday Considerations <a name="workdayconsiderations"/>

### As source of data

There are no particular considerations for this Anypoint Template regarding Workday as data origin.





## Workday Financials Considerations <a name="workdayfinancialsconsiderations"/>

### As source of data

There are no particular considerations for this Anypoint Template regarding Workday Financials as data origin.


# Run it! <a name="runit"/>
Simple steps to get Workday to Salesforce Sales Item to Product Migration running.


## Running on premise <a name="runonopremise"/>
In this section we detail the way you should run your Anypoint Template on your computer.


### Where to Download Mule Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Mule Studio from this [Location](http://www.mulesoft.com/platform/mule-studio)
+ You can download Mule ESB from this [Location](http://www.mulesoft.com/platform/soa/mule-esb-open-source-esb)


### Importing an Anypoint Template into Studio
Mule Studio offers several ways to import a project into the workspace, for instance:

+ Anypoint Studio Project from File System
+ Packaged mule application (.jar)

You can find a detailed description on how to do so in this [Documentation Page](http://www.mulesoft.org/documentation/display/current/Importing+and+Exporting+in+Studio).


### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click on you Anypoint Template project folder
+ Hover you mouse over `"Run as"`
+ Click on  `"Mule Application (configure)"`
+ Inside the dialog, select Environment and set the variable `"mule.env"` to the value `"dev"`
+ Click `"Run"`


### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Complete all properties in one of the property files, for example in [mule.prod.properties] (../master/src/main/resources/mule.prod.properties) and run your app with the corresponding environment variable to use it. To follow the example, this will be `mule.env=prod`.
After this, to trigger the use case you just need to hit the local HTTP connector with the port you configured in your file. If this is, for instance, `9090` then you should hit: `http://localhost:9090/migratesalesitems` and this will output a summary report and send it in the e-mail.

## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](http://www.mulesoft.org/documentation/display/current/Hello+World+on+CloudHub) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.
Once your app is all set and started, supposing you choose as domain name `wdayfsalesitemsmigration` to trigger the use case you just need to hit `http://wdayfsalesitemsmigration.cloudhub.io/migratesalesitems` and report will be sent to the emails configured.

### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Mule Studio provides you with really easy way to deploy your Template directly to CloudHub, for the specific steps to do so please check this [link](http://www.mulesoft.org/documentation/display/current/Deploying+Mule+Applications#DeployingMuleApplications-DeploytoCloudHub)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
In order to use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:
### Application configuration
**Application configuration**

+ http.port `9090`

**Salesforce Connector configuration**

+ sfdc.username `bob.dylan@orga`
+ sfdc.password `DylanPassword123`
+ sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`
+ sfdc.integration.pricebook2Id `standard_pricebook2_id`

**Workday Connector configuration**

+ wday.user `wdayf_user`
+ wday.password `wdayf_password`
+ wday.tenant `wday_tenant`
+ wday.endpoint `https://{your Workday domain}/ccx/service/{your tenant name}/Revenue_Management/v23.1`

**SMTP Services configuration**

+ smtp.host `smtp.gmail.com`
+ smtp.port `587`
+ smtp.user `gmail_user`
+ smtp.password `gmail_password`

**Email Details**

+ mail.from `batch.migratesalesitems.migration%40mulesoft.com`
+ mail.to `username@gmail.com`
+ mail.subject `Batch Job Finished Report`

# API Calls <a name="apicalls"/>
Salesforce imposes limits on the number of API Calls that can be made. Therefore calculating this amount may be an important factor to consider. The Anypoint Template calls to the API can be calculated using the formula:

*** 4 * X  ***

Being ***X*** the number of Sales Items to be synchronized on each run.

For instance if 10 records are fetched from origin instance, then 40 api calls will be made (4 * 10).


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As mule applications are based on XML files, this page will be organized by describing all the XML that conform the Anypoint Template.
Of course more files will be found such as Test Classes and [Mule Application Files](http://www.mulesoft.org/documentation/display/current/Application+Format), but to keep it simple we will focus on the XMLs.

Here is a list of the main XML files you'll find in this application:

* [config.xml](#configxml)
* [endpoints.xml](#endpointsxml)
* [businessLogic.xml](#businesslogicxml)
* [errorHandling.xml](#errorhandlingxml)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Configuration Properties](http://www.mulesoft.org/documentation/display/current/Configuring+Properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor they can be found on the *Global Element* tab.


## businessLogic.xml<a name="businesslogicxml"/>
Functional aspect of the Template is implemented on this XML, directed by one flow responsible of executing the logic.
For the purpose of this particular Template the *mainFlow* just executes a [Batch Job](http://www.mulesoft.org/documentation/display/current/Batch+Processing) which handles all the migration logic.



## endpoints.xml<a name="endpointsxml"/>
This is the file where you will find the inbound side of your integration app.
This Template has only a [HTTP Listener Connector](http://www.mulesoft.org/documentation/display/current/HTTP+Listener+Connector) as the way to trigger the use case.

**HTTP Listener Connector** - Start Report Generation

+ `${http.port}` is set as a property to be defined either on a property file or in CloudHub environment variables.
+ The path configured by default is `migratesalesitems` and you are free to change for the one you prefer.
+ The host name for all endpoints in your CloudHub configuration should be defined as `0.0.0.0`. CloudHub will then route requests from your application domain URL to the endpoint.
+ The endpoint is configured as a *request-response* by placing it in the source section of the flow. The response will be the total of Sales items synced and filtered by the criteria specified.



## errorHandling.xml<a name="errorhandlingxml"/>
This is the right place to handle how your integration will react depending on the different exceptions.
This file holds a [Error Handling](http://www.mulesoft.org/documentation/display/current/Error+Handling) that is referenced by the main flow in the business logic.
