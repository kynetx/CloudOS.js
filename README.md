# CloudOS.js

CloudOS.js is a library that interacts with CloudOS Personal Clouds. It uses the Sky Cloud API to get data and raises events to save that data.


## Registering your Cloud Fabric App
1. Log into your [SquareTag](https://squaretag.com/app.html). If you have already become a developer and have the KDK, go ahead and skip to step 4.
2. Become a developer. Go to your [settings](https://squaretag.com/app.html#!/app/a169x695/show). Choose myCloud as the application and change your Cloud Type to `cloudTypeDeveloper`.
3. Install the KDK. Open up the [appStore](https://squaretag.com/app.html#!/app/a169x669/show) and install the KDK.
4. Run the KDK. Open up [myApps](https://squaretag.com/app.html#!/app/a169x670/show) and click on the KDK icon.
5. Create an app. Open the app menu and select `Create App`. Enter your application's details and save the app. The required fields are similar to other OAuth protected API's.
6. View your new app. In order to get the developer token, you'll need to click on your newly created app.
7. Configure the CloudOS object by putting the following definitions in the JavaScript for your app after loading the CloudOS.js library:
```
   	CloudOS.appKey = "App_key_from_the_KDK";
	CloudOS.callbackURL = "URL_to_the_page_in_your_app_that_exchanges_OAuth_code_for_token";
```

## CloudOS.js Methods

### `CloudOS.getOAuthURL(fragment)`
This returns a link to display to your user. This link, when clicked on, will take the user to an OAuth authorization page.

It takes one optional parameter, the fragment. With the fragment, you can return back to the current page you are on. The fragment gets appended to the `callbackURL` defined in CloudOS.js.


### `CloudOS.getOAuthAccessToken(code, callback)`
This function fetches the OAuth access token from the Personal Cloud. The code will be provided as a parameter in the OAuth callback URL.

An optional parameter, callback, can be provided. The callback should take one parameter which is the JSON response in the following format:

```
{
 "access_token":"OAUTH_ACCESS_TOKEN",
 "OAUTH_ECI":"USER_ECI"
}
```

### `CloudOS.retrieveOAuthCode(query_string)`
Looks in the string passed in as `query_string` for a parameter named `code` and returns the value. This function is a helper function for retrieving the `code` passed into `CloudOS.getOAuthAccessToken()`.  For example:
```
CloudOS.getOAuthAccessToken(
     CloudOS.retrieveOAuthCode(window.location.search.substring(1)),
     function(json) {
	    // callback code (post authentication) here
     });
```

### `CloudOS.retrieveSession()`
Gets the stored ECI out of the session cookie. This enables logins to persist between visits. It returns nothing.

### `CloudOS.removeSession()`
Deletes the cookie. This allows logging out to stop login persistence.

### `CloudOS.authenticatedSession()`
Returns the whether or not there is a valid CloudOS ECI. Useful for determining if the user is logged in or not.

### `CloudOS.createChannel(callback)`
Creates a channel and passes the returned object to your callback function. The object is in the following format:

```
{
  "status":true,
  "token":"YOURNEWTOKENHERE"
}
```

`status` is a boolean which states whether it worked or not. True means that it worked and false means it failed.

### `CloudOS.destroyChannel(token, callback)`
Deletes a channel and calls your callback function after finishing.

### `CloudOS.getMyProfile(callback)`
Fetches the currently logged in user's information. It then calls your callback function with the profile information. The returned object looks like so:

```
{
  "myProfileName":"USERSNAME",
  "myProfilePhoto":"URLTOPROFILEPHOTO",
  "myProfileNotes":"PROFILENOTES",
  "myProfileDescription":"PROFILEDESCRIPTION",
  "myProfilePhone":"USERSPHONENUMBER",
  "myProfileEmail":"USERSEMAIL"
}
```

The format and fields in the profile may change.

### `CloudOS.updateMyProfile(updatedProfile, callback)`
Changes the profile and calls your callback function when finished. The updatedProfile variable should have the same options as what is returned from `CloudOS.getMyProfile`.

### `CloudOS.getFriendProfile(token, callback)`
Gets a friend's profile. In order to get a person's information, there must be a subscription set up
between the two clouds. The returned object is the same as from `CloudOS.getMyProfile`.

### `CloudOS.PDSAdd(namespace, key, value, callback)`
Stores arbitrary data in the user's personal data store. The namespace and key are path identifiers and the value is what you wish to store in the PDS. Value can be an integer, float, boolean, array, hash, or string. Your callback function gets called once the addition is completed.

Imagine you have a variable named `PDS` that is a hash. The namespace and key are used as follows:
`PDS[namespace][key]`. The namespace and key can be any valid hash key identifiers.

### `CloudOS.PDSDelete(namespace, key, callback)`
Deletes an item from the PDS identified whose path is identified by the namespace and key. See `CloudOS.PDSAdd` for more PDS related information.


### `CloudOS.PDSUpdate()`
Not yet implemented.

### `CloudOS.PDSList(namespace, callback)`
Lists all of the variables stored in the PDS with the path of namespace. This includes all of the keys added using `CloudOS.PDSAdd`. Your callback should take one option that is the returned data from the PDS. The format is as follows:

```
{
  "key1": "value1",
  "key2": [
    10,
    20
  ]
}
```

### `CloudOS.sendEmail(name, email, subject, body, callback)`
Sends an email to `email` with the other attributes. Calls your callback function once the email has been sent.

### `CloudOS.sendNotification(applicationName, subject, body, priority, token, callback)`
Sends a notification to the user identified by `token`. Similar to `CloudOS.sendEmail` but sends a notification to the user based on their notification settings.

### `CloudOS.subscribe(namespace, name, relationship, token, subscriptionAttributes, callback)`
Creates a subscription between the current user and the user identified by `token`. `namespace`, `name` and `relationship` are all strings identifying the type of subscription. `subscriptionAttributes' is a hash containing data you wish to include with the subscription.

### `CloudOS.subscriptionList(callParameters, callback)`
Returns all subscriptions. I have no clue what `callParameters` is, though. None what-so-ever. `callback` is the callback function we call with the subscriptions. It should accept one parameter that is the subscriptions. It looks like:

TODO: fill this part out
```
{
}
```

## Raising Events
You can raise general events using the CloudOS.js library. This makes it possible to create any custom behavior. You can also create KRL rulesets which listen for these events or use existing events and rulesets.

### `CloudOS.raiseEvent(domain, type, attributes, parameters, callback)`
Raises an event using an HTTP POST. Again, I do not know the difference between attributes and parameters.

### `CloudOS.skyCloud(module, functionName, parameters, callback)`
Calls the sky cloud API. This allows you to get data from module functions.
