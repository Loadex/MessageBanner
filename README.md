Message Banner
==============

**This project is currently in development and not ready for use yet.**

This library provides an **easy to use and fully customizable class to show notifications** views on top/bottom/center of the screen.

The messages banners are regrouped in 4 different type : **Error**, **Warning**, **Message** and **Success**.

Each different type of banner can have different and fully customizable appearance and behavior. (See configuration section)

Each banner can show a **title**, a **subtitle**, an **image** and a **button**.

ScreenShots
----------------
![Error](/Screenshots/ErrorMessageBanner.png?raw=true "Standart Error message banner")
![Warning](/Screenshots/WarningMessageBanner.png?raw=true "Standart Warning message banner")
![Message](/Screenshots/MessageMessageBanner.png?raw=true "Standart Message message banner")
![Success](/Screenshots/SuccessMessageBanner.png?raw=true "Standart Success message banner")

Installation
----------------
### From CocoaPods
Add ```pod 'MessageBanner'``` to your Podfile.
### Manually
Writting in progress

Quick Start Guide
----------------


Add the import on the right place : 
```objective-c
#import <MessageBanner.h>
```

To show a basic notification use one of the following call :
```objective-c
[MessageBanner showMessageBannerInViewController:aViewController
		                                   title:@"aTitle"
       		                            subtitle:@"aSubtitle"];

// ------------------------- OR ------------------------- 

    [MessageBanner showMessageBannerInViewController:self
                                               title:@"aTitle"
                                            subtitle:@"aSubtitle"
                                               image:[UIImage imageNamed:@"icon.png"]
                                                type:MessageBannerTypeMessage
                                            duration:MessageBannerDurationDefault
                              userDissmissedCallback:^(MessageBannerView *bannerView) {
                                  return;
                              }
                                         buttonTitle:@"BtnTitile"
                           userPressedButtonCallback:^(MessageBannerView *banner) {
                                  return;
                              }
                                          atPosition:MessageBannerPositionTop
                                canBeDismissedByUser:YES delegate:self];

```

Class documentation
----------------
###MessageBanner class documentation
The **MessageBanner class** provide the following methods

```objective-c
+ (void)setDefaultViewController:(UIViewController *)aViewController;
```  
Allow you to set a default view controller where the banners will be attached.  
**Default value** : UIApplication sharedApplication].keyWindow.rootViewController

```objective-c
+ (void)setMessageBannerDelegate:(id<MessageBannerDelegate>)aDelegate;
```  
Allow to set the delegate of MessageBanner class  
**See delegate documentation**

