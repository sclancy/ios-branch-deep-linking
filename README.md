[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/hyperium/hyper/master/LICENSE)

# Branch Metrics iOS SDK Reference

This is a repository of our open source iOS SDK, and the information presented here serves as a reference manual for our iOS SDK. See the table of contents below for a complete list of the content featured in this document.

___

## iOS Reference

1. External resources
  + [Full integration guide](https://dev.branch.io/getting-started/sdk-integration-guide/guide/ios/)
  + [Change log](https://github.com/BranchMetrics/ios-branch-deep-linking/blob/master/ChangeLog.md)
  + [Testing resources](https://dev.branch.io/getting-started/integration-testing/guide/ios/)
  + [Support portal](http://support.branch.io)
  + [Test app resources](#get-the-demo-app)

2. Getting started
  + [Library installation](#installation)
  + [Register for Branch key](#register-your-app)
  + [Add your Branch key](#add-your-branch-key-to-your-project)
  + [Register a URI scheme](#register-a-uri-scheme-direct-deep-linking-optional-but-recommended)
  + [Support Universal Links](#support-universal-linking-ios-9)

3. Branch general methods
  + [Get a Branch singleton](#get-a-singleton-branch-instance)
  + [Initialize Branch and register deep link router](#init-branch-session-and-deep-link-routing-function)
  + [Register view controller for auto deep linking](#register-a-deep-link-controller)
  + [Retrieve latest deep linking params](#retrieve-session-install-or-open-parameters)
  + [Retrieve the user's first deep linking params](#retrieve-install-install-only-parameters)
  + [Setting the user id for tracking influencers](#persistent-identities)
  + [Logging a user out](#logout)
  + [Tracking custom events](#register-custom-events)

4. Branch Universal Objects
  + [Instantiate a Branch Universal Object](#branch-universal-object)
  + [Register user actions on an object](#register-user-actions-on-an-object)
  + [List content on Spotlight](#list-content-on-spotlight)
  + [Configuring link properties](link-properties-parameters)
  + [Creating a short link referencing the object](#shortened-links)
  + [Triggering a share sheet to share a link](#uiactivityview-share-sheet)

5. Referral rewards methods
  + [Get reward balance](#get-reward-balance)
  + [Redeem rewards](#redeem-all-or-some-of-the-reward-balance-store-state)
  + [Get credit history](#get-credit-history)

___

## Get the Demo App

There's a full demo app embedded in this repository, but you can also check out our live demo: [Branch Monster Factory](https://itunes.apple.com/us/app/id917737838). We've [open sourced the Branchster's app](https://github.com/BranchMetrics/Branchster-iOS) as well if you're ready to dig in.

## Installation

**The compiled iOS SDK footprint is 180kb**

### Available in CocoaPods

Branch is available through [CocoaPods](http://cocoapods.org). To install it, simply add the following line to your Podfile:

```objc
pod "Branch"
```
### Carthage

To integrate Branch into your project using Carthage add the following to your `Cartfile`:

```ruby
github "BranchMetrics/ios-branch-deep-linking"
```

### Download the Raw Files

You can also install by downloading the raw files below.

* Download code from here:
[https://s3-us-west-1.amazonaws.com/branchhost/Branch-iOS-SDK.zip](https://s3-us-west-1.amazonaws.com/branchhost/Branch-iOS-SDK.zip)

* The testbed project:
[https://s3-us-west-1.amazonaws.com/branchhost/Branch-iOS-TestBed.zip](https://s3-us-west-1.amazonaws.com/branchhost/Branch-iOS-TestBed.zip)


### Register Your App

You can sign up for your own app id at [https://dashboard.branch.io](https://dashboard.branch.io).

### Add Your Branch Key to Your Project

After you register your app, your Branch Key can be retrieved on the [Settings](https://dashboard.branch.io/#/settings) page of the dashboard. Now you need to add it to YourProject-Info.plist (Info.plist for Swift).

1. In plist file, mouse hover "Information Property List," which is the root item under the Key column.
1. After about half a second, you will see a "+" sign appear. Click it.
1. In the newly added row, fill in "branch_key" for its key, leave type as String, and enter your app's Branch Key obtained in above steps in the value column.
1. Save the plist file.

![Branch Key Demo](docs/images/branch-key-plist.png)
If you want to add a key for both your live and test apps at the same time, you need change the type column to Dictionary, and add two entries inside:
1. For live app, use "live" (without double quotes) for key, String for type, and your live branch key for value.
2. For test app, use "test" (without double quotes) for key, String for type, and your test branch key for value.

![Branch Multi Key Demo](docs/images/branch-multi-key-plist.png)

### Register a URI Scheme Direct Deep Linking (Optional but Recommended)

You can register your app to respond to direct deep links (yourapp:// in a mobile browser) by adding a URI scheme in the YourProject-Info.plist file. Make sure to change **yourapp** to a unique string that represents your app name.

1. In Xcode, click on YourProject-Info.plist on the left.
1. Find URL Types and click the right arrow. (If it doesn't exist, right click anywhere and choose Add Row. Scroll down and choose URL Types).
1. Add "yourapp," where yourapp is a unique string for your app, as an item in URL Schemes as below:

![URL Scheme Demo](https://s3-us-west-1.amazonaws.com/branchhost/urlScheme.png)

Alternatively, you can add the URI scheme in your project's Info page.

1. In Xcode, click your project in the Navigator (on the left side).
1. Select the "Info" tab.
1. Expand the "URL Types" section at the bottom.
1. Click the "+" sign to add a new URI Scheme, as below:

![URL Scheme Demo](https://s3-us-west-1.amazonaws.com/branchhost/urlType.png)

### Support Universal Linking (iOS 9)

With iOS 9, Apple has added the ability to allow http links to directly open your app, rather than using the URI Schemes. This can be a pain to set up, as it involves a complicated process on your server. The good news is that Branch does this work for you with just two steps!

1. In Xcode, click on your project in the Navigator (on the left side).
1. Select the "Capabilities" tab.
1. Expand the "Associated Domains" tab.
1. Enable the setting (toggle the switch).
1. Add `applinks:xxxx.app.link` and `applinks:xxxx-alternate.app.link` to the list. Make sure `xxxx` matches the 4 character subdomain for your app (you can find it on the [dashboard here](https://dashboard.branch.io/#/settings/link)). If you use a custom subdomain, use that in place of the x's (eg `imgur.app.link` and `imgur-alternate.app.link`).
1. Add any additional custom domains you have (e.g. `applinks:vng.io`)

![Xcode Enable UL](docs/images/xcode-ul-enable.png)

1. On the Dashboard, navigate to your app's link settings page.
1. Check the "Enable Universal Links
1. Ensure that your Apple Team ID and app Bundle ID are correct (we try to auto-harvest these for you).
1. Be sure to save these settings updates.

![Dashboard Enable UL](docs/images/dashboard-ul-enable.png)

#### URI Scheme Considerations

The Branch SDK will pull the first URI Scheme from your list that is not one of `fb`, `db`, or `pin`. This value will be used one time to set the iOS URI Scheme under your Link Settings in the Branch Dashboard.

For additional help configuring the SDK, including step-by-step instructions, please see the [iOS Quickstart Guide](https://github.com/BranchMetrics/Branch-Integration-Guides/blob/master/ios-quickstart.md).

### Get a Singleton Branch Instance

All Branch methods require an instance of the main Branch object. Here's how you can get one. It's stored statically and is accessible from any class.

#### Methods

###### Objective-C

```objc
Branch *branch = [Branch getInstance];
```

###### Swift

```swift
let branch: Branch = Branch.getInstance()
```

###### Objective-C

```objc
#warning Remove for launch
Branch *branch = [Branch getTestInstance];
```

###### Swift

```swift
//TODO: Remove for launch
let branch: Branch = Branch.getTestInstance();
```

#### Parameters

**Branch key** (NSString *) _optional_
: If you don't store the Branch key in the plist file, you have the option of passing this key as an argument.


### Init Branch Session and Deep Link Routing Function

To deep link, Branch must initialize a session to check if the user originated from a link. This call will initialize a new session _every time the app opens_. 100% of the time the app opens, it will call the deep link handling block to inform you whether the user came from a link. If your app opens with keys in the params, you'll want to route the user depending on the data you passed in. Otherwise, send them to a generic screen.

#### Methods

###### Objective-C
```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    Branch *branch = [Branch getInstance];
    [branch initSessionWithLaunchOptions:launchOptions andRegisterDeepLinkHandler:^(NSDictionary *params, NSError *error) {
    	// route the user based on what's in params
    }];
    return YES;
}

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    if (![[Branch getInstance] handleDeepLink:url]) {
        // do other deep link routing for the Facebook SDK, Pinterest SDK, etc
    }
    return YES;
}

- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray *restorableObjects))restorationHandler {
    BOOL handledByBranch = [[Branch getInstance] continueUserActivity:userActivity];
    
    return handledByBranch;
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
    [[Branch getInstance] handlePushNotification:userInfo];
}
```

###### Swift
```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    let branch: Branch = Branch.getInstance()
    branch.initSessionWithLaunchOptions(launchOptions, andRegisterDeepLinkHandler: { params, error in
    	// route the user based on what's in params
    })
    return true
}

func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject) -> Bool {
    if (!Branch.getInstance().handleDeepLink(url)) {
        // do other deep link routing for the Facebook SDK, Pinterest SDK, etc
    }

    return true
}

func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]?) -> Void) -> Bool {
    // pass the url to the handle deep link call
    Branch.getInstance().continueUserActivity(userActivity);

    return true
}

func application(application: UIApplication, didReceiveRemoteNotification launchOptions: [NSObject: AnyObject]?) -> Void {
    Branch.getInstance().handlePushNotification(launchOptions)
}
```

#### Parameters

###### initSession

**launchOptions** (NSDictionary *) _required_
: These launch options are passed to Branch through didFinishLaunchingWithOptions and will notify us if the user originated from a URI call or not. If the app was opened from a URI like myapp://, we need to follow a special initialization routine.

**deepLinkHandler** ^(NSDictionary *params, NSError *error) _optional_
: This is the callback block that Branch will execute after a network call to determine where the user comes from. It is called 100% of the time the app opens up since Branch registers for lifecycle notifications.

- _NSDictionary *params_ : These params will contain any data associated with the Branch link that was clicked before the app session began. There are a few keys which are always present:
	- '+is_first_session' Denotes whether this is the first session (install) or any other session (open)
	- '+clicked_branch_link' Denotes whether or not the user clicked a Branch link that triggered this session
- _NSError *error_ : This error will be nil unless there is an error such as connectivity or otherwise. Check !error to confirm it was a valid link.
    - BNCServerProblemError There was an issue connecting to the Branch service
    - BNCBadRequestError The request was improperly formatted

Branch returns explicit parameters every time. Here is a list, and a description of what each represents.

* `~` denotes analytics
* `+` denotes information added by Branch
* (for the curious, `$` denotes reserved keywords used for controlling how the Branch service behaves)

| **Parameter** | **Meaning**
| --- | ---
| ~channel | The channel on which the link was shared, specified at link creation time
| ~feature | The feature, such as `invite` or `share`, specified at link creation time
| ~tags | Any tags, specified at link creation time
| ~campaign | The campaign the link is associated with, specified at link creation time
| ~stage | The stage, specified at link creation time
| ~creation_source | Where the link was created ('API', 'Dashboard', 'SDK', 'iOS SDK', 'Android SDK', or 'Web SDK')
| +match_guaranteed | True or false as to whether the match was made with 100% accuracy
| +referrer | The referrer for the link click, if a link was clicked
| +phone_number | The phone number of the user, if the user texted himself/herself the app
| +is_first_session | Denotes whether this is the first session (install) or any other session (open)
| +clicked_branch_link | Denotes whether or not the user clicked a Branch link that triggered this session
| +click_timestamp | Epoch timestamp of when the click occurred

**isReferrable** (BOOL) _optional_
: This boolean lets you control whether or not the user is eligible to be 'referred'. This is applicable for credits and influencer tracking. If isReferrable is set to NO | false, and the user clicks a link before entering the app, deep link parameters will appear, but that user will _not_ be considered referred. If isReferrable is set to YES | true, and the user clicks a link, deep link params will appear and the user _will_ be considered referred. Remove this argument to access the default, which only allows the user to be referred on a _fresh install_, but not on opens.

**automaticallyDisplayDeepLinkController** (BOOL) _optional_
: This boolean lets you control whether or not the Branch should attempt to launch Deep Linked controllers (based on those registered with `[branch registerDeepLinkController:forKey:]`). The default is NO | false.

###### handleDeepLink

**url** (NSString *) _required_
: This argument passes us the URI string so that we can parse the extra parameters. For example, 'myapp://open?link_click_id=12345'.

###### continueUserActivity

**userActivity** (NSUserActivity *) _required_
: This argument passes us the user activity so that we can parse the originating URL.

#### Returns

###### initSession

Nothing

###### handleDeepLink

**BOOL** handleDeepLink will return a boolean indicating whether Branch has handled the URI. If the URI call is 'myapp://open?link_click_id=12345', then handleDeepLink will return YES because the Branch click object is present. If just 'myapp://', handleDeepLink will return NO.

###### continueUserActivity

**BOOL** continueUserActivity will return a boolean indicating whether Branch has handled the Universal Link. If Universal Link is powered by Branch, then continueUserActivity will return YES because the Branch click object is present.

### Register a Deep Link Controller

Register a controller for Branch to show when specific keys are present in the Branch open / install dictionary. This is the mechanism to handle auto deep linking and should be called before `initSession`.

#### Methods

###### Objective-C

```objc
[[Branch getInstance] registerDeepLinkController:myController forKey:@"my-key"];
```

###### Swift

```swift
Branch.getInstance().registerDeepLinkController(myController forKey:"my-key")
```

#### Parameters

**controller** (UIViewController <BranchDeepLinkingController> *) _required_
: The controller to display when the key is present in the dictionary.

**key** (NSString *) _required_
: The key checked for in open / install dictionaries.

#### Returns

Nothing

### Retrieve session (install or open) parameters

These session parameters will be available at any point later on with this command. If no params, the dictionary will be empty. This refreshes with every new session (app installs AND app opens).

#### Methods

###### Objective-C

```objc
NSDictionary *sessionParams = [[Branch getInstance] getLatestReferringParams];
```

###### Swift

```swift
let sessionParams = Branch.getInstance().getLatestReferringParams()
```

#### Parameters

None

#### Returns

**NSDictionary *** When initSession returns a parameter set in the deep link callback, we store it in NSUserDefaults for the duration of the session in case you want to retrieve it later. Careful, once the app is minimized and the session ends, this will be cleared.

### Retrieve Install (Install Only) Parameters

If you ever want to access the original session params (the parameters passed in for the first install event only), you can use this line. This is useful if you only want to reward users who newly installed the app from a referral link. Note that these parameters can be updated when `setIdentity:` is called and identity merging occurs.

#### Methods

###### Objective-C

```objc
NSDictionary *installParams = [[Branch getInstance] getFirstReferringParams]; // previously getInstallReferringParams
```

###### Swift

```swift
let installParams = Branch.getInstance().getFirstReferringParams() // previously getInstallReferringParams
```

#### Parameters

None

### Persistent Identities

Often, you might have your own user IDs, or want referral and event data to persist across platforms or uninstall/reinstall. It's helpful if you know your users access your service from different devices. This where we introduce the concept of an 'identity'.

#### Methods

To identify a user, just call:

###### Objective-C

```objc
// previously identifyUser:
[[Branch getInstance] setIdentity:your user id];    // your user id should not exceed 127 characters
```

###### Swift

```swift
// previously identifyUser:
Branch.getInstance().setIdentity(your user id)  // your user id should not exceed 127 characters
```

#### Parameters

None

### Logout

If you provide a logout function in your app, be sure to clear the user when the logout completes. This will ensure that all the stored parameters get cleared and all events are properly attributed to the right identity.

**Warning**: This call will clear the promo credits and attribution on the device.

#### Methods

###### Objective-C

```objc
[[Branch getInstance] logout];  // previously clearUser
```

###### Swift

```swift
Branch.getInstance().logout()   // previously clearUser
```

#### Parameters

None

### Register Custom Events

#### Methods

###### Objective-C

```objc
[[Branch getInstance] userCompletedAction:@"your_custom_event"]; // your custom event name should not exceed 63 characters
```

###### Swift

```swift
Branch.getInstance().userCompletedAction("your_custom_event") // your custom event name should not exceed 63 characters
```

OR if you want to store some state with the event:

###### Objective-C

```objc
[[Branch getInstance] userCompletedAction:@"your_custom_event" withState:(NSDictionary *)appState]; // same 63 characters max limit
```

###### Swift

```swift
Branch.getInstance().userCompletedAction("your_custom_action", withState: [String: String]()) // same 63 characters max limit; replace [String: String]() with params dictionary
```

Some example events you might want to track:

```objc
@"complete_purchase"
@"wrote_message"
@"finished_level_ten"
```

####Parameters

None

## Branch Universal Object (for deep links, content analytics and indexing)

As more methods have evolved in iOS, we've found that it was increasingly hard to manage them all. We abstracted as many as we could into the concept of a Branch Universal Object. This is the object that is associated with the thing you want to share (content or user). You can set all the metadata associated with the object and then call action methods on it to get a link or index in Spotlight.

### Branch Universal Object best practices

Here are a set of best practices to ensure that your analytics are correct, and your content is ranking on Spotlight effectively.

1. Set the `canonicalIdentifier` to a unique, de-duped value across instances of the app
2. Ensure that the `title`, `contentDescription` and `imageUrl` properly represent the object
3. Initialize the Branch Universal Object and call `userCompletedAction` with the `BNCRegisterViewEvent` **on page load**
4. Call `showShareSheet` and `createShortLink` later in the life cycle, when the user takes an action that needs a link
5. Call the additional object events (purchase, share completed, etc) when the corresponding user action is taken

Practices to _avoid_:
1. Don't set the same `title`, `contentDescription` and `imageUrl` across all objects
2. Don't wait to initialize the object and register views until the user goes to share
3. Don't wait to initialize the object until you conveniently need a link
4. Don't create many objects at once and register views in a `for` loop.

### Branch Universal Object

#### Methods

###### Objective-C

```objc
#import "BranchUniversalObject.h"
```

```objc
BranchUniversalObject *branchUniversalObject = [[BranchUniversalObject alloc] initWithCanonicalIdentifier:@"item/12345"];
branchUniversalObject.title = @"My Content Title";
branchUniversalObject.contentDescription = @"My Content Description";
branchUniversalObject.imageUrl = @"https://example.com/mycontent-12345.png";
[branchUniversalObject addMetadataKey:@"property1" value:@"blue"];
[branchUniversalObject addMetadataKey:@"property2" value:@"red"];
```

###### Swift

```swift
let branchUniversalObject: BranchUniversalObject = BranchUniversalObject(canonicalIdentifier: "item/12345")
branchUniversalObject.title = "My Content Title"
branchUniversalObject.contentDescription = "My Content Description"
branchUniversalObject.imageUrl = "https://example.com/mycontent-12345.png"
branchUniversalObject.addMetadataKey("property1", value: "blue")
branchUniversalObject.addMetadataKey("property2", value: "red")
```

#### Parameters

**canonicalIdentifier**: This is the unique identifier for content that will help Branch dedupe across many instances of the same thing. If you have a website with pathing, feel free to use that. Or if you have database identifiers for entities, use those.

**title**: This is the name for the content and will automatically be used for the OG tags. It will insert $og_title into the data dictionary of any link created.

**contentDescription**: This is the description for the content and will automatically be used for the OG tags. It will insert $og_description into the data dictionary of any link created.

**imageUrl**: This is the image URL for the content and will automatically be used for the OG tags. It will insert $og_image_url into the data dictionary of any link created.

**metadata**: These are any extra parameters you'd like to associate with the Branch Universal Object. These will be made available to you after the user clicks the link and opens up the app. To add more keys/values, just use the method `addMetadataKey`.

**price**: The price of the item to be used in conjunction with the commerce related events below.

**currency**: The currency representing the price in [ISO 4217 currency code](http://en.wikipedia.org/wiki/ISO_4217). Default is USD.

**contentIndexMode**: Can be set to the ENUM of either `ContentIndexModePublic` or `ContentIndexModePrivate`. Public indicates that you'd like this content to be discovered by other apps. Currently, this is only used for Spotlight indexing but will be used by Branch in the future.

**expirationDate**: The date when the content will not longer be available or valid. Currently, this is only used for Spotlight indexing but will be used by Branch in the future.

#### Returns

None

### Register User Actions On An Object

We've added a series of custom events that you'll want to start tracking for rich analytics and targeting. Here's a list below with a sample snippet that calls the register view event.

| Key | Value
| --- | ---
| BNCRegisterViewEvent | User viewed the object
| BNCAddToWishlistEvent | User added the object to their wishlist
| BNCAddToCartEvent | User added object to cart
| BNCPurchaseInitiatedEvent | User started to check out
| BNCPurchasedEvent | User purchased the item
| BNCShareInitiatedEvent | User started to share the object
| BNCShareCompletedEvent | User completed a share

#### Methods

###### Objective-C

```objc
[branchUniversalObject userCompletedAction:BNCRegisterViewEvent];
```

###### Swift

```swift
branchUniversalObject.userCompletedAction(BNCRegisterViewEvent)
```

#### Parameters

None

#### Returns

None

### Shortened Links

Once you've created your `Branch Universal Object`, which is the reference to the content you're interested in, you can then get a link back to it with the mechanisms described below.

#### Encoding Note

One quick note about encoding. Since `NSJSONSerialization` supports a limited set of classes, we do some custom encoding to allow additional types. Current supported types include `NSDictionary`, `NSArray`, `NSURL`, `NSString`, `NSNumber`, `NSNull`, and `NSDate` (encoded as an ISO8601 string with timezone). If a parameter is of an unknown type, it will be ignored.

#### Methods

###### Objective-C

```objc
#import "BranchLinkProperties.h"
```

```objc
BranchLinkProperties *linkProperties = [[BranchLinkProperties alloc] init];
linkProperties.feature = @"sharing";
linkProperties.channel = @"facebook";
[linkProperties addControlParam:@"$desktop_url" withValue:@"http://example.com/home"];
[linkProperties addControlParam:@"$ios_url" withValue:@"http://example.com/ios"];
```

```objc
[branchUniversalObject getShortUrlWithLinkProperties:linkProperties andCallback:^(NSString *url, NSError *error) {
    if (!error) {
        NSLog(@"success getting url! %@", url);
    }
}];
```

###### Swift

```swift
let linkProperties: BranchLinkProperties = BranchLinkProperties()
linkProperties.feature = "sharing"
linkProperties.channel = "facebook"
linkProperties.addControlParam("$desktop_url", withValue: "http://example.com/home")
linkProperties.addControlParam("$ios_url", withValue: "http://example.com/ios")
```

```swift
branchUniversalObject.getShortUrl(with: linkProperties,  andCallback: { (url: String, error: Error?) in
    if error == nil {
        NSLog("got my Branch link to share: %@", url)
    }
})
```

#### Link Properties Parameters

**channel**: The channel for the link. Examples could be Facebook, Twitter, SMS, etc., depending on where it will be shared.

**feature**: The feature the generated link will be associated with. Eg. `sharing`.

**controlParams**: A dictionary to use while building up the Branch link. Here is where you specify custom behavior controls as described in the table below.

You can do custom redirection by inserting the following _optional keys in the dictionary_:

| Key | Value
| --- | ---
| "$fallback_url" | Where to send the user for all platforms when app is not installed. Note that Branch will forward all robots to this URL, overriding any OG tags entered in the link.
| "$desktop_url" | Where to send the user on a desktop or laptop. By default it is the Branch-hosted text-me service.
| "$android_url" | The replacement URL for the Play Store to send the user if they don't have the app. _Only necessary if you want a mobile web splash_.
| "$ios_url" | The replacement URL for the App Store to send the user if they don't have the app. _Only necessary if you want a mobile web splash_.
| "$ipad_url" | Same as above, but for iPad Store.
| "$fire_url" | Same as above, but for Amazon Fire Store.
| "$blackberry_url" | Same as above, but for Blackberry Store.
| "$windows_phone_url" | Same as above, but for Windows Store.
| "$after_click_url" | When a user returns to the browser after going to the app, take them to this URL. _iOS only; Android coming soon_.

You have the ability to control the direct deep linking of each link by inserting the following _optional keys in the dictionary_:

| Key | Value
| --- | ---
| "$deeplink_path" | The value of the deep link path that you'd like us to append to your URI. For example, you could specify "$deeplink_path": "radio/station/456" and we'll open the app with the URI "yourapp://radio/station/456?link_click_id=branch-identifier". This is primarily for supporting legacy deep linking infrastructure.
| "$always_deeplink" | true or false. (default is not to deep link first) This key can be specified to have our linking service force try to open the app, even if we're not sure the user has the app installed. If the app is not installed, we fall back to the respective app store or $platform_url key. By default, we only open the app if we've seen a user initiate a session in your app from a Branch link (has been cookied and deep linked by Branch).

**alias**: The alias for a link. Eg. `myapp.com/customalias`

**matchDuration**: The attribution window in seconds for clicks coming from this link.

**stage**: The stage used for the generated link, indicating what part of a funnel the user is in.

**tags**: An array of tag strings to be associated with the link.

#### Get Short Url Parameters

**linkProperties**: The link properties created above that describe the type of link you'd like

**callback**: The callback that is called with url on success, or an error if something went wrong. Note that we'll return a link 100% of the time. Either a short one if network was available or a long one if it was not.

### UIActivityView Share Sheet

UIActivityView is the standard way of allowing users to share content from your app. Once you've created your `Branch Universal Object`, which is the reference to the content you're interested in, you can then automatically share it _without having to create a link_ using the mechanism below..

**Sample UIActivityView Share Sheet**

![UIActivityView Share Sheet](https://dev.branch.io/img/pages/getting-started/branch-universal-object/ios_share_sheet.png)

The Branch iOS SDK includes a wrapper on the UIActivityViewController, that will generate a Branch short URL and automatically tag it with the channel the user selects (Facebook, Twitter, etc.).

#### Methods

###### Objective-C

```objc
#import "BranchLinkProperties.h"
```

```objc
BranchLinkProperties *linkProperties = [[BranchLinkProperties alloc] init];
linkProperties.feature = @"sharing";
[linkProperties addControlParam:@"$desktop_url" withValue:@"http://example.com/home"];
[linkProperties addControlParam:@"$ios_url" withValue:@"http://example.com/ios"];
```

```objc
[branchUniversalObject showShareSheetWithLinkProperties:linkProperties
                                           andShareText:@"Super amazing thing I want to share!"
                                     fromViewController:self 
                                             completion:^(NSString *activityType, BOOL completed){
    NSLog(@"finished presenting");
}];
```

###### Swift

```swift
let linkProperties: BranchLinkProperties = BranchLinkProperties()
linkProperties.feature = "sharing"
linkProperties.addControlParam("$desktop_url", withValue: "http://example.com/home")
linkProperties.addControlParam("$ios_url", withValue: "http://example.com/ios")
```

```swift
branchUniversalObject.showShareSheetWithLinkProperties(linkProperties, 
                                        andShareText: "Super amazing thing I want to share!",
                                        fromViewController: self,
                                        completion: { () -> Void in
    NSLog("done showing share sheet!")
})
```

#### Show Share Sheet Parameters

**linkProperties**: The feature the generated link will be associated with.

**andShareText**: A dictionary to use while building up the Branch link.

**fromViewController**: 

**completion**: 

#### Further Customization

The majority of share options only include one string of text, except email, which has a subject and a body. The share text will fill in the body and you can specify the email subject in the link properties as shown below.

```objc
[linkProperties addControlParam:@"$email_subject" withValue:@"This one weird trick."];
```

```swift
linkProperties.addControlParam("$email_subject", withValue: "Therapists hate him.")
```

#### Returns

None

### List Content On Spotlight

If you'd like to list your Branch Universal Object in Spotlight local and cloud index, this is the method you'll call. You'll want to register views every time the page loads as this contributes to your global ranking in search.

#### Methods

###### Objective-C

```objc
branchUniversalObject.automaticallyListOnSpotlight = YES;
[branchUniversalObject userCompletedAction:BNCRegisterViewEvent];
```

###### Swift

```swift
branchUniversalObject.automaticallyListOnSpotlight = true
branchUniversalObject.userCompletedAction(BNCRegisterViewEvent)
```

#### Parameters

**callback**: Will return the URL that was used to list the content in Spotlight if you'd like to store it for your own records.

#### Returns

None

## Referral System Rewarding Functionality

### Get Reward Balance

Reward balances change randomly on the backend when certain actions are taken (defined by your rules), so you'll need to make an asynchronous call to retrieve the balance. Here is the syntax:

#### Methods

###### Objective-C

```objc
[[Branch getInstance] loadRewardsWithCallback:^(BOOL changed, NSError *error) {
    // changed boolean will indicate if the balance changed from what is currently in memory

    // will return the balance of the current user's credits
    NSInteger credits = [[Branch getInstance] getCredits];
}];
```

###### Swift

```swift
Branch.getInstance().loadRewardsWithCallback { (changed: Bool, error: NSError!) -> Void in
    // changed boolean will indicate if the balance changed from what is currently in memory

    // will return the balance of the current user's credits
    let credits = Branch.getInstance().getCredits()
}
```

#### Parameters

**callback**: The callback that is called once the request has completed.

### Redeem All or Some of the Reward Balance (Store State)

Redeeming credits allows users to cash in the credits they've earned. Upon successful redemption, the user's balance will be updated reflecting the deduction.

#### Methods

###### Objective-C

```objc
// Save that the user has redeemed 5 credits
[[Branch getInstance] redeemRewards:5];
```

###### Swift

```swift
// Save that the user has redeemed 5 credits
Branch.getInstance().redeemRewards(5)
```

#### Parameters

**amount**: The number of credits being redeemed.

### Get Credit History

This call will retrieve the entire history of credits and redemptions from the individual user. To use this call, implement like so:

#### Methods

###### Objective-C

```objc
[[Branch getInstance] getCreditHistoryWithCallback:^(NSArray *history, NSError *error) {
    if (!error) {
        // process history
    }
}];
```

###### Swift

```swift
Branch.getInstance().getCreditHistoryWithCallback { (history: [AnyObject]!, error: NSError!) -> Void in
    if (error == nil) {
        // process history
    }
}
```

The response will return an array that has been parsed from the following JSON:

```json
[
    {
        "transaction": {
                           "date": "2014-10-14T01:54:40.425Z",
                           "id": "50388077461373184",
                           "bucket": "default",
                           "type": 0,
                           "amount": 5
                       },
        "event" : {
            "name": "event name",
            "metadata": { your event metadata if present }
        },
        "referrer": "12345678",
        "referree": null
    },
    {
        "transaction": {
                           "date": "2014-10-14T01:55:09.474Z",
                           "id": "50388199301710081",
                           "bucket": "default",
                           "type": 2,
                           "amount": -3
                       },
        "event" : {
            "name": "event name",
            "metadata": { your event metadata if present }
        },
        "referrer": null,
        "referree": "12345678"
    }
]
```
#### Parameters

**referrer**
: The id of the referring user for this credit transaction. Returns null if no referrer is involved. Note this id is the user id in a developer's own system that's previously passed to Branch's identify user API call.

**referree**
: The id of the user who was referred for this credit transaction. Returns null if no referree is involved. Note this id is the user id in a developer's own system that's previously passed to Branch's identify user API call.

**type**
: This is the type of credit transaction.

1. _0_ - A reward that was added automatically by the user completing an action or promo.
1. _1_ - A reward that was added manually.
2. _2_ - A redemption of credits that occurred through our API or SDKs.
3. _3_ - This is a very unique case where we will subtract credits automatically when we detect fraud.

