title: AVM2概述
categories: 技术
date: 2016-09-22 23:38:36
tags:
---

### 虚拟机结构 ###

#### 常量值 ####

这些值直接出现在ABC文件或者指令编码里。
这些类型的重要特征：
##### int #####

这种类型是用来表示一个整数值数。32位符号二进制补码整数值的范围是-2,147,483,648~2,147,483,647(-2^31~2^31-1)，全包含。

##### uint #####

32位无符号二进制补码整数。0~4,294,967,296(2^32)，全包含。

##### double #####

64位双精度浮点数IEEE754中指定的值作为二进制浮点运算IEEE标准。

##### string #####

Unicode字符序列。用UTF-8表示且能只要2^30-1字节长。

##### namespace #####

名称空间绑定URI（内部代表一个字符串）特征。单向关系，即只包含一个URI命名空间数据类型。每个命名空间都是一种特殊类型，且是关于特征和类型之间关系的限制。

##### null #####

一个单值表示“没有对象”。

##### undefined #####

一个单值表示“没有意义的值”。这个常数值只允许在特定环境。

AVM2利用几个表征值在指令编码和ABC文件为了提供所需尽可能紧凑的编码。
<!-- more -->

#### 虚拟机概述 ####

AVM2中计算是基于执行方法数据环境里的方法体代码，一个本地数据区， 一个常量池，一个针对运行时创建的非基础数据对象的堆，和一个运行时环境。许多数据元素是静态且从ABC文件在启动时读取。
-   方法体代码由指令组成，每条指令用一些方式修改机器状态，或会依靠输入或输出影响外部环境。
-   方法数据决定怎么使用方法。例如，当方法调用时，默认参数如何替代缺失参数。
-   方法的本地数据区包括操作数堆栈，范围堆栈和本地寄存器。
    -   操作数堆栈包含操作数的指令和接收他们的结果。参数被压入堆栈顶部或从栈顶弹出。顶部元素地址总为0，它下面的地址为1，依此类推。堆栈地址不使用除非作为一个明确说明机制。
    -   范围堆栈是运行时环境的一部分，且持有对象当调用名字查找指令执行时被AVM2搜索。指令把元素压进范围堆栈作为异常处理实现的一部分，闭合创建和ActionScript3.0的with声明。
    -   本地寄存器持有参数值，某些情况本地变量和临时变量。
-   常量池拥有常量值最终由指令流引用：数字，字符串和各种各样的名字。
-   指令和AVM2能在运行时创建新对象，这些对象分配在堆中。访问堆唯一的方法是通过分配在里面的一个对象。在堆中不再需要的对象最终由AVM2收回。
-   运行时环境逻辑上包含的对象链与命名这些对象的属性是在运行时查找名字期间发现的位置。名字查找延伸自最里面（最近压入）范围前进到最外层（全局）范围。
    创建方法闭合会导致当前创建的时候运行时环境在闭合里被捕获；当调用之后闭合，当前产生的范围，将由方法体内的代码扩展。

#### Names ####

名字在AVM相当于一个绝对名字和一个或多个命名空间的组合。这些统称为multinames。Multiname条目通常包含一个名字索引和一个命名空间或者命名空间集索引。一些multiname可以有名字和/或命名空间部分在运行时解析。有许多不同类型的multiname如下所述。对象属性总是命名为简单的QName（一对名称和命名空间）。其他类型的multiname用于解析运行时属性。
RTQName，RTQNameL和MultinameL被统称为运行时multiname。

##### QName（Qualified Name） #####

这是multiname的最简单形式。带有确切一个命名空间的名称，因此QName作为保留名称。QName条目会有一个名称索引，后跟一个命名空间索引。名称索引是字符串常量池里的一个索引，而命名空间索引是命名空间常量池里的一个索引。
QName通常用于表示变量名称和类型声明。
```
public var s:String;
```
这段代码将产生两个QName条目，一个用于变量s（public命名空间，名称"s"），一个用于字符串类型（public命名空间，名称"String"）。

##### RTQName(Runtime Qualified Name) #####

