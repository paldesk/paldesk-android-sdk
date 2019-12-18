
# Paldesk
## Installing

Paldesk SDK requires device with minimum Android API version 16.

Add Paldesk repository in your current repositories in project's root build.gradle:

    allprojects {
    	repositories {
			...
    		maven { url "https://s3-eu-west-1.amazonaws.com/paldesk-maven/release/" }
			...
    }

Add dependency in your applications build.gradle:

    dependencies {
        implementation 'com.paldesk.android:paldesk-sdk:1.0.5'
    }
    
Add these permissions to your Manifest file:

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>


## Usage

Initialize Paldesk with your Api Key in your Application class:
```java
@Override
public void onCreate() {
    super.onCreate();
    PaldeskSDK.init(this, "your api key");
    //...
}
```    
If you wish to enable logging, just add "loggingEnabled" parameter to initialize method:
```java
PaldeskSDK.init(this, "your api key", true);
```
## Starting conversation
To start conversation from your app, use:

```java
PaldeskSDK.startConversation(this)
```
## Creating client

Depending on your Client authentification type settings from Webapp -> Administration -> iOS SDK (https://www.paldesk.com/), you can choose three options:

* No data required - client will appear as “visitor” - you are free to start conversation without setting information about your client
* You provide information about client through code - you can create client when your client becomes available to you with following method:
        
```java
ClientParams clientParams = new ClientParams.Builder("externalId")
    .firstName("First Name")
    .lastName("Last Name")
    .email("email")
    .build();
                
PaldeskSDK.createClient(clientParams);
```        

  ExternalId field is mandatory and other fields are optional. ExternalId should be something unique from your side, preferably your client's id (or even email).
  If client information is not provided, your client will appear as “visitor". You can explicitly create "visitor" by calling:
```java
PaldeskSDK.createAnonymousClient()
``` 
* Client provides his information through form - your usr will have to provide his information in registration form which will be shown on first start (prior to starting conversation).



## Clearing client

To clear (logout) current client, use:
```java
PaldeskSDK.clear()
```
  
Important: If you call create client method multiple times, it will matter only first time. If you wish to create different client, please call clear method first and then you are able to create client again.
