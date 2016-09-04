# AppVirality-Web-SDK
AppVirality Web SDK for Web Referral Program

AppVirality supports web Referral program through API.

Follow the below mentioned steps to run the referral program on Web, using AppVirality API. In order to run a referral program on both web & mobile, kindly use the same APPKEY for all platforms. Referral program on web purely run on user Email ID. To participate in the web referral program, the user must be registered with a valid Email ID and logged in. In case user is not logged in, please prompt him to signup and start referring friends.

<b>Base URL :</b> https://api.appvirality.com 

<H4>STEP 1 - Generating Referral Link:</H4>

When a user clicks on "Invite & Earn" button on web, please check if the user is already ‘Logged In’ or not. If not, prompt the user to LogIn to begin referring.

On Signup, call AppVirality API with user details where EmailId field is mandatory. In response to the call, basic information along with his referral link is provided, which is required for further communication with the API.


<b>Route</b>
<pre><code>POST /v1/registerwebuser

Content-Type application/json
</code></pre>

#####<b>Sample Input:</b>

```json
{
"apikey":"763502957c434314b613d42400ac7a7f",
"privatekey":"8a433343ee8241a10419212415496579",
"avClick":"6vzi9-e3f31",
"ReferrerCode":"e3f31",
"EmailId":"myemail@domain.com",
"AppUserName":"Myname",
"UserIdInstore":"HYD78890",
"ProfileImage":"http://mywebsite.com/images/profile.png",
"Phone":"000-000-0000",
"city":"Hyderabad",
"state":"Telangana",
"country":"India"
}
   
```
<b>NOTE - </b> Query string parameter "avClick" will be available if the user visited the website by clicking on the referral link. This click ID is used to identify the referrer for this user. 

#####<b>Input Parameters:</b>

* <b>apikey</b> —  API Key of the App, get this from Dashboard > App Details
* <b>privatekey</b> — Private key for API requests, get this from Dashboard > App Details > Change Settings > Advanced Settings
* <b>avClick</b> — AppVirality Click ID, this is query string parameter "avclk" value, this will be available if a user visits your website by clicking on the referral link. If the user is already registered with you don't send this parameter.
* <b>ReferrerCode</b> — Friend's Referral Code, user will be attributed to his friend when on providing his friend referral code.
* <b>EmailId</b> — User Email Address, with which he has signed up.
* <b>AppUserName</b> — Name of the User
* <b>UserIdInstore</b> — This is the ID with which you recognize the user uniquely in your store.
* <b>ProfileImage</b> — User profile picture URL, required to personalize the referral messages.
* <b>Phone</b> — User mobile Number
* <b>city</b> — User current City
* <b>state</b> — User current State
* <b>country</b> — User current Country

#####<b>Sample Response:</b>

```json
{
"userkey":"04b5c3779e3d4abbac53a50900d64e78",
"shareurl":"http:\/\/r.appvirality.com\/67bfc",
"shortcode":"67bfc",
"isExistingUser":true,
"hasReferrer":true,
"successcode":1
}
```
#####<b>Response Parameters:</b>

* <b>userkey</b> —  User key, it's required to make the API calls
* <b>shareurl</b> — Email of the Referrer
* <b>shortcode</b> — Name of the Referrer
* <b>isExistingUser</b> —  This will be True, if the user is already registered with Appvirality.
* <b>hasReferrer</b> — This will be "True" if the user has a referrer.
* <b>successcode</b> — status of the API call, 0 -  Fail & 1 - Success

<H4>STEP 2 - Using share URL to Invite</H4>

Use the share URL received in STEP-1 and append it to the share message, while inviting the friends.

<b>Example message:</b> "Register with xyz and earn $10 reward, use my referral link to get it: http://r.appvirality.com/CxRTY"

<H4>STEP 3 - Handle invite link click & display landing page</H4>