这是一个运行时QName，命名空间直到运行时才解析。RTQName条目只有名称索引，索引到字符串常量池。命名空间是在运行时确定的。当RTQName的操作数是一个操作码，应该有一个命名空间值在堆栈上。所以当使用RTQName，堆栈顶部的值将弹出作为RTQName的命名空间。RTQName通常用于命名空间在编译时未知的限定名。
```
var ns = getANamespace();
x = ns::r;
```
这个代码将为ns::r产生RTQName条目。它将有一个名称“r”和代码产生ns的值压入堆栈。

##### RTQNameL(Runtime Qualified Name Late) #####

这是一个运行时QName，在运行时解析名称和命名空间。当RTQNameL是一个操作码的操作数，将会有一个名称和命名空间的值在堆栈。这个名称在堆栈上必须为字符串类型，且命名空间在堆栈上必须为命名空间类型。RTQNameL通常用在编译时名称和命名空间两个都不知道的限定名。
```
var x = getAName();
var ns = getANamespace();
w = ns::[x];
```
这个代码将为ns::[x]在常量池里产生RTQNameL条目。它既没有名称也没有命名空间，代码将生成ns和x的值压入堆栈。

##### Multiname（Multiple Namespace Name) #####

这是一个带有名称和命名空间集的multiname，命名空间集用来表示命名空间集合，Multiname条目将有一个名称索引紧跟一个命名空间集索引。名称索引是字符串常量池里的一个索引，命名空间集索引是命名空间集常量池里的一个索引。
Multiname通常用于非限定名称。在这些情况所有打开命名空间为multiname所用。
```
use namespace t;
trace(f);
```
这个代码将为f生成Multiname条目，它将有一个名称“f”和一个所有打开的命名空间的命名空间集（公共命名空间，命名空间t和在环境中打开的任何私有或者内部命名空间）。在运行时f可以解析成任何multiname指定的命名空间。

##### MultinameL(Multiple Namespace Name Late) #####

这是一个名称在运行时解析的运行时multiname。命名空间集用于表示命名空间集合。MultinameL条目有一个命名空间集索引。命名空间集索引是命名空间集常量池里的一个索引。当MultinameL是一个操作码的操作数时将会有一个名称值在堆栈上。堆栈上的名称值必须为字符串类型。
MultinameL通常用于名称在编译时未知的非限定名称。
```
use namespace t;
trace(o[x]);
```
这个代码将产生一个MultinameL条目。它没有名称，将有一个对环境里所有打开的命名空间的命名空间集。代码将生成x的值压入堆栈，这个值将用作名称。

##### 解析Multiname #####

通常，解析multiname的搜索排序是对象的声明特征，它的动态属性与最终原型链。动态属性和原型链搜索只发生在multiname包含公共命名空间（在AS3里动态属性总是在公共命名空间里；如果尝试增加非公有属性会示意一个运行时错误）。如果一个搜索不包括一个或多个这些位置，在接下来的章节文本中指出。否则你可以在排序中假设三个搜索来解析multiname。
如果multiname是任何类型的QName，那么QName将解析成相同名称和命名空间的属性的QName。如果没有属性有相同名称和命名空间的QName，QName解析不了。
如果multiname有一个命名空间集，那么对象搜索任何名称和multiname名称相同且命名空间匹配multiname命名空间集里的任何命名空间的属性。由于multiname可能有超过一个的命名空间，可以有多个匹配multiname的属性。如果多个属性匹配，由于multiname引用的属性歧义抛出一个TypeError。如果没有属性匹配，那么这对象的multiname解析不了。

#### 方法调用注意 ####

AVM2中调用一个方法时，第一个参数总是方法中使用的“this”值。所有方法都至少需要1个参数（“this”值），其次是任意声明的参数。
当请用[call]属性时，不同类型的闭包行为不同。一个闭包是一个包含方法引用的对象，[call]属性动作不同取决于它是一个函数，方法或者类闭包。一个函数闭包是一个全局方法，不与任何一个类的实例相关联。一个方法闭包包含一个类的实例方法，而且永远记住它的原始“this”值。
```
function f(){}
var a=f; //a is a function closure
class C{
  function m(){}
  }
var q = new C();
var a = q.m; //a is a method closure
```
如果闭包是一个函数闭包，那么第一个参数传递给[call]是传递给方法并被用作“this”值。如果第一个参数是null或者undefined，那么全局对象将被用作方法的“this”值。
如果闭包是一个方法闭包，那么第一个参数传递给[call]将被忽略，方法闭包保存的“this”值将传递给方法作为第一个参数。一个方法闭包记录它的原始“this”值是什么并总是用来代替[call]的第一个参数。

