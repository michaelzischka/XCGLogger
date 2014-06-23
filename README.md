#XCGLogger
#####By: Dave Wood
- Cerebral Gardens http://www.cerebralgardens.com/
- Twitter: [@CerebralGardens](https://twitter.com/CerebralGardens)

###tl;dr
A debug log framework for use in Swift projects. Allows you to log details to the console (and optionally a file), just like you would have with NSLog or println, but with additional information such as the date, function name, filename and line number.

Go from this:

```Simple message```

to this:

```2014-06-09 06:44:43.600 [Debug] [AppDelegate.swift:40] application(_:didFinishLaunchingWithOptions:): Simple message```

###Compatibility

XCGLogger works in both iOS and OS X projects. It is a Swift library intended for use in Swift projects.

Swift does away with the c preprocessor, which kills the ability to use ```#define``` macros. This means our traditional way of generating nice debug logs is dead. Resorting to just plain old ```println``` calls means you lose a lot of helpful information, or requires you to type a lot more code.

###How to Use

Add the XCGLogger files to your project as an embedded framework.

In each source file:

```Swift
import XCGLogger
```

In your AppDelegate, declare a global constant to the default XCGLogger instance.

```Swift
let log = XCGLogger.defaultInstance()
```

**Note**: previously this was ```XCGLogger.sharedInstance()```, but it was changed to better reflect that you can create multiple instances.

In the 
```Swift
application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: NSDictionary?)
```
function, configure the options you need:

```Swift
log.setup(logLevel: .Debug, showLogLevel: true, showFileNames: true, showLineNumbers: true, writeToFile: "path/to/file")
```
The value for ```writeToFile:``` can be a ```String``` or ```NSURL```. If the file already exists, it will be cleared before we use it. Omit a value or set it to nil to log to the console only.

Then, whenever you'd like to log something, use one of the convenience methods:

```Swift
log.verbose("A verbose message, usually useful when working on a specific problem")
log.debug("A debug message")
log.info("An info message, probably useful to power users looking in console.app")
log.error("An error occurred, but it's recoverable, just info about what happened")
log.severe("A severe error occurred, we are likely about to crash now")
```

The different methods set the log level of the message. XCGLogger will only print messages that >= its current log level setting.

###Advanced Use

It's possible to create multiple instances of XCGLogger with different options. For example, you only want to log a specific section of your app to a file, perhaps to diagnose a specific issue a user is seeing. In that case, create alternate instances like this:

```Swift
let fileLog = XCGLogger()
fileLog.setup(logLevel: .Debug, showLogLevel: true, showFileNames: true, showLineNumbers: true, writeToFile: "path/to/file")
fileLog.info("Have a second instance for special use")
```

You can create alternate log destinations (besides the two built in ones for the console  and a file). Your custom log destination must implement the ```XCGLogDestinationProtocol``` protocol. Instantiate your object, configure it, and then add it to the ```XCGLogger``` object with ```addLogDestination```. Take a look at ```XCGConsoleLogDestination``` and ```XCGFileLogDestination``` for examples.

Each log destination can have its own log level. Setting the log level on the log object itself will pass that level to each destination. Then set the destinations that need to be different.

###To Do
- Access NSDateFormatters in a thread safe manner
- Add examples of some advanced use cases
- Add additional log destination types

###More

If you find this library helpful, you'll definitely find these other tools helpful:

Watchdog: http://watchdogforxcode.com/

Slender: http://dragonforged.com/slender/

Briefs: http://giveabrief.com/


###Change Log

* **Version 1.0**: *(2014/06/09)* - Initial Release
* **Version 1.1**: *(2014/06/22)* - Changed the internal architecture to allow for more flexibility


