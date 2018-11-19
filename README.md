# Siri and SiriKit: an *Intents* topic
**Given by Alexsander Akers at Swift Alps on 29 Nov. 2018**

[SiriKit](https://developer.apple.com/documentation/sirikit) (Apple Documentation)

## Domains

### System Domains

* [Messaging](https://developer.apple.com/documentation/sirikit/messaging)
* [Lists and Notes](https://developer.apple.com/documentation/sirikit/lists_and_notes)
* [Workouts](https://developer.apple.com/documentation/sirikit/workouts)
* [Payments](https://developer.apple.com/documentation/sirikit/payments)
* [VoIP Calling](https://developer.apple.com/documentation/sirikit/voip_calling)
* [Visual Codes](https://developer.apple.com/documentation/sirikit/visual_codes)
* [Photos](https://developer.apple.com/documentation/sirikit/photos)
* [Ride Booking](https://developer.apple.com/documentation/sirikit/ride_booking)
* [Car Commands](https://developer.apple.com/documentation/sirikit/car_commands)*
* [CarPlay](https://developer.apple.com/documentation/sirikit/carplay)*
* [Restaurant Reservations](https://developer.apple.com/documentation/sirikit/restaurant_reservations)*

\* Requires private entitlement from Apple

### Custom Domains

* Defined in your app through _Intents.intentdefinition_ file
* Used to repeat a specific action that the user has already performed, or that the app thinks is relevant
* These intents support parameters, but are "baked into" the intent at the time the shortcut is added
  * For example, you cannot prompt for additional information through Siri, but you can continue a flow in your app

## A) Add Siri Shortcuts to your app with `NSUserActivity`

### What is `NSUserActivity`?

An `NSUserActivity` is a snapshot of what the user is doing in your app at a moment in time. These activities allow users to hand off their tasks between devices, to search for previously viewed content (e.g. map items), and to allow the system to suggest relevant data from user-initiated activities in your app.

### What you will be doing

[Donating Shortcuts](https://developer.apple.com/documentation/sirikit/donating_shortcuts) (Apple Documentation)

1. Assign NSUserActivity instances to your view controllers (or call the activity's [`becomeCurrent()`](https://developer.apple.com/documentation/foundation/nsuseractivity/1413665-becomecurrent) method).
1. Set [`isEligibleForPrediction`](https://developer.apple.com/documentation/foundation/nsuseractivity/2980674-iseligibleforprediction) on the activity to `true`.
1. Optionally set [`suggestedInvocationPhrase`](https://developer.apple.com/documentation/foundation/nsuseractivity/2976237-suggestedinvocationphrase) on the activity to show the user with a specific (localized!) as a prompt.
1. Implement the `UIApplicationDelegate` method [`application(_:continue:restorationHandler:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623072-application)
1. Use [`INShortcut`](https://developer.apple.com/documentation/sirikit/inshortcut) to create a shortcut. Then create a [`INUIAddVoiceShortcutViewController`](https://developer.apple.com/documentation/sirikit/inuiaddvoiceshortcutviewcontroller) view controller with that shortcut to allow the user to record a invocation phrase for this shortcut.

## B) Implement a system domain (see above) in your app

1. Create a new _Intents Extension_ target in your app.
1. Add the intent type to your extension's _Info.plist_ file under `NSExtension > IntentsSupported` or `NSExtension > IntentsRestrictedWhileLocked`
1. Modify the template _IntentHandler.swift_ class to initialize handler objects for `INIntent` types.
    * The [`INCreateTaskListIntentHandling`](https://developer.apple.com/documentation/sirikit/increatetasklistintenthandling) protocol matches the [`INCreateTaskListIntent`](https://developer.apple.com/documentation/sirikit/increatetasklistintent) intent so the string `INCreateTaskListIntent` would go in the _Info.plist_ array.
1. Create a `CreateTaskListIntentHandler` class that conforms to the `INCreateTaskListIntentHandling` protocol and implement the methods in that protocol to resolve details of the intent, confirm the response, and handle the intent of the user.
1. Repeat these steps (starting with 2) for each INIntent type you want to support

## C) Implement a custom domain in your app

1. Create a new _Intents Extension_ target in your app.
1. Create a _SiriKit Intent Definition File_-type file in your app's shared framework.
1. Include this file in your app, your app's shared framework, and your app's Intents extension. If your app has an IntentsUI extension, include this file there as well.
1. Make sure that the _Intent Classes_ option is only selected for your app's shared framework target(s).
1. Click on the `+` button to create a new custom intent or to customize an existing system intent.
1. Select an appropriate _Category_ and specify the _Title_ and _Description_ for your intent.
1. Follow the steps above to mark your extension as supporting your custom intent as well as to implement the custom intent handler.

## D) Implement the Lists domain in MVCTodo

[MVCTodo](https://github.com/a2/MVCTodo/tree/sirikit-workshop) is a sample project by Dave DeLong (modified to remove some of the initial shared framework overhead) that we can use as a basis on which to implement the Siri Lists domain. Follow the steps for Task B for each list- and task-related `INIntent` type found on the [Lists and Notes domain page](https://developer.apple.com/documentation/sirikit/lists_and_notes).

## Resources

- [SiriKit Documentation](https://developer.apple.com/documentation/sirikit)
- Apple Sample Code: [_Soup Chef_](https://developer.apple.com/documentation/sirikit/soup_chef_accelerating_app_interactions_with_shortcuts)
- Talk to me! I am also a resource. Raise your hand to get my attention, and I will try to help.
