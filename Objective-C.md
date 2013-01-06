#格式

##代码缩进
代码缩进使用4个空格字符。

##行宽
一行代码尽量控制在80个字符内。这不是强制要求，只要不影响阅读即可。

##方法的定义
\- 和 + 后留一个空格，方法参数名前留一个空格。方法和函数的第一个 {  单独占一行。其他情况下 { 前留一个空格，不单独占一行。参数名必须写，不留多余的空格。

	- (void)doSomethingWithString:(NSString *)theString
	{
		...
	}

如果参数过多，一行内写不完时，每个参数应该独占一行，并保持冒号对齐。

	- (void)doSomethingWith:(GTMFoo *)theFoo
	                   rect:(NSRect)theRect
	               interval:(float)theInterval
	{
	  ...
	}

当第一个参数名太短时，后面的参数需要有适当的缩进（至少4个空格）。

	- (void)short:(GTMFoo *)theFoo
	          longKeyword:(NSRect)theRect
	    evenLongerKeyword:(float)theInterval
	                error:(NSError **)theError
	{
	  ...
	}

##方法调用
如果能在一行内写完则把所有参数写在同一行，不留多余的空格。

	[myObject doFooWith:arg1 name:arg2 error:arg3];

如果参数过多，一行内写不完时，每个参数应该独占一行，并保持冒号对齐。

	[myObject doFooWith:arg1
	               name:arg2
	              error:arg3];

当第一个参数名太短时，后面的参数需要有适当的缩进（至少4个空格）。

	[myObj short:arg1
	          longKeyword:arg2
	    evenLongerKeyword:arg3
	                error:arg4];

##protocol
protocol 的名称和类型之间不留空格。

	@interface MyProtocoledClass : NSObject<NSWindowDelegate>
	{
	    id<MyFancyDelegate> _delegate;
	}
	- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;
	@end

所有 protocol 都要实现 NSObject 协议。

	@protocol SomeControllerDelegate <NSObject>
	
	- (void)someController:(SomeController *)controller didSomethingWithThisObject:(id)object;

所有 protocol 声明的方法都必须使用 @require, @optional 声明方法是否必须。

	@protocol SomeControllerDelegate  <NSObject>
	
	@required
	- (void)someController:(SomeController *)controller didSomethingWithThisObject:(id)object;
	
	@optional
	- (void)someControllerDidSomethingElse:(SomeController *)controller;
	
	@end



##指针
指针变量的 * 和变量名之间不留空格，* 和类型名之间留一个空格

	- (ReturnClass *)methodName:(ParamClass1 *)param1 another:(ParamClass2 *)param2
	{
	    SomeOtherClass *variable = [[SomeOtherClass alloc] init];
	    return nil;
	}

##其他情况
函数返回值类型后留一个空格。

函数后面紧跟的括号之间不留空格。

函数参数列表的逗号要紧跟前一个参数，逗号后留一个空格。

for, while 后面紧跟的括号前要留一个空格。

for的括号内 ; 前面不留空格，后面留一个空格。

赋值操作 = ， 二元操作符前后都要留一个空格。

一元操作符和操作数之间不留空格。每一行最后不留空白字符。

每一个文件最后一行必须是空行。

具体参考以下代码格式：

	NSInteger addSum(NSInteger firstNumber, NSInteger lastNumber)
	{
	    int sum = 0;
	    for (NSInteger index = firstNumber; index <= lastNumber; index++) {
	        sum += index;
	    }
	    return sum;
	}

--------------------

#命名

##类名
类名必须以大写字母开头，单词之间不使用下划线分隔，每个单词首字母大写，其他字母小写。如果单词是大家都明白的缩写（例如 HTML ），则单该词内所有字母都用大写。

##方法名
命名规则和类名一致，除了第一个单词首字母要小写。如果第一个单词是大家都明白的缩写（例如 HTML ）,这个单词内所有字母都用大写。方法名不要下划线开头。

##实例变量名
不使用匈牙利命名法。名称只需要表达出必要的意思即可。

--------------------

#注释

代码中的注释提到变量名称时，变量名称使用 | 加以标识。

	// Sometimes we need |count| to be less than zero.

只有多行注释才使用 /\* \*/ 。

	/* 
	 * Multi-line comments should be used whenever a comment spans
	 * multiple lines, like this one.
	 * Be sure the stars line up in a nice column, and that the end wing is on its
	 * own line.
	 * Also make sure you use a space after the column of stars.
	 */

C函数调用时可以适当地增加单行注释。

	unsigned len = [str length];
	//Count UTF-8 code units (may be more than the number of Unicode code-points)
	CFIndex UTF8len;
	CFStringGetBytes((CFStringRef)str,
		CFRangeMake(0, len),
		kCFStringEncodingUTF8,
		/*lossByte*/ 0U,
		/*isExternalRepresentation*/ false,
		/*buffer*/ NULL,
		len,
		&UTF8len
		);

--------------------

#代码

##autorelease
一个对象需要放进 autorelease pool 的话，应该在创建的时候调用 autorelease 方法。

	MyController* controller = [[[MyController alloc] init] autorelease];

##setter 方法内对 NSString 参数进行copy

	- (void)setFoo:(NSString *)aFoo
	{
	  [_foo autorelease];
	  _foo = [aFoo copy];
	}

##BOOL 类型

如果方法返回值是 BOOL 类型，必须保证返回值是 BOOL 类型的值或者是 &&, ||, ! 或比较运算符的结果。

	- (BOOL)isBold
	{
	    return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
	}
	- (BOOL)isValid
	{
	    return [self stringValue] != nil;
	}
	- (BOOL)isEnabled
	{
	    return [self isValid] && [self isBold];
	}

不要和 YES 进行比较。

	BOOL great = [foo isGreat];
	if (great)
	{
	    //great
	}

不要强制转换一个指针为 BOOL 类型。

##accessors 方法不要以 get 开头

	- (NSString *) name;
	- (NSString *) color;
	name = [object name];
	color = [object color];

##返回值类型是 void 的方法尽可能提前返回，减少需要阅读的代码

	- (void)someMethod
	{
	    if ([someOther boolValue]) {
	        //Do something important
	        return;
	    }
	    //Do something else important
	}

##实例变量

实例变量不要以 _ 开头。需以 _ 结尾。

##property

声明 property 时必须使用 nonatomic，除非你的类是线程安全的。

NSString 类型的 property 必须是 copy 。

property 必须有一个对应的实例变量。

	@interface MyClass : NSObject
	{
	    NSString *myIvar_;
	}
	
	@property (copy) NSString *myIvar;
	
	@end
	
	@implementation MyClass
	
	@synthesize myIvar = myIvar_;
	
	@end

##私有方法

在实现文件里使用 class extension 写该类的私有方法。保证头文件只有对外公开的方法。

头文件

	//
	// Foo.h
	//
	     
	@interface Foo : NSObject
	     
	// Only the public API can be accessed by including the header
	     
	@property(nonatomic) int myPublicProperty;
	     
	- (int)myPublicMethod;
	     
	@end

 实现文件

	//
	// Foo.m
	//
	     
	@interface Foo () // This is a "class extension" and everything declared in it is private, because it’s in the implementation file
	     
	@property(strong, nonatomic) Bar* myPrivateProperty;
	     
	- (int)myPrivateMethod;
	     
	@end
	     
	@implementation Foo
	// …
	@end

##比较和 nil
使用 isEqual: 或 compare: 方法进行比较是，必须检查参数是否为 nil。

##宏

传递参数给宏时，如果参数内有逗号，该参数必须用括号包起来。

	STAssertEqualObjects([obj methodTwo], (@[ @"expected1", @"expected2" ]), @"Didn't get the expected array");

##NSString的长度

NSString 变量的 length 方法返回的字符串长度不一定等于该字符串的字节数。应注意调用的参数意义。

	const char *cStr = [string UTF8String];
	write(fd, cStr, strlen(cStr));

##杂项

不要在头文件写实例变量

不要在 init 和 dealloc 方法内使用 accessor 方法