```objective-c
+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                    title:(NSString *)title
                                 subtitle:(NSString *)subtitle;

+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                    title:(NSString *)title
                                 subtitle:(NSString *)subtitle
                               atPosition:(MessageBannerPosition)messagePosition;

+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                    title:(NSString *)title
                                 subtitle:(NSString *)subtitle
                                     type:(MessageBannerType)type
                               atPosition:(MessageBannerPosition)messagePosition;

+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                    title:(NSString *)title
                                 subtitle:(NSString *)subtitle
                                     type:(MessageBannerType)type
                                 duration:(NSTimeInterval)duration
                               atPosition:(MessageBannerPosition)messagePosition;

+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                    title:(NSString *)title
                                 subtitle:(NSString *)subtitle
                                     type:(MessageBannerType)type
                                 duration:(NSTimeInterval)duration
                   userDissmissedCallback:(void (^)(MessageBannerView* bannerView))userDissmissedCallback
                               atPosition:(MessageBannerPosition)messagePosition
                     canBeDismissedByUser:(BOOL)dismissingEnabled;

+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                    title:(NSString *)title
                                 subtitle:(NSString *)subtitle
                                    image:(UIImage *)image
                                     type:(MessageBannerType)type
                                 duration:(NSTimeInterval)duration
                   userDissmissedCallback:(void (^)(MessageBannerView* bannerView))userDissmissedCallback
                               atPosition:(MessageBannerPosition)messagePosition
                     canBeDismissedByUser:(BOOL)dismissingEnabled;

+ (void)showMessageBannerInViewController:(UIViewController *)viewController
                                   title:(NSString *)title
                                subtitle:(NSString *)subtitle
                                   image:(UIImage *)image
                                    type:(MessageBannerType)type
                                duration:(NSTimeInterval)duration
                                userDissmissedCallback:(void (^)(MessageBannerView* bannerView))userDissmissedCallback
                             buttonTitle:(NSString *)buttonTitle
                          userPressedButtonCallback:(void (^)(MessageBannerView* banner))userPressedButtonCallback
                              atPosition:(MessageBannerPosition)messagePosition
                    canBeDismissedByUser:(BOOL)dismissingEnabled
                                 delegate:(id <MessageBannerDelegate>)aDelegate;
```
Add a new MessageBannerView to be displayed. Each MessageBannerView is shown on the screen alone. If multiple calls, the MessageBanner will show it when the previous message banner views disappeared.  
**Parameters description**  
* **viewController**: The view controller to show the message banner in. Can also be a UINavigationController. If nil, will use the default view controller instead. 
* **title**: The title in the message banner view.
* **subtitle**: The subtitle in the message banner view.
* **image**: The left image showed in the message banner view.
* **type**: The message banner view type (See type section for available values). The message type allow to load the correct design in the design configuration file. (See configuration file section)
* **duration**: The message banner displaying duration in seconds. (See duration section for custom values) 
* **userDissmissedCallback**: The block that should be executed when a user dismiss a message banner. (see dismiss gestures section to see possible dismiss gestures)
* **buttonTittle**: The title of the button. If set will allow a button to be created otherwise the message banner won't have a button.
* **userPressedButtonCallback**: The block that should be executed when a user hit the button if not nil.
* **messagePosition**: The message position on the viewController. (see position section to see possible positions)
* **dismissingEnabled**: Enable or disable the user dismissing methods on the message banner view.
* **aDelegate**: Set the delegate of the message banner (See delegate documentation to see available delegate methods)

```objective-c
+ (BOOL) hideMessageBanner;
+ (BOOL) hideMessageBannerWithCompletion:(void (^)())completion;
```
Hide the currently displayed message banner  
**Parameters description**
* **completion**: The block that should be executed when the message banner is hidden.  
**Return value**
* **BOOL**: Return YES if a message banner has been dismissed, NO otherwise.

###MessageBannerView class documentation
The **MessageBannerView** class provide the following methods:  
```objective-c
+ (BOOL)addMessageBannerDesignFromFileNamed:(NSString *)file;
```
Load a custom message banner design file.  
**Parameters description**  
* **file**:The name of the JSON config file. (See design configuration file section for syntax)  
**Return value**: YES if the file is corretly loaded. NO otherwise.

Delegate methods
----------------
The message banner class implements a delegate protocol with the following **optional** methods:
```objective-c
- (void)messageBannerViewWillAppear:(MessageBannerView *)messageBanner;
- (void)messageBannerViewDidAppear:(MessageBannerView *)messageBanner;
- (void)messageBannerViewWillDisappear:(MessageBannerView *)messageBanner;
- (void)messageBannerViewDidDisappear:(MessageBannerView *)messageBanner;
```
**Parameters description**
* messageBanner The message banner about to be displayed/hidden

NSNotifications
----------------
The MessageBanner class also send signals when the message banner is about or did appear/disappear. Each notification is directly **sent by the messageBannerView**.
The signals sent are:


|Description                               |Notification name                         |Notification define                            | 
|:----------------------------------------:|:----------------------------------------:|:---------------------------------------------:|
|Sent when the view is about to appear.    |messageBannerViewWillAppearNotification   |MESSAGE_BANNER_VIEW_WILL_APPEAR_NOTIFICATION   |
|Sent when the view is about to appear.    |messageBannerViewDidAppearNotification    |MESSAGE_BANNER_VIEW_DID_APPEAR_NOTIFICATION    |
|Sent when the view is about to disappear. |messageBannerViewWillDisappearNotification|MESSAGE_BANNER_VIEW_WILL_DISAPPEAR_NOTIFICATION|
|Sent when the view is about to disappear. |messageBannerViewDidDisappearNotification |MESSAGE_BANNER_VIEW_DID_DISAPPEAR_NOTIFICATION |

