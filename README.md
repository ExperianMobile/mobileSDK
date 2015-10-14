# mobileSDK
Experian Marketing Suite Mobile SDK
Overview:
For client mobile applications, a mobile SDK facilitates access to EMS Services.
The EMS SDK is provided for several popular device platforms:
iOS
Provided as a Framework and a Static Lib
Works directly with OBJ C
Interface files provided to support Swift
Android
Provided as a Zip Archive
Includes class files to include in the app project
Preparation for compiling the project:
iOS
Framework
Unzip archive libccmp_ios_framework.zip.

From the unZipped archive, add file:  "libcmp.framework" to "PROJECT-> General -> Embedded Binaries"

In "Build Phases->Link Binary with Libraries", add libz.dylib and  from the unZipped file add libcurl.a and libccmp.a


Include the following header files to your project in your main AppDelegate header file or any other file:
#import <libccmp/libccmp.h>
Insert the following code to test your implementation:
Implementation
void testcallback(const char *data){
}
 
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    CustomerConfig configApp;
    configApp.appId="ea51f919-8fe7-488a-b8d6-3010a8aa840f";
    configApp.custId="100";
    configApp.token="Foo";
    configApp.baseUrl="http://8.19.129.146";
    configApp.registrationId="96611A71-B453-40C6-87F0-E37A7060CF10";
 
    CustomerConfig * pconfigApp=&configApp;
    SaveRegistration(pconfigApp, testcallback);
    return YES;
}
Static Lib (Optional Advanced Setup)
Unzip archive libccmp_ios_static.zip and copy the folder to your project root folder.
Archive Contents: 

In "Build Phases->Link Binary with Libraries", add libz.dylib

In "Build Settings->Search Paths, add the following:
Header Search Paths:
$(SRCROOT)/include  (This is the folder that came from the Zip archive)


Library Search Paths:
$(SRCROOT)/include
$(SRCROOT)/include/ccmp


Add  "x86_64" to thr option "Standard achictectures" and "Valid Archictectures"

Add "-lcurl" into the "Other Linker Flags" option

Include the following header files to your project in your main AppDelegate header file or any other file:
#include <ccmp/curl.h>
#include <ccmp/libccmp.h>
Insert the following code to test your implementation:
Implementation
void testcallback(const char *data){
}
 
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    CustomerConfig configApp;
    configApp.appId="ea51f919-8fe7-488a-b8d6-3010a8aa840f";
    configApp.custId="100";
    configApp.token="Foo";
    configApp.baseUrl="http://8.19.129.146";
    configApp.registrationId="96611A71-B453-40C6-87F0-E37A7060CF10";
 
    CustomerConfig * pconfigApp=&configApp;
    SaveRegistration(pconfigApp, testcallback);
    return YES;
}

Android
Ensure you have the Android Native Development Kit (NDK) installed https://developer.android.com/ndk/downloads/index.html#download
Copy the folder experian/ccmp into your java/com folder of your project structure in android studio (all the needed classes based on their namespace/package name structure)
Contents of archive:

Copy the content of the /jniLibs to your /jniLibs folder inside your android project structure.
Class package Structure

Library Contents:

Make sure the following path and components are setup on your system (JDK, NDK) and their paths are setup both within your environment variables and Gradles Script -> local.properties section:

At this point, the project can be built.
 
Inside the SDK: Object Model and Object creation in swift, java, and objective-c
Objects:
Methods:
Method	Parameters
void SaveRegistration	
Object of type CustomerConfig
void SaveRegistration	
Object of type CustomerConfig
void GetRegistration	
Object of type CustomerConfig
void SaveRegistrationWithTokenInBody	
Object of type CustomerConfig
void UpdateRegistration	
Object of type CustomerConfig
void UpdateRegistrationWithTokenInBody	
Object of type CustomerConfig
void GetRegistrationTimeStamp	
Object of type CustomerConfig
void GetToken	
Object of type CustomerConfig
void GetApplication	
Object of type CustomerConfig
void SaveErrorRegistration	Object of type CustomerConfig and String type parameter for error textual representation
 	 
