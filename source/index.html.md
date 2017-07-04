---
title: WorldCoo Android SDK

language_tabs:
  - Java

toc_footers:
  - <a href='https://github.com/WorldCoo/docs-slate-sdk-android' target='_blank'>See on Github</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Android SDK overview

The Worldcoo Android SDK allows to this Operating System developers to interact with the <a href="http://docs.worldcoo.com/api/v3" target="_blank">Worldcoo Donation API</a> in a native way through a series of public methods and, also, to inject the <a href="http://docs.worldcoo.com/widget/v3" target="_blank">Worldcoo Widget</a> in an easier way than developing a custom integration.


# 1 - Installation

Follow these steps to include the Worldcoo SDK inside your app:

* Download the SDK from <a href="https://cdn.worldcoo.com/download/sdk/android/Worldcoo-1.0.aar">here</a>
* Copy the .aar library to a project lib folder (i.e. /libs/ folder).
* Add this line to the app build.gradle script on dependencies section:
_compile(name:'httprestlib-release', ext:'aar')_
* Add this line to the project build.gradle script on repositories section: flatDir{ dirs 'libs' }



# 2 - Authentication

```xml
<?xml version="1.0" encoding="utf-8"?>
 <manifest 
    xmlns:android="http://schemas.android.com/apk/res/android" 
    package="com.ateknea.widget.android.integrationtest">  
    
    <application 
        android:allowBackup="true" 
        android:icon="@mipmap/ic_launcher" 
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">  
        
        <meta-data android:name="com.worldcoo.sdk.api_key" android:value="@string/worldcoo_api_id"/> 
        <meta-data android:name="com.worldcoo.sdk.widget_id" android:value="@string/worldcoo_widget_id"/>  
        <activity android:name=".MainActivity"> 
            <intent-filter> 
                <action android:name="android.intent.action.MAIN" />  
                <categories android:name="android.intent.categories.LAUNCHER" /> 
            </intent-filter> 
        </activity>
    </application>  
</manifest>
```

In order to have access to the Worldcoo’s API and the widget generator a pair of keys are provided to the integrators. Those keys should be added by the developer into app’s code in a proper way to initialize the SDK successfully (keys API_KEY and WIDGET_ID needs to be declared without any modifications).

Since resources are declared using XML files, some values need to be declared using ASCII (‘-’ character should be &#8211)

Keys will be read from the AndroidManifest.xml as metadata and could be declared apart into string.xml file to hide values.

If everything has been declared in a proper way, the SDK method init(..) will load the keys automatically.


# 3 - Usage

## 3.1 - Common methods

These are the common public methods, both for API integration and Widget injection

### 3.1.1 - public static getInstance()

_Return the instance of the SDK._

**return**

* Worldcoo’s SDK instance


### 3.1.2 - init(Context appContext, boolean productionMode)

_Initializes the SDK_

**params**

* _appContext_ : Application context to load API keys and widget IDs from the manifest.
* _productionMode_ : True if production mode or false if using the REST API in sandbox mode

**return**

* True on success
* False if some errors occurs loading API keys


### 3.1.3 - setProductionMode(boolean productionMode)

_Sets mode of Donor API REST usage_

**params**

* _productionMode_ : True if production mode or false if using the REST API in sandbox mode.
 
 
### 3.1.4 - isProductionMode()
 
_Returns Donor API mode_

**return**

* True if production mode or false if using the REST API in sandbox mode



## 3.2 - API integration

These are the public methods provided by the SDK to interact with the Worldcoo Donation API:


### 3.2.1 - getAPIClient(Context context,  DonorListener listener)

_Returns a client to easily interact with the Donor web API_

**params**

* _context_ : Context to initialize the REST client
* _listener_ : Donorlistener instance

**return**

* DonorAPIClient instance


### 3.2.2 - getAvailableNGOs(int offset, int limit)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#get-available-ngos" target="_blank">Get Available NGOs</a> Worldcoo API endpoint.
</aside>

_Gets available NGOs_

**params**

* _offset_ : The amount of items to ignore. _(optional value, default=0)_
* _limit_ : The maximum number of items to return. _(optional value, default=20)_

**return**

* availableNGOs(ArrayList<NGO> ngos)

**onError(ErrorBody error, RequestType type)**

* _error_ : Error code, type and message
* _type_ : GET_NGOS


### 3.2.3 - getAvailableCampaigns(String ngo_id, int offset, int limit)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#get-available-campaigns" target="_blank">Get available campaigns</a> Worldcoo API endpoint.
</aside>

_Gets available campaign from an NGO._

**params**

* _ngo_id_ : NGO identifier
* _offset_ : The amount of items to ignore. _(optional value, default=0)_
* _limit_ : The maximum number of items to return. _(optional value, default=20)_

**return**

* availableCampaigns(ArrayList<Campaign> campaigns)

**onError(ErrorBody error, RequestType type)**

* _error_ : Error code, type and message
* _type_ : GET_CAMPAIGNS


### 3.2.4 - addDonation(Donation donation)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#add-donation" target="_blank">Add donation</a> Worldcoo API endpoint.
</aside>

_Adds a donation_

**params**

* _donation_ : Donation information

**onDonationAdded(DonationResponse donation)**

* _response_ : Donation information

**onError(ErrorBody error, RequestType type)**

* _error_ : Error code, type and message
* _type_ : ADD_DONATION


### 3.2.4 - cancelDonation(String donationId)

<aside class="notice">
    This method is a wrapper for the <a href="http://docs.worldcoo.com/api/v3/#cancel-donation" target="_blank">Cancel donation</a> Worldcoo API endpoint.
</aside>

_Cancels a donation_

**params**

* _donationId_ : Donation identifier

**onDonationCancelled(DonationResponse response)**

* _response_ : Donation information

**onError(ErrorBody error, RequestType type)**

* _error_ : Error code, type and message
* _type_ : CANCEL_DONATION


## 3.3 - Widget injection

### 3.3.1 - getInjector(Webview webview,  Widget widget)

_Returns a widget injector instance_

**params**

* _webview_ : Webview to inject the Worldcoo widget 
* _widget_ : Widget object with information about the donor and donation

**return**

* A WidgetInjector instance


### 3.3.2 - addInjectorListener(InjectorListener listener)

_Adds a listener to receive user interaction events from the widget injector_

**params**

* _InjectorListener_


### 3.3.3 - initialStep()

_Injects the widget first step on the given webview._


### 3.3.4 - confirmation()

_Injects the widget second step on the given webview._


## 3.4 - Example

```java
// SDK initialization
Worldcoo sdk = Worldcoo.getInstance()
sdk.init(context, productionMode)

// Donor API Client
sdk.getAPIClient().getAvailableNGOs(context,listener) // Being call from a context (fragmento or activity) that implements NativeListener interface

// This will be received from callback: 
onAvailableNGOs(List<NGO>)

// If there are errors
onError(ErrorBody, request)

// Same way to proceed with all Donor API methods

// Injector
// Construct and create the widget object
sdk.getWidgetInjector(Webview, Widget)
sdk.initialStep()
```