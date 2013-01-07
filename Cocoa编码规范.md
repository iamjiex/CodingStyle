（整理自 [http://www.cppblog.com/kesalin/archive/2011/11/03/coding_guideline_of_cocoa.html](http://www.cppblog.com/kesalin/archive/2011/11/03/coding_guideline_of_cocoa.html)）

#代码命名基础

在面向对象软件库的设计过程中，开发人员经常忽视对类，方法，函数，常量以及其他编程接口元素的命名。本节讨论大多数Cocoa接口的一些命名约定。

##一般性原则

###Clarity 清晰性

最好是既清晰又简短，但不要为简短而丧失清晰性：

	代码                              点评
	insertObject:atIndex:            good
	insert:at:                       不清晰；要插入什么？“at”表示什么？
	removeObjectAtIndex:             good
	removeObject:                    这样也不错，因为方法是移除作为参数的对象
	remove:                          不清晰；要移除什么？

名称通常不缩写，即使名称很长，也要拼写完全：

	代码                              点评
	destinationSelection             good
	destSel                          不清晰
	setBackgroundColor:              good
	setBkgdColor:                    不清晰

你可能会认为某个缩写广为人知，但有可能并非如此，尤其是当你的代码被来自不同文化和语言背景的开发人员所使用时。

然而，你可以使用少数非常常见，历史悠久的缩写。请参考：”可接受的缩略名“一节。

避免使用有歧义的 API 名称，如那些能被理解成多种意思的方法名称。

	代码                              点评
	sendPort                         是发送端口还是返回一个发送端口？
	displayName                      是显示一个名称还是返回用户界面中控件的标题？

###Consistency 一致性

尽可能使用与 Cocoa 编程接口命名保持一致的名称。如果你不太确定某个命名的一致性，请浏览一下头文件或参考文档中的范例。

在使用多态方法的类中，命名的一致性非常重要。在不同类中实现相同功能的方法应该具有相同的
名称。

	代码                                    点评
	- (int) tag                            在 NSView, NSCell, NSControl 中有定义
	- (void) setStringValue:(NSString *)   在许多 Cocoa classes 中有定义

请参考“方法参数”一节。

###No Self Reference 不要自我指涉

不要名称自我指涉。

	代码                              点评
	NSString                         okay
	NSStringObject                   自我指涉

掩码（可使用位操作进行组合）和用作通知名称的常量不受该约定限制。

	代码                                      点评
	NSUnderlineByWordMask                    okay
	NSTableViewColumnDidMoveNotification     okay

##Prefixes 前缀

前缀是名称的重要组成部分。它们可以区分软件的功能范畴。通常，软件会被打包成一个框架或多个紧密相关的框架（如 Foundation 和 Application Kit 框架）。前缀可以防止第三方开发者与苹果公司之间的命名冲突（同样也可防止苹果内部不同框架之间的命名冲突）。

前缀有规定的格式。它由两到三个大写字符组成，不能使用下划线与子前缀。

	前缀                              Cocoa 框架
	NS                               Foundation
	NS                               Application Kit
	AB                               Address Book
	IB                               Interface Builder

命名 class，protocol，structure，函数，常量时使用前缀；命名成员方法时不使用前缀，因为方法已经在它所在类的命名空间种；同理，命名结构体字段时也不使用前缀。

##Typographic Conventions 书写约定
在为 API 元素命名时，请遵循如下一些简单的书写约定。

对于包含多个单词的名称，不要使用标点符号作为名称的一部分或作为分隔符（下划线，破折号等）；此外，大写每个单词的首字符并将这些单词连续拼写在一起。请注意以下限制：

方法名小写第一个单词的首字符，大写后续所有单词的首字符。方法名不使用前缀。如：

	fileExistsAtPath:isDirectory:

如果方法名以一个广为人知的大写首字母缩略词开头，该规则不适用，如：NSImage 中的 TIFFRepresentation。

函数名和常量名使用与其关联类相同的前缀，并且要大写前缀后面所有单词的首字符。如：

	NSRunAlertPanel
	NSCellDisabled

避免使用下划线来表示名称的私有属性。苹果公司保留该方式的使用。如果第三方这样使用可能会导致命名冲突，他们可能会在无意中用自己的方法覆盖掉已有的私有方法，这会导致严重的后果。请参考“私有方法”一节以了解私有 API 的命名约定的建议。

##Class and Protocol Names 类与协议命名

类名应包含一个明确描述该类（或类的对象）是什么或做什么的名词。类名要有合适的前缀（请参考“前缀”一节）。Foundation 及 Application Kit 有很多这样例子，如：NSString, NSDate, NSScanner, NSApplication, UIApplication, NSButton, 以及 UIButton。

协议应该根据对方法的行为分组方式来命名。

大多数协议仅组合一组相关的方法，而不关联任何类，这种协议的命名应该使用动名词(ing)，以不与类名混淆。

	代码                              点评
	NSLocking                        good
	NSLock                           糟糕，它看起来像类名

有些协议组合一些彼此无关的方法（这样做是避免创建多个独立的小协议）。这样的协议倾向于与某个类关联在一起，该类是协议的主要体现者。在这种情形，我们约定协议的名称与该类同名。

NSObject 协议就是这样一个例子。这个协议组合一组彼此无关的方法，有用于查询对象在其类层次中位置的方法，有使之能调用特殊方法的方法以及用于增减引用计数的方法。由于 NSObject 是这些方法的主要体现者，所以我们用类的名称命名这个协议。

##Header Files 头文件

头文件的命名方式很重要，我们可以根据其命名知晓头文件的内容。

声明孤立的类或协议：将孤立的类或协议声明放置在单独的头文件中，该头文件名称与类或协议同名。

	头文件                          声明
	NSLocale.h                     NSLocale 类

声明相关联的类或协议：将相关联的声明（类，类别及协议) 放置在一个头文件中，该头文件名称与主要的类/类别/协议的名字相同。

	头文件                          声明
	NSString.h                     NSString 和 NSMutableString 类
	NSLock.h                       NSLocking 协议和 NSLock, NSConditionLock, NSRecursiveLock 类