OAUTH
void GetAuthorizationToken	Object of type CustomerConfig, OauthConfig Object
void AddRecipients
Object of type CustomerConfig, OauthConfig Object , free form json string
void GetRecordsByRecordId
Object of type CustomerConfig, OauthConfig Object, SearchQuery Object
void GetRecordsByTableName
Object of type CustomerConfig, OauthConfig Object, SearchQuery Object
void SearchRecordsByOperation
Object of type CustomerConfig, OauthConfig Object, SearchQuery Object
void SearchRecordByTableName
Object of type CustomerConfig, OauthConfig Object, SearchQuery Object
3.3) Code Example and Callback
How to use libccmp in objective-c (code):

Implementation
//include the right header
#include <ccmp/libccmp.h>
//configuration object
CustomerConfig configApp;
configApp.appId="1af062d1-4de8-463c-91bf-fd3fda3badc9";
configApp.custId="100";
configApp.token="Foo";
configApp.baseUrl="http://10.24.19.123";
configApp.registrationId="96611A71-B453-40C6-87F0-E37A7060CF10";
 
CustomerConfig * pconfigApp=&configApp;
SaveRegistration(pconfigApp, testcallback);
 
//define callback function
void testcallback(const char *data){
}
 
 
How to use libccmp in swift (code):  

Example 1. Registration
Implementation
//declare and instantiate the object push of Type Push
 
var push:Push?
//define all the variables required
var _ccmpCustId = ""
var _ccmpIp = ""
var _ccmpAppId = ""
var _apnToken = ""
var _apnTokenString = ""
var _prid = ""
var ccmpDeviceToken = ""
var ccmpAuthToken: NSString!
 
//configuration object
var config = CustomerConfig();
 
//example of usage – load configuration variables
func loadViewClassVariables() {
        _ccmpCustId = viewController!.ccmpCustId.text
        _ccmpIp = viewController!.ccmpIP.text
        _ccmpAppId = viewController!.ccmpAppId.text
        _apnTokenString = viewController!.apnTokenString
        _prid = viewController!.prid
 
        config.hostname =  ("\(_httpProtocol())://\(_ccmpIp)" as NSString).UTF8String
        config.custId = (_ccmpCustId as NSString).UTF8String
        config.appId = (_ccmpAppId as NSString).UTF8String
        config.baseUrl=("\(_httpProtocol())://\(_ccmpIp)" as NSString).UTF8String
        config.token=(_apnTokenString as NSString).UTF8String
        config.registrationId = (_prid as NSString).UTF8String
    }
//example of calling a methods againt the SDK
// GET
    func sdkGetPrid () {
        loadViewClassVariables()
        self.push = Push()
 
        //assigning the callback function
        self.push?.block = sdkGetPrid_callback
        self.push?.getRegistration(&config);
    }
//create a callback
   func sdkGetPrid_callback(error: NSError!, message: String!, ResponseCode: String! ) -> String!{
        println("response = \(message)")
}
 
 
Example 2. Push Notification Processing.

A received Push Notification Message will run the delegate  didReceiveRemoteNotification which is located in your appDelegate.swift file. Below is a generic example for how to handle a notification message.  EMS provides two additional fields in the notification:
"ems_open":"url" - This is similar to a tracking pixel, used to indicate that the Push Notification was viewed.
"ems_url":"url" - This is a 'deep link' whereby the app can use to get additional content associated with the campaign.

