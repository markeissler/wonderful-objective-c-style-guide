# Mixtur Objective-C style guide.

This style guide outlines the coding conventions of the OSX and iOS team at Mixtur. This guide is just a more comprehensive and also more specific than the individual guides from which we drew our inspiration.

We welcome your feedback in issues and pull requests.

## Introduction

As with any coding style guide, this one has been created to not only gain a high level of consistency between disparate bodies of code but also to vastly improve legibility. While the former goal is important because it alleviates the engineer from having to adjust to differing stylistic conventions, the latter is critical when it comes down to the arduous task of scanning someone else's code in search of a time-tested pattern or an otherwise obvious error.

**The application and enforcement of a style guide is not about doing things the right way or the wrong way, it's about agreeing on doing things the same way.**

## Credits

To get here, we leveraged the work of [The official raywenderlich.com Objective-C style guide](https://github.com/raywenderlich/objective-c-style-guide/#methods), [The New York Times Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide) and the [Objective-C Cheat Sheet](https://github.com/iwasrobbed/Objective-C-CheatSheet). Many thanks to the contributors to those documents.

## Background

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Language](#language)
* [Code Organization](#code-organization)
* [Line Wrapping (Code Width)](#line-wrapping-(code-width))
* [Spacing](#spacing)
* [Comments](#comments)
* [Documentation](#documentation)
	* [Class Documentation](#class-documentation)
	* [Constant Documentation](#constant-documentation)
	* [Property Documentation](#property-documentation)
	* [Class Method Documentation](#class-method-documentation)
	* [Instance Method Documentation](#instance-method-documentation)
* [Naming](#naming)
    * [Naming Conventions for Methods and Variables](#naming-conventions-for-methods-and-variables)
    * [Naming Conventions for Constants and Macros](#naming-conventions-for-constants-and-macros)
    * [Naming Conventions for Enumerated Types](#naming-conventions-for-enumerated-types)
    * [Underscores](#underscores)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Switch Statements and Case Label Blocks](#switch-statements-and-case-label-blocks)
* [Private Properties](#private-properties)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
	* [Conditional Expressions](#conditional-expressions)
    * [Ternary Operator](#ternary-operator)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [CGRect Functions](#cgrect-functions)
* [Golden Path](#golden-path)
* [Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)
* [Xcode Project](#xcode-project)
* [Other Objective-C Style Guides](#other-objective-c-style-guides)


## Language

US English should be used.

**Preferred:**

```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not Preferred:**

```objc
UIColor *myColour = [UIColor whiteColor];
```


## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Class Methods

+ (ClassObject)classWithDefaults;
+ (ClassObject)convertToJSON;
+ (ClassObject)convertToXML;

#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## Line Wrapping (Code Width)
Although really big computer displays are the norm these days as they help boost the productivity of engineers, that productivity gain is bound to trend downward if a large portion of that screen real estate must be dedicated to a single code window because of excessively long lines. While automated wrapping may at first appear to be a solution, that feature tosses out all of the gains achieved from proper styling...and therefore efficiency is similarly lost. The better solution is to continue to restrict line width.

Line width is currently restricted to 80 columns. Xcode should be configured to display a Page Guide at 80 columns to assist with manual formatting:

	Preferences->Text Editing->Page Guide at column: 

As seen here:
![Xcode Page Guide Pref](http://mix-pub-dist.s3-website-us-west-1.amazonaws.com/objective-c-style-guide/img/pref_page_guide_sm.png)

Objective-C is a verbose language. Selectors can be very long. The recommendation is to use an Xcode plugin along with a formatting configuration to facilitate the reformatting of code to comply with this guide.

**SEE TOOLS BELOW -- PENDING**

## Spacing

* Indent using 2 spaces (this conserves space in print and makes line wrapping less likely). Never indent with tabs. Be sure to set this preference in Xcode.
* **Method braces and other braces** (`if`/`else`/`switch`/`while` etc.) **always open on a new line and close on a new line**.

**Preferred:**

```objc
if (user.isHappy) 
{
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**

```objc
if (user.isHappy) {
    // Do something
}
else {
    // Do something else
}
```

 In conditional statements, braces appear on the same line as an *else* or *else if* branching key word. The conditional statement is *less* readable when constructed as follows:
 
```objc
if (user.isHappy) 
{
  // Do something
} 
else 
{
  // Do something else
}
```  
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* Prefer using auto-synthesis. But if necessary, `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
* Colon-aligning method invocation should often be avoided.  There are cases where a method signature may have >= 3 colons and colon-aligning makes the code more readable. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes it illegible.

**Preferred:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. **Exception: This does not apply to those comments used to generate documentation.**

## Documentation

Header files should include completed documentation tags to support [Doxygen](http://doxygen.org) auto-generation via the [Doxywrite](https://github.com/markeissler/Doxywrite) tool. In general, *JavaDoc* style tags should be used. **Documentation for a public interface will be generated from header files only.** Include documentation for a private interface in the implementation file.

The following items should be documented to facilitiate developer on-boarding, development of new functionality, and the ongoing maintenance cycle:

* Classes
* Prototype
* Constants
* Properties
* Class Methods
* Instance Methods

**Class Documentation**

```objc
-- **EXAMPLE PENDING** --
```

**Constant Documentation**

```objc
/**
 *  @memberof PDFReaderConfig
 *  Default value for bookmarksEnabled: TRUE
 */
extern const BOOL kPDFReaderDefaultBookmarksEnabled;
```

**Property Documentation**

```objc
/**
 *  Enable/disable bookmark support.
 *
 *  @see kPDFReaderDefaultBookmarksEnabled
 */
@property (nonatomic, readwrite, unsafe_unretained, getter=isBookmarksEnabled)
    BOOL bookmarksEnabled;
```

**Class Method Documentation**

```objc
/**
 * Returns the shared PDFReaderConfig instance, creating it if necessary.
 *
 * @return The shared PDFReaderConfig instance.
 */
+ (instancetype)sharedConfig;
```

**Instance Method Documentation**

```objc
/**
 *  Initializes and returns a newly allocated PDFReaderViewController object.
 *
 *  @param object Reference to an initialized PDFReaderDocument
 *
 *  @return Initialized class instance or nil on failure
 *
 *  @throws "<MissingArguments>" When object is nil
 *  @throws "<WrongType>" When object is not a reference to a PDFReaderDocument
 *    object
 *
 *  @remark Designated initializer.
 */
- (instancetype)initWithReaderDocument:(PDFReaderDocument *)object;

/**
 *  Update bookmark state for each page in the PDFReaderDocument's bookmarks 
 *    list.
 */
- (void)updateToolbarBookmarkIcon;
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

### Naming Conventions for Methods and Variables
Long, descriptive method and variable names are good as they support the concept of self-documenting code. Therefore, while clarity and brevity are both important, clarity should never be sacrificed for brevity.

**Preferred:**

```objc
UIButton *settingsButton;
NSString *pageTitle = @"My Title";
int pageCounter = 0;
```

**Not Preferred:**

```objc
UIButton *setBut;
NSString *string = @"My Title";
int c = 0;
```

### Naming Conventions for Constants and Macros
The following naming patterns may at first appear overly complex but the intention is to support a quick way for an engineer to determine the type (i.e. constant or macro), the origin, as well as the location of the item's declaration. The pattern breaks down like this:

	tPRE_Space_Name
	
Where:

| Element | Definition                   | Value       | Example                              |
| ------- | -----------------------------| ----------- | ------------------------------------ |
| t       | type = constant              | k*          | **k**MIX_MyClass_DefaultTitle        |
|         | type = macro                 | m*          | **m**BUZ_MyClass_doubleIt            |
| PRE     | three-letter prefix          | XXX         | k**XXX**_MyClass_MenuBarHeight       |
| Space   | unique namespace within tPRE | WindowSize  | kXXX\_**MyClass**\_WindowSize        |
| Name    | unique name within tPRE_Space| BorderColor | kZRE_MyClass\_**BorderColor**        |
|         |                              | isIphone    | mZBB_MyClass\_**isIphone**           |

The type (t) element must either be a "k" (for constant) or an "m" (for macro). Other fields are to be defined by the developer.

**Constant names** should be camel-case with all words capitalized and prefixed by the letter "k" and a three-letter prefix followed by an underscore, followed by a specific namespace identifier (either the library name, class name, or app name), followed by another underscore which is then followed by the camel-case constant name (leading with an uppercase letter).

**Preferred:**

```objc
static const NSInteger kMIX_PDFReader_DefaultPagingViews = 3;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 1.7;
```

**Macro names** should be camel-case with all words capitalized and prefixed by the letter "m" and a three-letter prefix followed by an underscore, followed by a specific namespace identifier (either the library name, class name, or app name), followed by another underscore which is then followed by the camel-case function name (leading with a lowercase letter).

**Preferred**

```objc
#define mMIX_MacroLib_rgb(r,g,b) [UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:1.0f]
```

**Not Preferred**

```objc
#define RGBColor(r,g,b) [UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:1.0f]
```

**Property names** should be camel-case with the leading word being lowercase.

**Instance variable names** should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**Preferred:**

```objc
@property (nonatomic, readwrite, strong) NSString *descriptiveVariableName;

// if/when needed
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
...
@property (strong) id varnm;
...
@synthesize varnm;
```

### Naming Conventions for Enumerated Types
**Enumerated Type names** should be camel-case with all words capitalized and prefixed by the letter "e" and a three-letter prefix followed by an underscore, followed by a specific namespace identifier (either the library name, class name, or app name), followed by another underscore which is then followed by the camel-case constant name (leading with an uppercase letter).

**Preferred:**

```objc
typedef enum eMIX_UtilityLib_PlayerStateType : NSInteger eMIX_UtilityLib_PlayerStateType;
enum eMIX_UtilityLib_PlayerStateType : NSInteger {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};

// using the NS_ENUM macro (preferred)
typedef NS_ENUM(NSInteger, eMIX_UtilityLib_PlayerStateType) {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};
```

**Not Preferred:**

```objc
enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
};

typedef NSInteger PlayerState;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. 

An exception to this: inside initializers, the backing instance variable (i.e. `_variableName`) should be used directly to avoid any potential side effects of the getters/setters.

Local variables should not contain underscores.

## Methods

In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style).  Always include a keyword and be descriptive with the word before the argument which describes the argument.

The usage of the word "and" is reserved.  It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

**Preferred:**

```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent. 

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Preferred:**

```objc
@interface MyAppTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface MyAppTutorial : NSObject {
  NSString *tutorialName;
}
```


## Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code.  The order of properties should be atomicity, accessibility (readonly, readwrite), followed by storage. **Yes, we include all three attributes every time.**

This ordering varies from the generated code produced by Xcode's templates, but it increases legibility by enforcing a consistent visual pattern: most `@property` statements will begin with the same attributes (nonatomic, readwrite) which pushes the differences between adjacent statements to the right.

**Preferred:**

```objc
@property (nonatomic, readwrite, weak) IBOutlet UIView *containerView;
@property (nonatomic, readwrite, strong) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

Properties with mutable counterparts (e.g. NSString) should prefer `copy` instead of `strong`. 
Why? Even if you declared a property as `NSString` somebody might pass in an instance of an `NSMutableString` and then change it without you noticing that.  

**Preferred:**

```objc
@property (nonatomic, readwrite, copy) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic, readwrite, strong) NSString *tutorialName;
```

## Dot-Notation Syntax

Dot notation syntax is purely a convenient wrapper around accessor method calls. When you use dot notation, the property is still accessed or changed using the underlying getter and setter methods (auto-generated via synthesis or not).  Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html). 

**Example:**

```objc
- (void)updateMyBackroundColor
{
  // dot notation...
  self.bgColor = [UIColor blueColor];
  
  // is same as sending setBgColor message to self...
  [self setBgColor:[UIColor blueColor]];
}
```

It is because the getter and setter methods are still the underlying implementation involved that dot notation (and explicit messages to self) should generally be avoided within `init` and `dealloc` methods as discussed [here](http://qualitycoding.org/objective-c-init/#more-910). In these methods, access data members directly via the `_variableName`.

**Example:**

```objc
- (instancetype)init
{
  self = [super init];
  if (self)
  {
    _bgColor = [UIColor whiteColor];
  }

  return self;
}
```

Outside of `init` and `dealloc`, dot-notation should **always** be used for accessing and mutating properties as it makes code more concise.

**Preferred:**

```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**

```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## Constants

Constants (declared with the `const` keyword) are preferred over in-line string literals or numbers as they allow for easy reproduction of commonly used variables and can be quickly changed without the need to conduct an error-prone find and replace. Constants should never be declared using the `#define` preprocessor directive as that pattern circumvents type checking; you may, however, use `#define` when declaring a macro.

**Preferred:**

```objC
// Format: type const constantName = value;
NSString * const kMIX_MyClassShortDateFormat = @"MM/dd/yyyy";

// Macro: #define constantName value
\#define mMIX_MacroLib_isIPad (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad)
```

**Not Preferred:**

```objc
\#define CompanyName @"MyCompany"
\#define thumbnailHeight 2
```

To make a constant available to external classes, you must also add it to the header `.h` file:

```objC
extern NSString * const kMIX_MyClass_ShortDateFormat;
```

If the constant is to be used only within the implementation file in which it is declared, narrow its scope with the `static` keyword.

```objC
static NSString * const kMIX_MyClass_ShortDateFormat = @"MM/dd/yyyy";
```

A static variable declared within a method retains its value between invocations.  This can be useful when declaring a [singleton](#singletons) or creating custom setters/getters for a property.

## Enumerated Types

When declaring enumerated types, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`

**Preferred**

```objc
// using the NS_ENUM macro (preferred)
typedef NS_ENUM(NSInteger, eMIX_UtilityLib_PlayerStateType) {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};

// with explicit value assignments
typedef NS_ENUM(NSInteger, eMIX_UtilityLib_PlayerStateType) {
  PlayerStateOff = 0,
  PlayerStatePlaying = 1,
  PlayerStatePaused = 2
};
```

**Not Preferred:**

```objc
// Apple Modern Objective-C style, provides type checking, enum type declared inline...
typedef enum _eMIX_UtilityLib_PlayerStateType : NSUInteger {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
} eMIX_UtilityLib_PlayerStateType;

// Apple Modern Objective-C style, provides type checking, enum type declared seperately...
enum _eMIX_UtilityLib_PlayerStateType : NSInteger {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};
typedef enum _eMIX_UtilityLib_PlayerStateType : NSInteger eMIX_UtilityLib_PlayerStateType;

// Apple legacy style enum (32bit/64bit portability, no formal relationship between type and enum)...
enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
};
typedef NSInteger PlayerState;

// Apple legacy style enum (no 32bit/64bit portability)...
typedef enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
} PlayerState;
// NOTE: Implies...
// typedef int PlayerState;

// Generic C-style enum (no 32bit/64bit portability)...
typedef enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused} PlayerState;
```

## Switch Statements and Case Label Blocks

Braces for switch statements begin on a new line and close on a new line as mentioned in [Spacing](#spacing) above. Beware that braces are usually not required for case label blocks but may be enforced by the compiler and are definitely required when declaring variables within a case statement in order to define scope; therefore, when a case contains more than one line, braces should always be added.

**Note:** With regard to case blocks with braces, this is one situation where braces will begin on the same line as the `case:` label but will still end on a new line.

```objc
switch (condition) 
{
  case 1:
    // ...
    break;
    
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  
  case 3:
    // ...
    break;
  
   default: 
    // ...
    break;
}

```

There are times when the same code can be used for multiple cases, and a fall-through should be used.  A fall-through is the removal of the `break` statement for a case thus allowing the flow of execution to pass to the next case value.  A fall-through should be commented for coding clarity.

```objc
switch (condition) 
{
  case 1:
    // ** fall-through! **
    
  case 2:
    // code executed for values 1 and 2
    break;
    
  default: 
    // ...
    break;
}

```

When using an enumerated type for a switch, `default` is not needed.   For example:

```objc
kMIX_LeftMenuTopItemType menuType = kMIX_LeftMenuTopItemMain;

switch (menuType) {
  case kMIX_LeftMenuTopItemMain:
    // ...
    break;
  case kMIX_LeftMenuTopItemShows:
    // ...
    break;
  case kMIX_LeftMenuTopItemSchedule:
    // ...
    break;
}
```


## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `MIX_Private` or `private`) should never be used unless extending another class.   The Anonymous category can be shared/exposed for testing using the <headerfile>+Private.h file naming convention.

**For Example:**

```objc
@interface MyAppDetailViewController ()

@property (nonatomic, readwrite, strong) GADBannerView *googleAdView;
@property (nonatomic, readwrite, strong) ADBannerView *iAdView;
@property (nonatomic, readwrite, strong) UIWebView *adXWebView;

@end
```

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state. This pattern may at first appear to be result in unnatural file names but when glancing at a directory listing it will result in a natural grouping of obviously related files.

**Examples:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

## Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code.  Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**Preferred:**

```objc
if (someObject)
{
  // code...
}

if (![anotherObject boolValue]) 
{
  // code...
}
```

**Not Preferred:**

```objc
if (someObject == nil)
if ([anotherObject boolValue] == NO)
if (isAwesome == YES)     // Never do this.
if (isAwesome == true)    // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (nonatomic, readwrite, unsafe_unretained, getter=isEditable) BOOL editable;
```

Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**Preferred:**

```objc
if (!error) 
{
  return success;
}
```

**Not Preferred:**

```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

### Conditional Expressions

The expression evaluated in a conditional statement should generally consist of variables and not method calls. It's okay to declare temporary local variables ahead of the conditional. Because sending messages to Objective-C methods is often ridiculously verbose, calling the methods directly within the conditional expression often makes it hard to visually parse what exactly it is that is being tested. Collect your evaluation criteria, and then evaluate in the expression.

**Preferred:**

```objc
BOOL continuousPlayEnabled = [[MediaAppPrefs sharedInstance] continuousPlay];
MediaAppTrack *nextMediaTrack = [MediaAppPlayer nextTrack];
if (continuousPlayEnabled && nextMediaTrack)
{
  // play the next song
}
```

**Not Preferred:**

```objc
if ([[MediaAppPrefs sharedInstance] continuousPlay] && [MediaAppPlayer nextTrack])
{
  // play the next song
}
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something. Parentheses are always added for improved readability.

**Preferred:**

```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = (isHorizontal) ? x : y;
```

**Not Preferred:**

```objc
result = a > b ? x = c > d ? c : d : y;

result = isHorizontal ? x : y;
```

## Init Methods

Init methods should follow the convention provided by Apple's generated code template.  A return type of `instancetype` should also be used instead of `id`.

```objc
- (instancetype)init
{
  self = [super init];
  if (self) {
    // ...
  }

  return self;
}
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## Class Constructor Methods

Where class constructor methods are used, these should always return type of `instancetype` and never `id`. This ensures the compiler correctly infers the result type. 

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(MyAppAirplaneType)type;
@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Bail out and return early. Multiple return statements are OK.

**Preferred:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
	return;
  }

  //Do something important
}
```

**Not Preferred:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## Error handling

As mentioned in [Apple's developer documentation](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/ErrorHandling/ErrorHandling.html), when a method returns an NSError object by reference it's important to always test the return value of the  method call itself to determine whether an error has occurred.

If an error has occurred, the value of the NSError object will generally refer to a valid NSError object and the method will return NO; however, if no error has occurred, the NSError object may or may not be set to nil.

**Preferred:**

```objc
NSError *error;
BOOL success;
success = [self trySomethingWithError:&error];
if (!success) {
  // Handle Error
  // e.g.
  NSLog(@"Something bad just happened: %@", error);
}
```

**Not Preferred:**

```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

When an error is encountered your app should recover or present the user with the details (and possibly a solution). Leverage the information returned by the NSError object to determine the most appropriate course of action.

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.

```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  // thread-safe init
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```

This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## Line Breaks

Line breaks are an important topic since this style guide is focused for print and online readability.

For example:

```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

A long line of code like this should be carried on to the second line adhering to this style guide's Spacing section (two spaces).

```objc
self.productsRequest = [[SKProductsRequest alloc] 
  initWithProductIdentifiers:productIdentifiers];
```


## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Other Objective-C Style Guides

If ours doesn't fit your tastes, have a look at some other style guides:

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