When a friend clicks on invite URL, AppVirality redirects the user to landing page (web URL provided in Dashboard > Edit campaign > Advanced URL configurations) with query string parameter "avclk". Store this click ID in cookie or at server side and pass the same to AppVirality during the "registerwebuser" API call i.e. STEP-1.

<H4>STEP 4 - Get Referrer Details </H4>

This is useful to show welcome message to the new user by showing his referrer name and profile picture. use "avclk" received in STEP-3 to complete this call.

<b>Route</b>
<pre><code>GET /v1/getreferrer/{avClick}

Content-Type application/json
</code></pre>

#####<b>Sample Input:</b>

v1/getreferrer/6vzi9-e3f31

#####<b>Input Parameters:</b>

* <b>avClick</b> —  AppVirality Click ID, this is query string parameter "avclk" value, this will be available if a user visits your website by clicking on referral link.

#####<b>Sample Response:</b>

```json
{
"name":"my Name",
"email":"mymail@domain.com",
"shortcode":"e3f31",
"profileimage":null,
"successcode":1
}
```
#####<b>Response Parameters:</b>

* <b>name</b> —  Name of the referrer
* <b>email</b> — Email of the Referrer
* <b>shortcode</b> — Referrer shortcode
* <b>profileimage</b> —  Profile picture of the Referrer
* <b>successcode</b> — status of the API call, 0 -  Fail & 1 - Success

<H4>STEP 5 - Register Conversion Event</H4>

After successful completion of "registerwebuser" API call, you are ready to send the conversion event.