如果闭包是类闭包，且1个参数传递给[call]（除了“this”参数），那么调用被作为类型转换对待，参数将被强制作为闭包所代表的类型。

#### 指令集概要 ####

本节提供AVM2指令集概述。按照惯例，指令具体数据类型细节被命名带一个后缀，指出操作的数据类型。具体来说，使用下面的后缀：_b(Boolean),_a(any),_i(int),_d(double),_s(string),_u(unsigned),_o(object)。

##### 加载并存储指令 #####

本地寄存器能使用下面指令访问：**getlocal**,**getlocal0**,getlocal1,getlocal2,getlocal3,setlocal,setlocal0,setlocal1,setlocal2,setlocal3。

##### 自述指令 #####

算术指令提供完整的数学运算目录。零个，一个或多个通常两个操作数从栈顶移除并将操作结果压回操作栈。
加法使用下面之一运行：increment,increament_i,inclocal,inclocal_i,add,add_i。
减法使用下面达成：decrement,decrement_i,declocal,declocal_i,subtract,subtract_i。
乘法和除法用multiply,multiply_i,divide和modulo实现。为了反转值的符号，可以用negate或者negate_i指令。
这里也存在指令集执行栈顶两个条目值比较，用true或者false替代他们。这些包括equals,strictequals,lessthen,lessequals,greaterthan,greaterequals,istype,istypelate和in。

##### 位操作指令 #####

指令允许值的位操作包括bitnot,bitand,bitor,bitxor,lshift,rshift,urshift。
优先执行这些指令，要操作的值转换为一个整数，如果必要的。

##### 类型转换指令 #####

as语言是弱类型语言，对象自由转换成要求完成一个操作所需的任何类型。在某些情况需要明确的转换，这种实例提供coerce指令。这些包括coerce,convert_b,coerce_a,convert_i,convert_d,coerce_s,convert_s,convert_u,convert_o。

##### 对象创建与控制指令 #####

实体使用下面指令之一创建：newclass,newobject,newarray,newactivation。
要求调用一个对象构造指令使用construct,constructsuper,constructprop。

命名空间能使用dxns和dxnslate动态构造。

##### 栈管理指令 #####

多数指令提供直接访问操作放在堆栈的值。直接压入值的指令包括pushnull,pushundefined,pushtrue,pushfalse,pushnan,pushbyte,pushshort,pushstring,pushint,pushdouble,pushscope,pushnamespace。
一个值能使用pop从栈移除，同时dup复制栈顶的值，类似的swap交换栈顶的两个值。

##### 控制转移指令 #####

控制转移指令转移执行一个指令除了立即跟在转移指令后的一个。转移能无条件或者指令基于一个隐式比较操作。
条件分支指令包括iflt,ifle,ifnlt,ifnle,ifgt,ifge,ifngt,ifnge,ifeq,ifne,ifstricteq,ifstrictne,iftrue,iffalse。这些指令为了实现比较运行任何所需的类型转换；转换规则在ECMA-262概述。
label指令用来标记向后分支指令的目标位置点。因此每个向后分支指令的目标位置应基于label指令。
lookupswitch指令提供简洁形式编码多种方法比较表达式。

##### 函数调用与return指令 #####

这里有多个指令调用函数和方法。call指令实现完全符合ECMA-262规范演绎Function.prototype.call。调用对象实例方法可用callmethod指令。同样的调用类，也知道静态方法，存在callstatic。要调用不在对象上而在它的基类的实例方法使用callsuper。对于作为方法调用并as编译器可以认可这种用例的名称元素，callproperty和callproplex可用。后者是针对属性被调用存在栈上的对象。对于调用的返回值从不使用的情况，callpropvoid和callsupervoid能各自被使用在callproperty和callsuper的地方。