Message Positions
-----------------
Each Message Banner View has a position. By default or if not set, the message banner view will have the position defined by **MessageBannerPositionTop**
Available positions are:
* **MessageBannerPositionTop**: The message banner will be displayed on the top of the view controller, right under the status bar or the navigation controller if the view has one.
* **MessageBannerPositionCenter**: The message banner will be displayed on the middle of the view controller.
* **MessageBannerPositionBottom**: The message banner will be displayed on the bottom of the view controller, or just above the toolbar if the view has one.  
![Top Positionning](/Screenshots/MessageBannerTopPosition.png?raw=true "Top positioned message banner")
![Center Positioning](/Screenshots/MessageBannerMiddlePosition.png?raw=true "Centered message banner")
![Bottom Positioning](/Screenshots/MessageBannerBottomPosition.png?raw=true "Bottom positioned message banner")

Message types
----------------
Each Message Banner is defined by his type. The desig in the config file is loaded according to the his type.
Available type with their already setted design are :
* **MessageBannerTypeError**: The error message banner.
* **MessageBannerTypeWarning**: The warning message banner.
* **MessageBannerTypeMessage**: The standart message banner.
* **MessageBannerTypeSuccess**: The success message banner.  
![Message Error](/Screenshots/MessageBannerErrorType.png?raw=true "Error message banner")
![Message Warning](/Screenshots/MessageBannerWarningType.png?raw=true "Warning message banner")
![Message](/Screenshots/MessageBannerMessageType.png?raw=true "Message message banner")
![Message Success](/Screenshots/MessageBannerSuccessType.png?raw=true "Success message banner")

Message duration
----------------
Each Message Banner has a "stay on screen" time. This duration can be manually setted by the method parameter. However, the class also provide custom durations :
* **MessageBannerDurationDefault**: The duration will be automatically calculated according to the size of the message banner view.
* **MessageBannerDurationEndless**: The message banner will be displayed until the user dismiss it or it is dismissed programatically.

Dismiss methods
----------------
The message banner can be dismissed with multiple user gesture. The user can dismiss it with a **single tap** on the view and swipes gesture. The swipes dismiss gesture are different according to the position of the banner.
* **MessageBannerPositionTop**: The user can **swipe to the top** of the screen to dismiss the banner.
* **MessageBannerPositionCenter**: The user can **swipe both to the left and right** to dismiss the banner.
* **MessageBannerPositionBottom**: The user can **swipe to the bottom** of the screen to dismiss the banner.

Design configuration file
----------------
The configuration file is a JSON formated file.

The JSON file is formatted this way :

```JSON
{
    "MessageBannerTypeName": {
        "PropertyName"    : "Property value"
        ,"AnotherProperty" : 1.0
    },
    
    "AnOtherMessageBannerTypeName": {
        "PropertyName"    : "Property value"
        ,"AnotherProperty" : 1.0
    }
}
```

This library come with 4 differents type of banner, with the following corresponding name for the JSON design file format :

|   Name   |   Type   |
|:--------:|:--------:|
|Error     |MessageBannerTypeError|
|Warning   |MessageBannerTypeWarning|
|Message   |MessageBannerTypeMessage|
|Success   |MessageBannerTypeSuccess|

Each type of banner can have the following properties : 

### General properties
|     Property name     |                    Effect                   |    Possible values   |   Default value    |                Remarks                    |
|:---------------------:|---------------------------------------------|:--------------------:|:------------------:|-------------------------------------------|
|defaultImageForType    | Define a default left image                 | "The_image_name.png" |         None       |The image parameter override this setting  |
|blurRadius             | Blur the attached view with the given radius|         0.0+         |   0.0 (Disabled)   | iOS7 style default radius for blur is 40.0|
|backgroundColor        | Set the banner background color             |      "#FFFFFF"       | "#FFFFFF" (White)  | BackgroundImageName override this parameter|
|backgroundImageName    | Set the banner background image             | "The_image_name.png" |        None        | Override BackgroundColor property         |
|backgroundAlpha        | Set the banner background color transparency|     0.0 -> 1.0       |        1.0         |                                           |