Push Notification Processing
func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject]) {
    let defaults: NSUserDefaults = NSUserDefaults.standardUserDefaults()
    let notificationStyle = defaults.integerForKey("notification_style")
    let ccmpIp = defaults.valueForKey("ccmp_ip") as? String
     
    print("Received: \(userInfo)")
     
    print("All of userInfo:\n\( userInfo)\n")
     
    //Parsing userinfo:
    if let info = userInfo["aps"] as? Dictionary<String, AnyObject>
    {
        var alertMsg: String
        switch(notificationStyle) {
        case 0:
            alertMsg = info["alert"] as! String
            break;
        case 1:
            alertMsg = "\(userInfo)"
            break;
        default:
            alertMsg = "Internal App Error, unknown notification style"
            break;
        }
         
        var alert: UIAlertView!
        alert = UIAlertView(title: "", message: alertMsg, delegate: nil, cancelButtonTitle: "OK")
        alert.show()
    }
     
    if let openInfo = userInfo["ems_open"] as? String
    {
        let url: NSURL = NSURL(string: openInfo.stringByReplacingOccurrencesOfString("localhost", withString: ccmpIp!))!
        let request = NSMutableURLRequest(URL: url)
        request.HTTPMethod = "GET"
         
        let task = NSURLSession.sharedSession().dataTaskWithRequest (request, completionHandler: DidSendOpen)
        task.resume()
    }
     
    if let urlInfo = userInfo["ems_url"] as? String
    {
        if let url = NSURL(string:urlInfo) {
            UIApplication.sharedApplication().openURL(url)
        }
    }
}

How to use libccmp in Java (code):

Implementation
//import the right classes
import com.experian.ccmp.CustomerConfig;
import com.experian.ccmp.ICallBack;
import com.experian.ccmp.Push;
 
//setup textboxes
EditText etCustId = null;
EditText etAppId = null;
EditText etDeviceToken = null;
EditText etIp = null;
EditText etKey = null;
EditText etPass = null;
EditText etReturnValue =null;
EditText etPushRegId =null;
 
//
private void SetConfigParams(){
        custId = etCustId.getText().toString();
        appId = etAppId.getText().toString();
        deviceToken =etDeviceToken.getText().toString();
        registrationId = etPushRegId.getText().toString();
        //initialize configuration object
        config= new CustomerConfig();
        config.appId= appId;
        config.custId= custId;
        config.token= deviceToken;
        config.hostname="http://" + etIp.getText().toString();
        config.baseUrl="http://" + etIp.getText().toString();
        config.registrationId = registrationId;
    }
 
private OnClickListener registrationListener= new OnClickListener() {
@Override
public void onClick(View v) {
    SetConfigParams();
        try {
                Log.d(TAG, "start registration to ccmp");
                //defining a callback
                ICallBack myCallBack= new ICallBack() {
                public void mcallback(byte[] message_byte){
                    String message = new
                    String(message_byte);
                    System.out.println("debug info: "+message);
                    etReturnValue.setText("");
                    etReturnValue.setText(message);
 
 
                    try {
                        JSONObject jsonObject = new JSONObject(message);
                        String pushRegId =(String) jsonObject.get("Push_Registration_Id");
                        etPushRegId.setText(pushRegId);
                    }
                    catch(JSONException e){
                        System.out.println("Exception when creation of jsonObject : "+
                        e.toString());
                    }
              }
 
 
        };
        Push objectPush = newPush(myCallBack);
        //this is should return nothing              
        objectPush.SaveRegistration(config);
 
 
        Log.d(TAG, "end registration to ccmp");
    } 
    catch (Exception e) {
        Log.d(TAG, "Error registration to ccmp", e);
        lastException = e.toString();
 
        //UPDATE UI param
        runOnUiThread(new Runnable() {
        @Override
        public void run() {
         etReturnValue.setText("Error registration to ccmp :" + lastException);
        }
    });
}
System.gc();
}

Libccmp Data JSON format
Implementation
//Methods in Libccmp return the following json structure
 
{"request":
    {
        "responseCode":"200", 
        "isError":false,
        "erroMessage":"",
        "data":
                {
                    "content":{/*NESTED CONTENT BASED REQUEST CALL*/}
                }
    }
}
 
 
// The Nested content can be type of well formatted json response coming from the server.
// ResponseCode:  it is the HTTP response code call
//isError: it is a bool value that return true whenever the response code is different of 200 or 304\
//errorMessage:  it is most the time empty. All major error message are display in the data.content object of the JSON.
