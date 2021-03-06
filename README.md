# JSQCoreDataKit
[![Version Status](http://img.shields.io/cocoapods/v/JSQCoreDataKit.png)][docsLink] [![license MIT](http://img.shields.io/badge/license-MIT-orange.png)][mitLink]

*A swifter Core Data stack*

## About
This project is a Swift rewrite and rethinking of [RSTCoreDataKit](https://github.com/rosettastone/RSTCoreDataKit). 

This framework aims to simplify the processes of standing up and testing your [Core Data](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CoreData/cdProgrammingGuide.html) stack, and reduces the boilerplate involved in getting started with Core Data. 

![stack](https://developer.apple.com/library/ios/documentation/DataManagement/Devpedia-CoreData/Art/single_persistent_stack.jpg)

*The Core Data Stack, via Apple [Core Data Core Competencies](https://developer.apple.com/library/ios/documentation/DataManagement/Devpedia-CoreData/coreDataStack.html#//apple_ref/doc/uid/TP40010398-CH25-SW1) Guide*

## Requirements

* iOS 8
* Swift 1.2

## Installation

From [CocoaPods](http://cocoapods.org):

````ruby
use_frameworks!

# For latest release in cocoapods
pod 'JSQCoreDataKit'  

# Feeling adventurous? Get the latest on develop
pod 'JSQCoreDataKit', :git => 'https://github.com/jessesquires/JSQCoreDataKit.git', :branch => 'develop'
````

Manually:

1. Clone this repo and add the `JSQCoreDataKit.xcodeproj` to your project
2. Select your project app target "Build Phases" tab
3. Add the `JSQCoreDataKit.framework` to the "Link Binary With Libraries"  
4. Create a new build phase of type "Copy Files" and set the "Destination" to "Frameworks"
5. Add the `JSQCoreDataKit.framework` and check "Code Sign On Copy"

For an example, see the demo project included in this repo.

For more information, see the [Framework Programming Guide](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Tasks/IncludingFrameworks.html#//apple_ref/doc/uid/20002257-BAJJBBHJ).

## Documentation

Read the fucking [docs](http://www.jessesquires.com/JSQCoreDataKit/). Generated with [jazzy](https://github.com/realm/jazzy). Hosted by [GitHub Pages](https://pages.github.com).

````bash
# regenerate documentation
$ cd /path/to/JSQCoreDataKit/
$ ./build_docs.sh
$ open _docs/
````

## Getting Started

````swift
import JSQCoreDataKit
````

#### Standing up the stack

````swift
// Initialize the Core Data model, this class encapsulates the notion of a .xcdatamodeld file
// The name passed here should be the name of an .xcdatamodeld file
let model = CoreDataModel(name: "MyModel", bundle: NSBundle(identifier: "com.MyApp.MyModelFramework")!)

// Initialize a default stack
let stack = CoreDataStack(model: model)

// Initialize a private queue, in-memory stack
let privateStack = CoreDataStack(model: model, storeType: NSInMemoryStoreType, options: nil, concurrencyType: .PrivateQueueConcurrencyType)
````

#### Saving a managed object context

````swift
// Saving returns a tuple (Bool, NSError?)
let result: ContextSaveResult = saveContextAndWait(stack.managedObjectContext)
if !result.success {
    // save failed
    println("Save error: \(result.error)")
}
````

#### Deleting the store

````swift
let model = CoreDataModel(name: "MyModel", bundle: NSBundle(identifier: "com.MyApp.MyModelFramework")!)
model.removeExistingModelStore()
````

#### Checking migrations

````swift
let model = CoreDataModel(name: "MyModel", bundle: NSBundle(identifier: "com.MyApp.MyModelFramework")!)
let needsMigration: Bool = model.modelStoreNeedsMigration
````

#### Using child contexts

````swift
let model = CoreDataModel(name: "MyModel", bundle: NSBundle(identifier: "com.MyApp.MyModelFramework")!)
let stack = CoreDataStack(model: model)

// Create a default child context on main queue
let childContext = stack.childManagedObjectContext()

// Create a private queue child context with custom merge policy
let privateChildContext = stack.childManagedObjectContext(concurrencyType: .PrivateQueueConcurrencyType, mergePolicyType: .ErrorMergePolicyType)
````

## Unit tests

There's a suite of unit tests for the `JSQCoreDataKit.framework`. To run them, open `JSQCoreDataKit.xcworkspace`, select the `JSQCoreDataKit` scheme, then &#x2318;-u.

These tests are well commented and serve as further documentation for how to use this library.

## Contribute

Please follow these sweet [contribution guidelines](https://github.com/jessesquires/HowToContribute).

## Credits

Created and maintained by [**@jesse_squires**](https://twitter.com/jesse_squires)

This project is a Swift rewrite and rethinking of [RSTCoreDataKit](https://github.com/rosettastone/RSTCoreDataKit).

## License

`JSQCoreDataKit` is released under an [MIT License][mitLink]. See `LICENSE` for details.

>**Copyright &copy; 2015 Jesse Squires.**

*Please provide attribution, it is greatly appreciated.*

[mitLink]:http://opensource.org/licenses/MIT
[docsLink]:http://www.jessesquires.com/JSQCoreDataKit
