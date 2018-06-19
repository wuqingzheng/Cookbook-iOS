## Technical Q&A QA1669 - Improved logging in Objective-C

Q：如何将上下文信息（例如当前方法或行号）添加到我的日志记录语句中？
A：C 预处理器提供了许多标准宏，可以为您提供有关当前文件、行号或函数的信息。 此外，Objective-C 具有 `_cmd` 隐式参数，它给出当前方法的选择器，以及将选择器和类转换为字符串的函数。您可以在 `NSLog` 语句中使用这些语句在调试或错误处理过程中提供有用的上下文。

清单 1 记录当前方法和行号的示例。将其粘贴到您的项目中，并查看它打印的内容！

```
NSMutableArray *someObject = [NSMutableArray array];
NSLog(@"%s:%d someObject=%@", __func__, __LINE__, someObject);
[someObject addObject:@"foo"];
NSLog(@"%s:%d someObject=%@", __func__, __LINE__, someObject);
```

下表列出了在日志语句中可能有用的最常用的宏和表达式。

表 1 C/C++/Objective-C 用于记录日志的预处理器宏。

Macro|Format Specifier|Description  
-|:-:|:-:  
`__func__`|%s|Current function signature.
`__LINE__`|%d|Current line number in the source code file.
`__FILE__`|%s|Full path to the source code file.
`__PRETTY_FUNCTION__`|%s|Like `__func__`, but includes verbose type information in C++ code.

表 2 Objective-C 中用于记录日志的表达式。

Expression|Format Specifier|Description  
-|:-:|:-:  
`NSStringFromSelector(_cmd)`|%@|当前 selector 的名称
`NSStringFromClass([self class])`|%@|当前对象的类的名称
`[[NSString stringWithUTF8String:__FILE__] lastPathComponent]`|%@|源代码文件的名称
`[NSThread callStackSymbols]`|%@|作为程序员可读的字符串当前堆栈踪迹的数组。仅用于调试，不要将其展示给最终用户或用于在程序中执行任何逻辑。