## Huawei Analytics Codelab
This repo contains many files. Please go through this Readme and follow all the steps in order to implement Huawei Analytics in your Android Application. In order to save time, I have attached herewith the quick start project. 

## **What You Will Learn**

In this codelab, you will learn how to:

-   Use Android Studio to efficiently integrate capabilities of Analytics Kit.
-   Correctly use APIs of Analytics Kit.

## **Downloading the Source Code**
Download the [.zip file](https://github.com/yasirtahir/huaweianalytics/raw/main/HuaweiAnalytics_StarterCode.rar) and open the project in the Android Studio. If you face any issues, please raise your concerns.

## Integration Preparations (Optional)
The following steps has been completed already for you but It's worth mentioning.

To integrate HUAWEI HMS Core services, you must complete the following preparations:

-   Create an app in AppGallery Connect.
-   Create an Android Studio project.
-   Add the app package name and save the configuration file.
-   Configure the Maven repository address and AppGallery Connect gradle plug-in.

For details, please refer to  [Integrating the HMS Core SDK](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/android-integrating-sdk-0000001050161876).


> **Note:** You need to register as a developer to complete the operations above.


1.  Sign in to  [AppGallery Connect](https://developer.huawei.com/consumer/en/service/josp/agc/index.html)  and select  **My projects**.
2.  Click the app for which you need to enable Analytics Kit.
3.  Select any menu under  **HUAWEI Analytics**  and click  **Enable Analytics service**. (You need the management permission to perform this operation.)

![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/1.png)


4. Turn on **Enable the API permission**. This option is selected by default. If you deselect this option, data will not be reported. If you do not enable the API permission here but the API permission is required later, go to **Project Setting** > **Manage APIs** and manually enable the API permission for Analytics Kit.

![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/2.png)

5. Download the configuration file.
![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/3.png)

6. In Android Studio, switch to the **Project** view and move the **agconnect-services.json** file to the **app** directory in your Android project.
![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/4.png)

## **Adding SDK Dependencies**

1. Add the Maven repository address and AppGallery Connect service dependencies to the **build.gradle** file of your project.

	a) Open the **build.gradle** file in the directory of your project.

	![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/5.png)

	b) **(Step 1.0.0)** Add the Maven repository address to **repositories**. 
	
	   repositories {
		   // ....
		   // TODO: 1.0.0 Add Maven repo here
		   maven {url 'https://developer.huawei.com/repo/'}
		}

		allprojects {
			repositories {
			   // ....
			   // TODO: 1.0.0 Add Maven repo here
			   maven {url 'https://developer.huawei.com/repo/'}
			}
		}

	c) **(Step 1.0.1)** Add the AppGallery Connect dependency to **dependencies**.

		dependencies {  
			// ....
			// TODO: 1.0.1 Add the AGConnect Dependency here  
			  classpath  'com.huawei.agconnect:agcp:1.3.1.300'  
		}

2. Add the Analytics Kit dependency to the **build.gradle** file in the **app** directory.

	a) Open the **build.gradle** file in the **app** directory.
	![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/6.png)


	b) **(Step 1.1.0)** Add the build dependency to **dependencies**.

		dependencies { 
			// TODO: 1.1.0 Add Huawei Analytics library dependency here  
			implementation 'com.huawei.hms:hianalytics:5.0.3.300'  
			implementation 'com.huawei.agconnect:agconnect-core:1.4.1.300'  
		}
		
	c) **(Step 1.1.1)** Add the AppGallery Connect plug-in dependency.
		
		apply plugin: 'com.huawei.agconnect'


3. Click **Sync now** or **Sync Project with Gradle Files** to build a project.

![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/7.png)

## **Modifying the BaseApplication Class**

1. **(Step 1.2)** Declare Analytics Kit by adding the following code inside the BaseApplication **onCreate** 

		// TODO: 1.2 Declare Analytics Kit  
		HiAnalyticsTools.enableLog()

2. **(Step 1.3) Most Important Step**
In this step, we will add the following code inside the companion object of BaseApplication.

		// TODO: 1.3 Define a var for Analytics Instance
		// Add the sendEvent method. This method is responsible to send events to our AGConnect Console.  
		
		lateinit var hiAnalyticsInstance: HiAnalyticsInstance  
  
		fun sendEvent(key: String, value: Bundle){  
		    if(HmsGmsUtil.booleanIsHmsAvailable(appContext)){  
		        hiAnalyticsInstance.onEvent(key, value)  
		        Log.i(TAG, "HMS Device -- Send Analytics to AGConnect")  
		    } else if(HmsGmsUtil.booleanIsGmsAvailable(appContext)){  
		        // Send event to Google Analytics  
				Log.i(TAG, "GMS phone -- Send Analytics to Firebase")  
		    }  
		} 

3. **(Step 1.4)** Initiate Analytics Kit by adding the following code inside the BaseApplication **onCreate** 

		// TODO: 1.4 Initiate the Analytics Instance 
		hiAnalyticsInstance = HiAnalytics.getInstance(this)



## **Modifying the MainActivity Class**
1. **(Step 1.5)** Modify the reportAnswerEvt fun by adding the following code inside. Please make sure you paste the exact same code:

		// TODO: 1.5 Report a customized Event  
		
		// Event Name: Answer  
		// Event Parameters:  
		//  -- question: String  
		//  -- answer:String  
		//  -- answerTime: String  
		// Initialize parameters.  
		
		val bundle = Bundle()  
		bundle.putString("question", txtQuestion.text.toString().trim { it <= ' ' })  
		bundle.putString("answer", answer)  
		
		val sdf = SimpleDateFormat("yyyy-MM-dd HH:mm:ss", Locale.ENGLISH)  
		bundle.putString("answerTime", sdf.format(Date()))  
  
		// Report a custom event.  
		BaseApplication.sendEvent( "WRITE_YOUR_NAME", bundle) // TODO: 1.5.1 Replace WRITE_YOUR_NAME with your real name