包含框架头文件：每个框架应该包含一个与框架同名的头文件，该头文件包含该框架所有公开的头文件。

	头文件                          框架
	Foundation.h                   Foundation.framework

为已有框架中的某个类扩展 API：如果要在一个框架中声明属于另一个框架某个类的范畴类的方法，该头文件的命名形式为：原类名+“Additions”。如 Application Kit 中的 NSBundleAdditions.h。

相关联的函数与数据类型：将相联的函数，常量，结构体以及其他数据类型放置到一个头文件中，并以合适的名字命名。如 Application Kit 中的 NSGraphics.h。

#Naming Methods 方法命名

##General Rules 一般性规则

为方法命名时，请考虑如下一些一般性规则：

小写第一个单词的首字符，大写随后单词的首字符，不使用前缀。请参考“书写约定”一节。

有两种例外情况：1，方法名以广为人知的大写字母缩略词（如TIFF or PDF）开头；2，私有方法可以使用统一的前缀来分组和辨识，请参考“私有方法”一节。

表示对象行为的方法，名称以动词开头：

	- (void) invokeWithTarget:(id)target:
	- (void) selectTabViewItem:(NSTableViewItem *)tableViewItem

名称中不要出现 do 或 does，因为这些助动词没什么实际意义。也不要在动词前使用副词或形容词修饰。

如果方法返回方法接收者的某个属性，直接用属性名称命名。不要使用 get，除非是间接返回一个或多个值。请参考“访问方法”一节。

	- (NSSize) cellSize;                对
	- (NSSize) calcCellSize;            错
	- (NSSize) getCellSize;             错

参数要用描述该参数的关键字命名。

	- (void) sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;        对
	- (void) sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;   错

参数前面的单词要能描述该参数。

	- (id) viewWithTag:(int)aTag;            对
	- (id) taggedView:(int)aTag;             错

细化基类中的已有方法：创建一个新方法，其名称是在被细化方法名称后面追加参数关键词。

	- (id) initWithFrame:(NSRect)frameRect;     NSView, UIView
	- (id) initWithFrame:(NSRect)frameRect mode:(ind)aMode cellClass:(Class)factoryId numberOfRows:(int)rowsHigh numberOfColumns:(int)colsWide;          NSMatrix - NSView 的子类

不要使用 and 来连接用属性作参数关键字。

	- (int) runModalForDirectory:(NSString *)path file:(NSString *)name types:(NSArray *)fileTypes;          对
	- (int) runModalForDirectory:(NSString *)path addFile:(NSString *)name addTypes:(NSArray *)fileTypes;          错

虽然上面的例子中使用 add 看起来也不错，但当你方法有太多参数关键字时就有问题。

如果方法描述两种独立的行为，使用 and 来串接它们。

	- (BOOL) openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;         NSWorkspace