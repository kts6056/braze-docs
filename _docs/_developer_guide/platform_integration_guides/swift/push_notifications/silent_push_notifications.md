---
nav_title: Silent Push Notifications
article_title: Silent Push Notifications for iOS
platform: Swift
page_order: 4
description: "This article covers how to implement silent iOS push notifications for the Swift SDK."
channel:
  - push

---

# Silent push notifications

> Push notifications allow you to send out notifications from your app when important events occur. 

You might send a push notification when you have an important alert for a user. Push notifications can also be silent, containing no alert message or sound, being used only to update your app's interface or trigger background work. Silent push notifications can wake your app from a "Suspended" or "Not Running" state to update content or run certain tasks without notifying your users.

Braze has several features which rely on silent push notifications:

|Feature|User Experience|
|---|---|
|[Uninstall Tracking][6] | User receives a silent, nightly uninstall tracking push.|
|[Geofences][9] | Silent syncing of geofences from server to device.|
{: .reset-td-br-1 .reset-td-br-2}

## Setting up silent push notifications

To use silent push notifications to trigger background work, you must configure your app to receive notifications even when it is in the background. To do this, add the Background Modes capability using the **Signing & Capabilities** pane to the main app target in Xcode. Select the **Remote notifications** checkbox.

![Xcode showing the "remote notifications" mode checkbox under "capabilities".][3]

Even with the remote notifications background mode enabled, the system will not launch your app into the background if the user has force-quit the application. The user must explicitly launch the application or reboot the device before the app can be automatically launched into the background by the system.

For more information, refer to [pushing background updates][4] and the `application:didReceiveRemoteNotification:fetchCompletionHandler:` [documentation][5].

## Sending silent push notifications

To send a silent push notification, set the `content-available` flag to `1` in a push notification payload. 

{% alert note %}
What Apple calls a remote notification is just a normal push notification with the `content-available` flag set.
{% endalert %}

The `content-available` flag can be set in the Braze dashboard as well as within our [Apple push object]({{site.baseurl}}/api/objects_filters/messaging/apple_object/) in the [messaging API][1].

![The Braze dashboard showing the "content-available" checkbox found in the "settings" tab of the push composer.][2]

When sending a silent push notification, you might also want to include some data in the notification payload, so your application can reference the event. This could save you a few networking requests and increase the responsiveness of your app.

## iOS silent notifications limitations

The iOS operating system may gate notifications for some features. Note that if you are experiencing difficulties with these features, the iOS's silent notifications gate might be the cause.

Refer to Apple's [instance method][7] and [unreceived notifications][8] documentation for more details.

[1]: {{site.baseurl}}/api/endpoints/messaging/
[2]: {% image_buster /assets/img_archive/remote_notification.png %} "content available"
[3]: {% image_buster /assets/img_archive/background_mode.png %} "background mode enabled"
[4]: https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/pushing_background_updates_to_your_app
[5]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intfm/UIApplicationDelegate/application:didReceiveRemoteNotification:fetchCompletionHandler:
[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/swift/analytics/uninstall_tracking/
[7]: https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623013-application
[8]: https://developer.apple.com/library/content/technotes/tn2265/_index.html#//apple_ref/doc/uid/DTS40010376-CH1-TNTAG23
[9]: {{site.baseurl}}/user_guide/engagement_tools/locations_and_geofences