##### 异常指令 #####

一个异常以编程方式抛出使用throw指令。当碰到一个异常的条件时异常也能由各种AVM指令抛出。
try/catch语句在as语言被翻译进一个间隔表与在abc文件指定方法体部分的目标指令。这个表定义了指令遍及的范围，哪些给定的异常类型可能被捕获。因此如果在运行给定的指令集期间一个异常抛出且在异常表里有相关的条目，程序执行将在表里指定的目标指令继续。

##### 调试指令 #####

不像大多传统运行环境，AVM2的调试场所与直接放在运行流里的一系列指令紧紧相关联。跟踪当前文件名和行号信息debugfile和debugline在指令流的合适点发出。在需要额外的调试细节的情况下，使用调试指令。例如，本地变量的名称由这个机制提供。

### AS字节码格式 ###

as代码语法完整部分由编译器处理进as字节码片段。这些片段由下面定义的abc文件结构描述。abc文件结构由AVM2加载和运行单元使用。abc文件结构描述8位字节块解释。尽管abc文件的名字，内容不需要从文件系统里的文件读取；它能由运行时编译器或者其他工具动态生成。“文件”这个词的使用是历史的。
abc文件结构包含原始数据，结构化数据和原始与结构化数据数组。以下部分描述所有数据格式。原始数据以各种方式包括整数和浮点数编码。结构数据，包括abc文件本身，在这里使用一个c结构符号，与单独的命名字段。字段在结构里实际上只是根据它们类型解释的字节序列。顺序存储的字段没有任何填充或对齐。

#### 原始数据类型 ####

多字节原始数据用little-endian顺序存储（较小的有效位先于更大的有效位）。负的整数用二进制补码表示。
-    u8类型表示单字节无符号整数值。
-    u16类型表示双字节无符号整数值。
-    s24类型表示3字节符号整数值。
-    u30类型表示变长编码30位无符号整数值。
-    u32和s32各自表示变长编码32位无符号和符号整数值。
-   d64类型定义一个8位IEEE-754浮点值。double值的高位包含符号和上层指数，低位包含有效位最小有效位。

变长编码对于u30,u32和s32使用1到5字节，依据值编码大小。每个字节贡献它的低7位给值。如果字节高位（第八）设置了，那么abc文件的下个字节也是值的一部分。在s32的情况，应用符号扩展：编码的最后字节第七位是传送填充编码值的32位。

#### abcFile ####

abcFile结构描述一个运行代码块带有所有它的常量数据，类型描述，代码和元数据。由以下字段构成。
minor_version，major_version
major_version和minor_version的值是abcFile格式的主要和次要版本号。次要版本号改变表示文件格式改变向后兼容，意义在于一个AVM2的实现还能用于一个旧版本文件。一个主要版本号改变表示一个不兼容对文件格式的调整。
当这个概述出版时，主要版本号是46，次要版本号是16.

constant_pool
constant_pool是一个变长结构由整数，双精度浮点数，字符串，命名空间，命名空间集和multiname构成。这些常量引用自abcFile结构的其他部分。

method_count, method
method_count的值是在method数组里条目的数量。method数组的每个条目是一个变长method_info结构。数组持有这个abcFile里关于每个方法的信息。方法体代码在method_body数组里分开保存。在方法里的一些条目可能没有方法体——这是本地方法的情况，如例。

metadata_count, metadata
metadata_count的值是metadata数组里的条目数量。每个元数据条目是一个关联名称到字符串值集的metadata_info结构。

class_count, instance, class
class_count的值是在instance和class数组里的条目数量。
每个实例条目是一个变长instance_info结构，详细说明由特定类创建的对象实例的特征。
每个class条目定义一个类的特征。它被用在结合instance字段获得一个AS类的完整描述。

script_count, script
script_count的值是Script数组条件的数量。每个script条目是一个script_info结构，定义这个文件单个脚本的特征。作为上个章节的解释，这个数组最后的条目是abcFile运行的入口点。

