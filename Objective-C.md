#格式

##代码缩进
代码缩进使用4个空格字符。

##行宽
一行代码尽量控制在80个字符内。这不是强制要求，只要不影响阅读即可。

##方法的定义
\- 和 + 后留一个空格，方法参数名前留一个空格。方法第一个 {  和方法最后一个参数在同一行， { 前留一个空格。参数名必须写，不留多余的空格。

	- (void)doSomethingWithString:(NSString *)theString {
		...
	}

如果参数过多，一行内写不完时，每个参数应该独占一行，并保持冒号对齐。

	- (void)doSomethingWith:(GTMFoo *)theFoo
	                   rect:(NSRect)theRect
	               interval:(float)theInterval {
	  ...
	}

当第一个参数名太短时，后面的参数需要有适当的缩进（至少4个空格）。

	- (void)short:(GTMFoo *)theFoo
	          longKeyword:(NSRect)theRect
	    evenLongerKeyword:(float)theInterval
	                error:(NSError **)theError {
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
protocol名称和类型之间不留空格。

	@interface MyProtocoledClass : NSObject<NSWindowDelegate> {
	    id<MyFancyDelegate> _delegate;
	}
	- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;
	@end

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

	NSInteger addSum(NSInteger firstNumber, NSInteger lastNumber) {
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

--------------------

#代码

##autorelease
一个对象需要放进 autorelease pool 的话，应该在创建的时候调用 autorelease 方法。

	MyController* controller = [[[MyController alloc] init] autorelease];

##setter 方法内对 NSString 参数进行copy

	- (void)setFoo:(NSString *)aFoo {
	  [_foo autorelease];
	  _foo = [aFoo copy];
	}

##BOOL 类型

如果方法返回值是 BOOL 类型，必须保证返回值是 BOOL 类型的值或者是 &&, ||, ! 或比较运算符的结果。

	- (BOOL)isBold {
	    return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
	}
	- (BOOL)isValid {
	    return [self stringValue] != nil;
	}
	- (BOOL)isEnabled {
	    return [self isValid] && [self isBold];
	}

不要和 YES 进行比较。

	BOOL great = [foo isGreat];
	if (great) {
	    //great
	}