### Title properties
|     Property name     |                    Effect                   |    Possible values   |   Default value    |                Remarks                    |
|:---------------------:|---------------------------------------------|:--------------------:|:------------------:|-------------------------------------------|
|titleTextColor         | Set the banner title text color             |     "#FFFFFF"        | "#000000" (Black)  |                                           |
|titleTextSize          | Set the banner title text size              | 0.0+                 |        14.0        |                                           |
|titleTextShadowColor   | Set the banner title text shadow color      | "#FFFFFF"            | "#FFFFFF" (White)  |                                           |
|titleTextShadowAlpha   | Set the banner title text shadow transparency| 0.0 -> 1.0          |        1.0         |                                           |
|titleTextShadowOffsetX | Set the banner title text x shadow offset   |      -0.0+           |         0.0        |                                           |
|titleTextShadowOffsetY | Set the banner title text y shadow offset   |      -0.0+           |         1.0        |                                           |

### Subtitle properties
|     Property name     |                    Effect                   |    Possible values   |   Default value    |                Remarks                    |
|:---------------------:|---------------------------------------------|:--------------------:|:------------------:|-------------------------------------------|
|subtitleTextColor         | Set the banner subtitle text color             |     "#FFFFFF"        | "#000000" (Black)  |                                           |
|subtitleTextSize          | Set the banner subtitle text size              | 0.0+                 |        14.0        |                                           |
|subtitleTextShadowColor   | Set the banner subtitle text shadow color      | "#FFFFFF"            | "#FFFFFF" (White)  |                                           |
|subtitleTextShadowAlpha   | Set the banner subtitle text shadow transparency| 0.0 -> 1.0          |        1.0         |                                           |
|subtitleTextShadowOffsetX | Set the banner subtitle text x shadow offset   |      -0.0+           |         0.0        |                                           |
|subtitleTextShadowOffsetY | Set the banner subtitle text y shadow offset   |      -0.0+           |         1.0        |                                           |

### Button properties
|     Property name     |                    Effect                   |    Possible values   |   Default value    |                Remarks                    |
|:---------------------:|---------------------------------------------|:--------------------:|:------------------:|-------------------------------------------|
|buttonBackgroundColor  | Set the button background color             | "#FFFFFF"            |  "#FFFFFF" (White) | buttonBackgroundImage and buttonBackgroundPatternImage override this parameter|
|buttonBackgroundImage  | Set the button background image               |  "The_image_name.png"|        None        | Ovveride buttonBackgroundColor            |
|buttonBackgroundPatternImage  | Set the button background image with a pattern image |  "The_image_name.png"|        None        | Ovveride buttonBackgroundImage            |
|buttonBackgroundAlpha  | Set the button background transparency      |  0.0 -> 1.0          |        1.0         |                                           |
|buttonCornerRadius     | Set the button corner radius                |       0.0+           |        0.0         |                                           |
|buttonBorderColor      | Set the button border color                 | "#FFFFFF"            |  "#000000"(Black)  | Visible only if buttonBorderSize >= 1     |
|buttonBorderAlpha      | Set the button border transparency          | 0.0 -> 1.0           |       1.0          |                                           |
|buttonBorderSize       | Set the border size of the button           | 0.0+                 |      0.0 (None)    |                                           |
|buttonTextColor         | Set the banner button text color             |     "#FFFFFF"        | "#000000" (Black)  |                                           |
|buttonTextSize          | Set the banner button text size              | 0.0+                 |        14.0        |                                           |
|buttonTextShadowColor   | Set the banner button text shadow color      | "#FFFFFF"            | "#FFFFFF" (White)  |                                           |
|buttonTextShadowAlpha   | Set the banner button text shadow transparency| 0.0 -> 1.0          |        1.0         |                                           |
|buttonTextShadowOffsetX | Set the banner button text x shadow offset   |      -0.0+           |         0.0        |                                           |
|buttonTextShadowOffsetY | Set the banner button text y shadow offset   |      -0.0+           |         1.0        |                                           |

Roadmap
----------------
Writting in progress
FAQ
----------------
No question asked so far.

Requirements
----------------
Writting in progress

Repository Infos
----------------

    Owner:      Thibault Carpentier
    GitHub:     https://github.com/Loadex
    LinkedIn:   www.linkedin.com/in/CarpentierThibault/
    StackOverflow:  http://stackoverflow.com/users/1324369/loadex