method_body_count, method_body
method_body_count的值是method_body数组里条目的数量。每个method_body条目含有一个变长method_body_info结构，包含一个单独的方法或函数的指令。

#### 常量池 ####

常量池是一个基于数组条目的块，反射由所有方法使用的常量。每个计算条目（例如，int_count）必须超过对应数组条目数量，且数组的首个条目是元素“1”。对于所有常量池，索引“0”拥有特殊的意义，典型的一个实用的默认值。例如，“0”条目被用来表示空字符串（""），任何命名空间或者任何类型（*）依据使用的环境。当“0”拥有一个特殊的意思，在下面的文本描述。
如果对于相同实体在这些数组中有多个条目，例如一个名字，AVM可能会或者可能不会认为这是两个相同的条目。当前AVM保证名称标记为属于“private”命名空间被作为唯一的对待。
int_count, integer
int_count值是在interger数组里的条目数量，加一个。interger数组持有整数常量由字节码的引用。interger数组的“0”条目在abcFile是不存在的；它为了给可选参数和字段初始化提供值的目的表达为0值。
unit_count, uinterger
同上
double_count,double
同上，但表达为NaN(Not-a-Number)
string_count,string
string_count值是在String数组里的条目数量，加一个。string数组持有由编译代码和abcFile的多个其他部分引用的UTF-8编码字符串。除了在程序里描述字符串常量，常量池里的字符串数据被用于多种的名称的描述。string数组的“0”条目在abcFile里是不存在的；在大多数环境它相当于空字符串，但是在其他环境也用来表示“any”名称（在as里作为“*”）。
namespace_count,namespace
同上，“0”不存在，表示“any"
ns_set_count,ns_set
同上，“0”不存在。
nultiname_count,multiname
同上，“0”不存在。

#### String ####

string_info元素以长度和数据格式编码一个16位字符的字符串。每个字符的含义通常被当作是16位Unicode代码点。数据是UTF-8编码。对于Unicode的更多信息，查看unicode.org。

##### Namespace #####

namespace_info条目定义一个命名空间。命名空间有由字符串数组索引表示的字符串名称和类型。自定义命名空间有类型CONSTANT_Namespace或者CONSTANT_ExplicitNamespace和一个非空名称。系统命名空间有空名称和其他类型之一，且为加载器提供一个意思映射引用到内部实体的这些命名空间。

一个单字节定义以下条目的类型，因此由加载器识别name字段应该怎么解释。name字段是常量池的字符串部分里的一个索引。一个0值表示一个空字符串。下面的表格列出合法的类型值。

##### Namespace set #####

一个ns_set_info条目定义一个命名空间集，允许集合作为multiname的定义里的一个单元使用。
count字段定义这个条目多少ns被标识，然而每个ns是一个整数，索引常量池内的namespace数组。ns数组里没有条目可能是0。

##### Multiname #####

一个multiname_info条目是一个变长项，用来定义由字节码使用的multiname实体。这里有multiname的多种类型。kind字段当作一个标签：它的值决定加载器怎么查看变长data字段。data字段的内容设计基于独有的kind是下面描述的multiname_kind_结构。

以“A”结尾的常量（如CONSTANT_QNameA）表示名称的属性

###### QName ######

multiname_kind_QName格式用于CONSTANT_QName和CONSTANT_QNameA类型。

ns和name字段被各自索引到constant_pool条目的namespace和string数组。ns字段为零值指示任何（“*”）命名空间，且name字段为零值指示任何（“*”）名称。

###### RTQName ######

nultiname_kind_RTQName格式用于CONSTANT_RTQName和CONSTANT_RTQNameA类型。

单个字段，name，是常量池的string数组里的一个索引。一个零值表示任何（“*”）名称。

###### RTQNameL ######

multiname_kind_RTQNameL格式用于CONSTANT_RTQNameL和CONSTANT_RTQNameLA类型。

类型没有关联数据。

###### Multiname ######

multiname_kind_Multiname格式用于CONSTANT_Multiname和CONSTANT_MultinameA类型。