Please visit the API documentation [STEP - 7](https://github.com/appvirality/AppVirality-API#7-register-conversion-event) on how to Register Conversion event.

<H4>STEP 6 - Get Referrer/Friend Balance</H4>

Use this call to check the referrer/friend available balance and under review credits. If the user participated in multiple campaigns, it returns multiple records with reward details for each campaign.

Please visit the API documentation [STEP - 6](https://github.com/appvirality/AppVirality-API#6-get-referrerfriend-balance) on how to get user balance.

<b>Live Sample :</b> [http://referral.appvirality.com/](http://referral.appvirality.com/)

<H4>STEP 7 - Register Email for Testing</H4>

Use this call to register an email as a test user. This will allow to register using the same email after reset.

<b>NOTE - </b> It is mandatory to click on "Add Test Device" button in the AppVirality Dashboard -> Test Devices page before you execute this call. It will give a window of 30 sec to register the email for testing. 

<b>Route</b>
<pre><code>POST /v1/webdebugregister

Content-Type application/json
</code></pre>

#####<b>Sample Input:</b>

```json
{
"apikey":"004d37de619e41777eb7a4f800715468",
"privatekey":"0ef86e6ee69017abb18aa50900554688",
"email":"test1998@gmail.com"
}
   
```

#####<b>Input Parameters:</b>

* <b>apikey</b> —  API Key of the App, get this from Dashboard > App Details
* <b>privatekey</b> — Private key for API requests, get this from Dashboard > App Details > Change Settings > Advanced Settings
* <b>email</b> — User Email Address, with which he has signed up.

#####<b>Sample Response:</b>

```json
{
  "success": true,
  "message": null
}
```
#####<b>Response Parameters:</b>

* <b>success</b> —  returns true when successfully registered
* <b>message</b> — message if any

<H4> # Submit Referral Code</H4>

It helps to apply referral code at a later stage and not in [registerwebuser API](https://github.com/appvirality/AppVirality-Web-SDK#step-1---generating-referral-link) call.

<b>Route</b>
<pre><code>POST /v1/setuserreferrer

Content-Type application/json
</code></pre>

#####<b>Sample Input:</b>

```json
{
"apikey":"7b808b1f969048db8463a4c60091c49b",
"privatekey":"09726325ed71465191eda4ec006d601b",
"userkey":"d2d2be61edc8453c8cf8ec3089c9036f",
"ReferrerCode":"jb2by",
"avClick":"3xoke-jb2by"
}   
```
<b>NOTE - </b> Query string parameter "avClick" will be available if the user visited the website by clicking on the referral link. This click ID is used to identify the referrer for this user. 

#####<b>Input Parameters:</b>

* <b>apikey</b> —  API Key of the App, get this from Dashboard > App Details
* <b>privatekey</b> — Private key for API requests, get this from Dashboard > App Details > Change Settings > Advanced Settings
* <b>userkey</b> —  User key, it's required to make the API calls
* <b>ReferrerCode</b> — Friend's Referral Code, user will be attributed to his friend when on providing his friend referral code.
* <b>avClick</b> — AppVirality Click ID, this is query string parameter "avclk" value, this will be available if a user visits your website by clicking on the referral link. If the user is already registered with you don't send this parameter.

#####<b>Sample Response:</b>

```json
{
"userkey":"04b5c3779e3d4abbac53a50900d64e78",
"shareurl":"http:\/\/r.appvirality.com\/67bfc",
"shortcode":"67bfc",
"isExistingUser":true,
"hasReferrer":true,
"successcode":1
}
```
#####<b>Response Parameters:</b>

* <b>userkey</b> —  User key, it's required to make the API calls
* <b>shareurl</b> — Email of the Referrer
* <b>shortcode</b> — Name of the Referrer
* <b>isExistingUser</b> —  This will be True, if the user is already registered with Appvirality.
* <b>hasReferrer</b> — This will be "True" if the user has a referrer.
* <b>successcode</b> — status of the API call, 0 -  Fail & 1 - Success


Help: To run complete test cycle follow the [Testing Guide](https://github.com/appvirality/appvirality-sdk-android/wiki/Testing-Your-Referral-Programs#lets-start-real-testing) from step 8 in the testing Guide.

<H3>Help on other topics:</H3>

Please have a look at our [Wiki page](https://github.com/appvirality/appvirality-sdk-android/wiki)

1. [Getting Started With AppVirality Android SDK Integration](https://github.com/appvirality/appvirality-sdk-android#integrating-appvirality-into-your-app)
2. [AppVirality API for referral Growth Hack](https://github.com/appvirality/AppVirality-API)
3. [How to Add Test Devices & Test Referral Program](https://github.com/appvirality/appvirality-sdk-android/wiki/Testing-Your-Referral-Programs)
4. [Using Custom Domain](https://github.com/appvirality/appvirality-sdk-android/wiki/Using-Custom-Domain)
5. [Init Callback | hasReferrer | getUserKey](https://github.com/appvirality/appvirality-sdk-android/wiki/AppVirality-Init-Callback)
6. [Using Referral Code & Referral Link](https://github.com/appvirality/appvirality-sdk-android/wiki/Referral-program-with-Referral-code-&-Link)
7. [Optional] [Exclude Premium Users](https://github.com/appvirality/appvirality-sdk-android/wiki/Exclude-Premium-Users)
8. [Optional] [Dynamic Share Message](https://github.com/appvirality/appvirality-sdk-android/wiki/Dynamic-Share-Message)
9. [Optional] [Remind Later Settings](https://github.com/appvirality/appvirality-sdk-android/wiki/Remind-Later-Settings)
10. [Optional] [To Whom and When to Show Growth Hack](https://github.com/appvirality/appvirality-sdk-android/wiki/To-Whom-and-When-to-Show-Growth-Hack)
11. [Optional] [Reward Notifications or Web-hook Configuration](https://github.com/appvirality/appvirality-sdk-android/wiki/Reward-Notifications)
12. [Optional] [Android Push Notification Configuration](https://github.com/appvirality/appvirality-sdk-android/wiki/Android-Push-Notification-Configuration)
13. [Optional] [Personalized Welcome Screen](https://github.com/appvirality/appvirality-sdk-android/wiki/Personalized-Welcome-Screen)
14. [Referral Program on Apps having Login and Logout](https://github.com/appvirality/appvirality-sdk-android/wiki/Referral-Program-on-Apps-having-LogOut-&-LogIn)
