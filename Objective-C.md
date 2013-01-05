#格式

###代码缩进
代码缩进使用4个空格字符。

###行宽
一行代码尽量控制在80个字符内。这不是强制要求，只要不影响阅读即可。

###方法的定义
\- 和 + 后留一个空格，方法参数名前留一个空格。参数名必须写，不留多余的空格。

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

###方法调用
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

###protocol
protocol名称和类型之间不留空格。

	@interface MyProtocoledClass : NSObject<NSWindowDelegate> {
	    id<MyFancyDelegate> _delegate;
	}
	- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;
	@end

--------------------

#命名

###类名
类名必须以大写字母开头，

#代码