name字段是string数组里的一个索引，ns_set字段是ns_set数组里的一个索引。name字段为0值指示任何（“*”）名称。ns_set的值不能为0。

###### MultinameL ######

multiname_kind_MultinameL格式用于CONSTANT_MultinameL和CONSTANT_MultinameLA类型。

ns_set字段是常量池ns_set数组里的一个索引。ns_set的值不能为0。

#### 方法签名 ####

method_info条目定义单一方法的签名。

param_count, param_type
param_count字段是方法支持的正式参数数量；它也表示param_type数组的长度。param_type数组的每个条目是常量池的multiname数组里的一个索引；条目的名称提供相应正式参数的类型的名称。0值表示任何（“*”）类型。

return_type
return_type字段是常量池的multiname数组里的一个索引；条目的名称提供方法返回类型的名称。0值表示任何（“*”）类型。

name
name字段是常量池的string数组里的一个索引；条目的字符串提供这个方法的名称。如果索引为0，方法没有名字。

flags
flag字段是一个位向量，提供方法相关的额外信息。这个位由下面的表格描述。（表格里没有描述的位都设置为0）

options
条目只有在flags里设置了HAS_OPTIONAL标志时出现。

param_names
条目只有在flags里设置了HAS_PARAM_NAMES标志时出现。

##### 可选参数 #####

option_info参数用来定义方法可选参数的默认值。可选参数数目由option_count获取，必须不为零也不大于闭合的method_info结构的parameter_count字段
每个可选值由kind字段构成，表示值表现的类型，val字段是一个常量池的数组条目之一的索引。正确的数组是基于类型的选择。

##### 参数名字 #####

param_name条目只有当在flags里设置了HAS_PARAM_NAMES位时可用。每个param_info的数组元素是一个常量池的string数组的索引。参数名字条目对外部工具使用单独存在且不由AVM2使用。

#### metadata_info ####

metadata_info条目提供一个嵌入任意键/值对到ABC文件的方法。AVM2将忽略所有这种条目。

name字段是常量池string数组里的一个索引；它为元数据条目提供一个名称。名称字段的值必须不为0。0或更多的项可能与条目相关联；item_count表示按照items数组的项目数。

item_info条目由item_count元素构成，作为索引到常量池string表的键/值对解释。如果key的值为0，这是一个无键的条目且只传递一个值。

#### Instance  ####

instance_info条目是用于定义AVM2里一个运行时对象（类实例）的特征。相应的class_info条目用于完整定义一个as3类。

name
name字段是常量池的multiname数组里的一个索引；它为类提供一个名称。指定的条目必须为QName。

super_name
super_name字段是常量池的multiname数组里的一个索引；它提供这个类的基类的名称，如果有的话。一个零值表示这个类没有基类。

flags
flags字段是用于当解释instance_info条目时定义各种选项。它是位向量；定义以下条目。其他位和必须为0。

protectedNs
字段只有在flags的CONSTANT_ProtectedNs位设置时出现。它是常量池的namespace数组的一个索引且作为这个类的保护命名空间识别命名空间。

intrf_count, interface
intrf_count字段的值是interfac数组里的条目数。interface数组包含常量池里的multiname数组的目录；引用名称指定这个类的接口实现。没有索引可能为0。

iinit
这是abcFile的method数组里的一个索引；它引用方法无论何时这个类对构造调用。这个方法有时被称为初始化一个实例。

trait_count, trait
trait_count值是在trait数组里的元素数。trait数组定义类实例的特征集。下一部分定义traits_info结构的方法。

#### Trait ####

一个特征是一个对象或类的确定属性；它拥有一个名称，类型和一些关联数据。traits_info结构绑定这些数据

name
名称字段是常量池的multiname数组的一个索引；它为特征提供一个名称。值不能为0，且multiname条目指定必须为一个QName。

kind
kind字段包含两个4位字段。低4位决定这个特征的类型。高4位由一个位向量构成提供特征的属性。参见以下表格和完整描述部分。

data
data字段的解释取决于特征的类型，由kind字段的低4位提供。参考以下完整描述。

metadata_count, metadata




