## Table of Contents

- [Language](#language)
- [Code Organization](#code-organization)
	- [Imports](#imports)
- [Spacing](#spacing)
- [Comments](#comments)
- [Naming](#naming)
	- [Underscores](#underscores)
	- [Assets](#assets)
- [Methods](#methods)
- [Variables](#variables)
- [Type Safety](#type-safety)
- [Property Attributes](#property-attributes)
- [Dot Notation Syntax](#dot-notation-syntax)
- [Literals](#literals)
- [Constants](#constants)
- [Enumerated Types](#enumerated-types)
- [Case Statements](#case-statements)
- [Private Properties](#private-properties)
- [Booleans](#booleans)
- [Conditionals](#conditionals)
	- [Ternary Operator](#ternary-operator)
- [Init Methods](#init-methods)
- [Class Constructor Methods](#class-constructor-methods)
- [CGRect Functions](#cgrect-functions)
- [Golden Path](#golden-path)
- [Error handling](#error-handling)
- [Singletons](#singletons)
- [Xcode Project](#xcode-project)

## Language

US English should be used for everything including variable and class names, documentation and comments.

__Preferred:__

``` objc
UIColor *myColor = [UIColor whiteColor];
```

__Not Preferred:__

``` objc
UIColor *myColour = [UIColor whiteColor];
```

## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure:

``` objc
#pragma mark - Base class name

- (instancetype)init {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - Actions

- (IBAction)submitData:(id)sender {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Public

- (void)publicMethod {}
```

Use `#pragma mark` for subcategories _(except the first one)_ this way:

``` objc
#pragma mark - Private

#pragma mark - First

- (void)firstMethod {}

#pragma mark Second

- (void)secondMethod {}
```

There should be one newline before and after the `#pragma mark`.

### Imports

The order of import groups separated by blank lines should be this:
- For `.m` files, the counterpart `.h` file.
- 3rd party frameworks.
- 3rd party libraries.
- Own utility methods.
- Own `UIView` or `NSObject` subclasses.
- The rest of the imports (interface elements, view controllers) sorted alphabetically or rarely by the order of use.

``` objc
//
// SettingsViewController.m
//

#import "SettingsViewController.h"

#import <FacebookSDK/FacebookSDK.h>

#import "DateTools.h"

#import "DatabaseUtils.h"

#import "AutoupdatingLabel.h"

#import "DifficultyViewController.h"
#import "ProfileViewController.h"
#import "SocialViewController.h"
```

## Spacing

- Indent using tabs. Never indent with spaces. Be sure to set this preference in Xcode.
- Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
- One space should be added before the first opening parenthesis and braces and none between parantheses and their contents.

__Preferred:__

``` objc
if (user.isHappy) {
	// something
} else {
	// something else
}
```

__Not Preferred:__

``` objc
if(user.isHappy)
{
	// something
}
else{
	// something else
}
```

- There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
- Prefer using auto-synthesis. But if necessary, `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
- Colon-aligning method invocation should be avoided.

__Preferred:__

``` objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
	// something
} completion:^(BOOL finished) {
	// something
}];
```

__Not Preferred:__

``` objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

- There should be one newline after the method's header for improved readability.

__Preferred:__

``` objc
- (void)myMethod {

	// something
}
```

__Not Preferred:__

``` objc
- (void)myMethod {
	// something
}
```

- Don't put a space between an object type and the protocol it conforms to.

__Preferred:__

``` objc
@property (weak, nonatomic) id<MyDelegate> myDelegate;
```

__Not Preferred:__

``` objc
@property (weak, nonatomic) id <MyDelegate> myDelegate;
```

- Separate class extension and implementation content by newlines.

__Preferred:__

``` objc
@interface DatabaseManager ()

// properties

@end

@implementation MyClass

// body (private constants and methods)

@end
```

- When doing math, use a single space between operators, Unless that operator is unary, in which case don't use a space.

__Preferred:__

``` objc
NSInteger index = position % 2 + 25;
index++;
index += 1;
```

__Not Preferred:__

``` objc
NSInteger index = position%2+25;
NSInteger index=position % 2 + 25;
index ++;
index + = 1;
```

- Generally, the order of statements should be: variable declaration, property accessing and method calling, separated by one newline.

``` objc
- (void)loadViews {

	UILabel *titleLabel = [UILabel alloc] init];

	titleLabel.text = @"Settings";
	titleLabel.center = self.view.center;

	[titleLabel setNeedsLayout];
}
```

- Always end a file with a newline.

__Preferred:__

``` objc
@end

```

__Not Preferred:__

``` objc
@end
```

## Comments

When they are needed, comments should be used to explain __why__ a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. _Exception: This does not apply to those comments used to generate documentation._

Comments regarding one line should be added at the end of it separated by one space and comments regarding multiple lines should be added before the first line. The comment text and `//` should be separated by one space.

__Preferred:__

``` objc
NSInteger abstractNumber; // comment

// comment
NSInteger oneAbstractNumber;
NSInteger anotherAbstractNumber;
```

__Not Preferred:__

``` objc
NSInteger abstractNumber;// comment
NSInteger abstractNumber; //comment

NSInteger oneAbstractNumber;//comment
NSInteger anotherAbstractNumber;
```

## Naming

Apple naming conventions should be adhered to wherever possible.

Long, descriptive method and variable names are good.

__Preferred:__

``` objc
UIButton *settingsButton;
UIViewController *mainViewController;
```

__Not Preferred:__

``` objc
UIButton *setBut;
UIViewController *mainVC;
```

For IBOutlets, the name should start with its class name without the prefix for easier autocompletion of code.

__Preferred:__

``` objc
@property (strong, nonatomic) IBOutlet UIButton *buttonSettings;
```

__Not Preferred:__

``` objc
@property (strong, nonatomic) IBOutlet UIButton *settingsButton;
```

A three letter prefix should always be used for class names and constants, however may be omitted for Core Data entity names.
Avoid using overly simple names like `Model`, `View`, `Object`. 

Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

__Preferred:__

``` objc
static NSTimeInterval const IOTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

__Not Preferred:__

``` objc
static NSTimeInterval const fadetime = 1.7;
```

Properties should be camel-case with the leading word being lowercase. Use auto-synthesis for properties rather than manual @synthesize statements unless you have good reason.

__Preferred:__

``` objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

__Not Preferred:__

``` objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`.

An exception to this: inside initializers, the backing instance variable (i.e. _variableName) should be used directly to avoid any potential side effects of the getters/setters.

Local variables should not contain underscores.

### Assets

Use descriptive file namings. Describe what the asset is used for, whether it is a background, button or image. Delimit the words by underscore and use only lowercase, following this format:

	<module>_<element type>_<descriptive name>_<status>[@<retina>].png

__Preferred:__

	login_background@2x.png
	login_button_register_normal@2x.png
	login_button_register_highlighted@2x.png
	login_button_register_disabled@2x.png
	login_button_register_selected@2x.png
	login_icon_company.png
	settings_cell_label_background_company.png

__Not Preferred:__

	background@2x.png
	button_normal@2x.png
	login_button_register_normal_2x.png
	highlighted_login_button_register@2x.png
	disabled_login_button_register@2x.png
	selected_login_button_register@2x.png
	icon_company_login.png

## Methods

In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.

The usage of the word `and` is reserved. It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

__Preferred:__

``` objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

__Not Preferred:__

``` objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (id)taggedView: (NSInteger)tag;
- (id)taggedView:(NSInteger) tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height; // never do this.
```

Avoid having empty methods in classes unless they are overriding a method and it needs to be empty.

__Preferred:__

``` objc
- (void)viewDidLoad {

	[super viewDidLoad];

	// something
}
```

__Not Preferred:__

``` objc
- (void)viewDidLoad {

	[super viewDidLoad];
}
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent.

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

__Preferred:__

``` objc
@interface IOTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

__Not Preferred:__

``` objc
@interface IOTutorial : NSObject {
	NSString *tutorialName;
}
```

## Type Safety

Be as type safe as possible. Avoid using `id` or base classes like `NSObject` or `UIView` where possible. Use the lowest common multiple preferring a protocol or an abstract class.

__Preferred:__

``` objc
+ (StorageManager *)managerWithData:(StorageData *)data {

	return [[StorageManager alloc] initWithData:data];
}
```

__Not Preferred:__

``` objc
+ (id)managerWithData:(id)data {

	return [[StorageManager alloc] initWithData:(StorageData *)data];
}
```

## Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code. The order of properties should be storage then atomicity, which is consistent with automatically generated code when connecting UI elements from Interface Builder.

__Preferred:__

``` objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

__Not Preferred:__

``` objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

Properties with mutable counterparts (e.g. NSString) should prefer `copy` instead of `strong`.
Why? Even if you declared a property as `NSString` somebody might pass in an instance of an `NSMutableString` and then change it without you noticing that.

__Preferred:__

``` objc
@property (copy, nonatomic) NSString *tutorialName;
```

__Not Preferred:__

``` objc
@property (strong, nonatomic) NSString *tutorialName;
```

## Dot Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you use dot syntax, the property is still accessed or changed using getter and setter methods.

Dot notation should __always__ be used for accessing and mutating properties, as it makes code more concise. The only eception are the cases where the corresponding accesor method has been overriden, in which case the bracket notation should be used to avoid confusion. Bracket notation is preferred in all other instances.

__Preferred:__

``` objc
NSInteger arrayCount = self.array.count;
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

__Not Preferred:__

``` objc
NSInteger arrayCount = [self.array count];
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.
Because literal strings are created using `@` and the string eclosed in quotes, with no space, all other literals should be created in the same way too keep them consistent.

__Preferred:__

``` objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @(YES);
NSNumber *buildingStreetNumber = @(10018);
```

__Not Preferred:__

``` objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

or

``` objc
NSArray *names = @[ @"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul" ];
NSDictionary *productManagers = @{ @"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill" };
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
NSNumber *postalCodeNumber = @( 10018 );
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

__Preferred:__

``` objc
static NSString * const IOAboutViewControllerCompanyName = @"iulianonofrei.com";

static CGFloat const IOImageThumbnailHeight = 50.0;
```

__Not Preferred:__

``` objc
#define CompanyName @"iulianonofrei.com"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes the macro `NS_ENUM()` to facilitate and encourage use of fixed underlying types.

``` objc
typedef NS_ENUM(NSInteger, IOLeftMenuTopItemType) {
	IOLeftMenuTopItemMain,
	IOLeftMenuTopItemShows,
	IOLeftMenuTopItemSchedule
};
```

You can also make explicit value assignments (showing older k-style constant definition):

``` objc
typedef NS_ENUM(NSInteger, IOGlobalConstants) {
	IOPinSizeMin = 1,
	IOPinSizeMax = 5,
	IOPinCountMin = 100,
	IOPinCountMax = 500,
};
```

Older k-style constant definitions should be __avoided__.

__Not Preferred:__

``` objc
enum GlobalConstants {
	kMaxPinSize = 5,
	kMaxPinCount = 500,
};
```

## Case Statements

Braces are not required for case statements, unless enforced by the complier.
When a case contains more than one line, braces should be added. In this case, all other `case`s should contain braces too keep the `switch` consistent.
The line containing the `break` statement should be separated by one newline from the rest of the `case`s body.

``` objc
switch (condition) {
	case 1: {
		// ...

		break;
	}
	case 2: {
		// ...
		// multi-line example using braces

		break;
	}
	case 3: {
		// ...

		break;
	}
	default: {
		// ...

		break;
	}
}

```

There are times when the same code can be used for multiple cases, and a fall-through should be used. A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value. A fall-through should be commented for coding clarity.

``` objc
switch (condition) {
	case 1:
		// fall-through
	case 2:
		// code executed for values 1 and 2

		break;
	default:
		// ...

		break;
}

```

When using an enumerated type for a switch, 'default' is not needed. For example:

``` objc
IOLeftMenuTopItemType menuType = IOLeftMenuTopItemMain;

switch (menuType) {
	case IOLeftMenuTopItemMain:
		// ...

		break;
	case IOLeftMenuTopItemShows:
		// ...

		break;
	case IOLeftMenuTopItemSchedule:
		// ...

		break;
}
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `IOPrivate` or `private`) should never be used unless extending another class. The Anonymous category can be shared/exposed for testing using the <headerfile>+Private.h file naming convention.

``` objc
@interface IODetailViewController ()

@property (strong, nonatomic) IBOutlet;

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

## Booleans

Objective-C uses `YES` and `NO`. Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code. Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

__Preferred:__

``` objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

__Not Preferred:__

``` objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // never do this.
if (isAwesome == true) {} // never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

``` objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, even more dangerous defect may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

__Preferred:__

``` objc
if (!error) {
	return success;
}
```

__Not Preferred:__

``` objc
if (!error)
	return success;
```

or

``` objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability. If the variable being compared is a boolean type, then no parentheses are needed.

__Preferred:__

``` objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

__Not Preferred:__

``` objc
result = a > b ? x = c > d ? c : d : y;
```

## Init Methods

Init methods should follow the convention provided by Apple's generated code template. A return type of 'instancetype' should also be used instead of 'id'.

``` objc
- (instancetype)init {

	self = [super init];

	if (self) {
		// ...
	}

	return self;
}
```

## Class Constructor Methods

Where class constructor methods are used, these should always return type of 'instancetype' and never 'id'. This ensures the compiler correctly infers the result type.

``` objc
@interface Airplane

+ (instancetype)airplaneWithType:(IOAirplaneType)type;

@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

__Preferred:__

``` objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

__Not Preferred:__

``` objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path. That is, don't nest `if` statements. Multiple return statements are OK.

__Preferred:__

``` objc
- (void)someMethod {

	if (![someOther boolValue]) {
		return;
	}

	// something important
}
```

__Not Preferred:__

``` objc
- (void)someMethod {

	if ([someOther boolValue]) {
		// something important
	}
}
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

__Preferred:__

``` objc
NSError *error;

if (![self trySomethingWithError:&error]) {
	// handle error
}
```

__Not Preferred:__

``` objc
NSError *error;

[self trySomethingWithError:&error];

if (error) {
	// handle error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
``` objc
+ (instancetype)sharedInstance {

	static id sharedInstance = nil;
	static dispatch_once_t onceToken;

	dispatch_once(&onceToken, ^{
		sharedInstance = [self new];
	});

	return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](https://github.com/boredzo/Warnings-xcconfig/wiki/Warnings-Explained) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

## Credits

Inspired from these sources:
- [The official raywenderlich.com Objective-C style guide (62ef178)](https://github.com/raywenderlich/objective-c-style-guide/commit/62ef178f64066895e7072dc2eccd6b65ca5b6496)
- [Robots & Pencils Objective-C Style Guide (3858357)](https://github.com/RobotsAndPencils/objective-c-style-guide/commit/3858357f5e24d9b84bf627a4681075c6362417cc)
- [NYTimes Objective-C Style Guide (fd23784)](https://github.com/NYTimes/objective-c-style-guide/commit/fd23784e918a60a3fdc77855fc1d4ff5c0a4b4ac)
- [ustwo™ Objective-C Style Guide (839dcff)](https://github.com/ustwo/objective-c-style-guide/tree/839dcff75e14b2b036da7ac5b50a0fd3f794ed6b)
