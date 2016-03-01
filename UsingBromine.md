# Using Bromine #

## Introduction ##

Bromine will run your application on simulator executing a user-defined list of commands that will manipulate and verify application status.

Your 'screens' will be represented as XML so your commands will be able to access you view through XPath.

## Installing Bromine ##
  1. export Bromine from svn to your project's folder or use it as an external svn resource;
  1. back to XCode, duplicate your application main target and name anything you like (eg. Bromine);
  1. add Bromine folder to your project and link it **only** with Bromine target;
  1. edit Bromine target and, in Build section, do the following:
    1. add this to the Header Search Path: **/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS2.0.sdk/usr/include/libxml2**;
    1. link the above library to your Frameworks folder
    1. add the preprocessor macro (_GCC\_PREPROCESSOR\_DEFINITIONS_) with value _BROMINE\_ENABLED_;
    1. make sure _GCC\_C\_LANGUAGE\_STANDARD_ is _c99_;
  1. now the invasive part: add the following lines to your application delegate:
```
#import "MyAppDelegate.h"

#ifdef BROMINE_ENABLED
    #import "ScriptRunner.h"
#endif

@implementation MyAppDelegate

...

- (void)applicationDidFinishLaunching:(UIApplication *)application {
    ...

    #ifdef BROMINE_ENABLED
	[[[ScriptRunner alloc] init] autorelease];
    #endif

}

```
  1. create a _TestScript.plist_ file somewhere in your application.

## Writing tests ##

Once you created _TestScript.plist_ file you'll be able to start testing your application. Your test script source code should look like this:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<array>
	<dict>
		<key>command</key>
		<string>waitForElement</string>
		<key>viewXPath</key>
		<string>//UINavigationItemView[title="UICatalog"]</string>
	</dict>
	<dict>
		<key>command</key>
		<string>simulateTouch</string>
		<key>viewXPath</key>
		<string>//UITableViewCell[text="TextFields"]</string>
	</dict>
	<dict>
		<key>command</key>
		<string>waitForElement</string>
		<key>viewXPath</key>
		<string>//UINavigationItemView[title="TextFields"]</string>
	</dict>
	<dict>
		<key>command</key>
		<string>simulateTouch</string>
		<key>viewXPath</key>
		<string>//UINavigationButton[title="Edit"]</string>
	</dict>

	<dict>
		<key>command</key>
		<string>setText</string>
		<key>viewXPath</key>
		<string>/./descendant::UITextField[1]</string>
		<key>text</key>
		<string>text 1</string>
	</dict>
	<dict>
		<key>command</key>
		<string>pause</string>
		<key>seconds</key>
		<string>9</string>
	</dict>
	<dict>
		<key>command</key>
		<string>outputView</string>
	</dict>
</array>
</plist>

```

If you're not familiarized with plists files, this represents an array of dictionaries and, in this case, eac dictionary represents a command to be executed against your applications.

The above example should do the following steps:
  1. wait for some UINavigationItemView with title "UICatalog" to be avaiable;
  1. simulate an user touch on some UITableViewCell with text "TextFields";
  1. wait the UINavigationItemView to change its title to "TextFields;
  1. touches an Edit UINavigationButton;
  1. find the first UITextField avaiable and set its text to "text 1";
  1. pause the script (but not the application) for 9 second;
  1. print the XML representation of current application status to the console window;
  1. finish gracefully.

### "Oh, this seems pretty easy... to you. How can I write my own tests?" ###

Ok, ok... you'll be able to write your own tests as soon as you find the following 2 missing pieces.

### XML representantion ###

Bromine will descript your windows and views as a single - and probably huge - XML structure.

#### How can I get this XML? ####

Through the **outputView** command! You can use it anytime during the test process to obtain a "snapshot" of avaiable views. Those views should be the ones you'll may execute commands on.

You can also save your XML directly to a file so you'll be able to manipulate it in special editors. Just add _outputPath_ parameter with the file path to the _outputView_ command.

Remember: Not only UIView is a view. All of its descendants are views to like UIButton, UITextField, UITableView, etc.

Now that you know "who" your views are, you must find a way to "contact" them.

### XPath ###

XPath is a language for finding information in an XML document. You'll find a nice tutorial on [W3Schools - XPath Tutorial](http://www.w3schools.com/xpath/default.asp).

Now you can do the magic. Get your application XML, define XPath sentence for the desired views and execute your commands on them. Will can take a look at the AvailableCommands list

## Nothing like a good example ##

I know it's pretty boring read all the stuff above. If you feel like skipping the reading and seeing Bromine in action, just check out the UICatalog sample (http://bromine.googlecode.com/svn/samples/UICatalog).

Don't forget to leave console window open so you can watch your commands' results.

## Please, contribute ##

No.. not asking for money...

There are many possible features to be improved or added to Bromine. Feel free to notify bugs, make suggestions or critics and, even better, **send patches**!