2. **(Step 1.5.1)** Replace **WRITE_YOUR_NAME** with your real name
		
		BaseApplication.sendEvent( "WRITE_YOUR_NAME", bundle) // TODO: 1.5.1 Replace WRITE_YOUR_NAME with your real name


3. **(Step 1.6)** Modify the postScorefun by adding the following code inside. Please make sure you paste the exact same code:

		// TODO: 1.6 Report score by using SUBMITSCORE Event  
		// Initiate Parameters  
  
		val bundle = Bundle()  
		bundle.putString("Name","WRITE_YOUR_NAME") // TODO: 1.6.1 Replace WRITE_YOUR_NAME with your real name
		bundle.putLong(HAParamType.SCORE, score.toLong())  
  
		// Report a predefined Event  
		BaseApplication.sendEvent(SUBMITSCORE, bundle)

4. **(Step 1.6.1)** Replace **WRITE_YOUR_NAME** with your real name
		
		bundle.putString("Name","WRITE_YOUR_NAME") // TODO: 1.6.1 Replace WRITE_YOUR_NAME with your real name


For details about APIs, please refer to [HUAWEI Analytics API Reference.](https://developer.huawei.com/consumer/en/doc/development/HMSCore-References/android-api-analytics-overview-0000001051067140)


## Testing Your App
Start your app in the simulator. Tap some buttons to trigger event reporting.

![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/10.jpg)


## Real Time Visualization 
Now, I'll show you how to monitor real-time visualization on the AGConnect Console.



## Checking Data

1.  Sign in to  [AppGallery Connect](https://developer.huawei.com/consumer/en/service/josp/agc/index.html)  and click  **My projects**.
2.  Click the app to manage.
3.  Go to  **HUAWEI Analytics > Overview > Real-time monitoring**.
4.  Check the data. For details, please refer to  [HUAWEI Analytics Kit Operation Guide](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/dashboard-0000001050985173).


	![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/8.png)


## Validate your Analytics configuration with App Debugging
Use App Debugging to validate your Analytics configuration for apps during development.

To enable Analytics Debug mode on an emulated Android device, execute the following command line:

	adb shell setprop debug.huawei.hms.analytics.app <package_name>

> In our case, write the package name com.yasir.huaweianalytics

This behavior persists until you explicitly disable Debug mode by executing the following command line:

	adb shell setprop debug.huawei.hms.analytics.app .none.

View the reported debugging data.

![enter image description here](https://github.com/yasirtahir/huaweianalytics/raw/main/images/9.png)


## **HmsGmsUtil Class**
In real world, we use HMS core only on Huawei devices. In order to maintain single code base, we use this Utility Class to make sure to initiate the respective SDK.

1.  **(Step 1.7)** Paste the following code inside HmsGmsUtil Class.

		// TODO: 1.7 Utility functions to check either the phone is HMS or GMS  
  
		fun booleanIsHmsAvailable(context: Context?): Boolean {  
			var booleanIsAvailable = false  
			if (null != context) {  
		        val result = HuaweiApiAvailability.getInstance().isHuaweiMobileServicesAvailable(context)  
		        booleanIsAvailable = ConnectionResult.SUCCESS == result  
		    }  
			Log.i(TAG, "isHmsAvailable: $booleanIsAvailable")  
		    return booleanIsAvailable  
		}  
  
		fun booleanIsGmsAvailable(context: Context?): Boolean {  
		    var booleanIsAvailable = false  
			if (null != context) {  
		        val result = GoogleApiAvailability.getInstance().isGooglePlayServicesAvailable(context)  
		        booleanIsAvailable = ConnectionResult.SUCCESS == result  
		    }  
		    Log.i(TAG, "isGmsAvailable: $booleanIsAvailable")  
		    return booleanIsAvailable  
		}



## **FAQs**

### **1. When should I perform initialization?**

Answer:  
Initialize Analytics Kit in the main thread by using the  **onCreate**  method of the first app activity. Otherwise, the processing of lifecycle events on the automatic collection page may be affected.

### **2. How does Analytics Kit identify users?**

Answer:  
Analytics Kit identifies users by their AAID.

### **3. When will the AAID be reset? How does Analytics Kit collect user statistics after AAID reset?**

Answer:  
The AAID will be reset in the following scenarios:

-   The user reinstalls the app.
-   The user restores the device to its factory settings.
-   The user clears the app data.
-   The app calls the  **clearCachedData()**  API.  
    A user is counted as a new user after the AAID is reset.

### **4. What permissions are required for using Analytics Kit?**

Answer:  
Analytics Kit requires the following permissions, which have been preset in Analytics Kit. You do not need to apply for permissions.

-   android.permission.INTERNET
-   android.permission.ACCESS_NETWORK_STATE
-   com.huawei.appmarket.service.commondata.permission.GET_COMMON_DATA

### **5. Why cannot I view the analysis result of the data reported during the test?**

Answer:

(1) When app debugging is disabled, you can only view some analysis results on the  **Real-time monitoring**  page. Analysis results on the  **Event analysis**  and  **Activity analysis**  are available only after the corresponding data is processed on the next day.

(2) When app debugging is enabled, the reported test data will not be analyzed and can only be viewed on the  **App debugging**  page.


## Congratulations


Well done! You have successfully completed this codelab and learned how to:

-   Integrate Huawei Analytics Kit.
-   Call APIs of Huawei Analytics Kit.

