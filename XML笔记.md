# XML笔记

[toc]

## XML是什么？

**1.** XML是一种用于**传输和存储数据**的语言。
**2.** 基于这种目的，XML不需要预定义标签。
**3.** XML的标签**完全**由用户自定义，对于处理XML文件的程序而言，每个标签的含义都未知，会一视同仁的处理。
**4.** 也因此，XML中的标签名需要有特定的语义，以免编写者或其他用户看不懂。

## 怎么写XML

1. 在XML文件的开头要有XML声明，如：

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    ```

    XML声明作用是明确该文件的类型、编码，以避免未来处理该文件的程序选择错误的处理方式。
    </br>
2. XML文件的内容要被包含在元素的开始标签和结束标签之间，如下：

    ```xml
    <person>张三</person>
    ```

    尖括号内写元素名，表示该元素的**开始标签**。
    元素名前再加上斜线/，表示该元素的**结束标签**。
    </br>
3. 对于不含内容的元素，可以省略结束标签，但在开始标签的元素名后面必须有斜线/，如：

   ```xml
   <person/>
   ```

4. 每个XML文件都有一个根元素，其他元素都作为根元素的内容，如：

    ```xml
        <!-- 这里的根元素是person-->
        <person>
            <name>张三</name>
            <age>16</age>
            <address>罗翔</address>
        </person>
    ```

    _**注：**_ 这里`<!--`和`-->`之间的部分叫注释，用于向阅读文件的用户解释代码的含义
    </br>

5. 元素可以嵌套，但不能交叉

    ```xml
    <!-- 错误示范，两个结束标签交叉 -->
    <b><i>张三</b></i>
    <!-- 正确示范-->
    <b><i>张三</i></b>
    ```

6. 元素属性的值必须用双引号括起来，如：

   ```xml
   <!--person具有属性id，值为“123”-->
   <person id="123">
    </person>
    ```

    对于可以自定义标签的XML而言，属性就是鸡肋，毫无使用的必要，反而会导致更多的解析代码。建议仅在属性不依赖元素便无意义时才使用属性
    </br>

7. XML中的< > ' "和&这五种符号有语法用途，非语法用途时要用以下转义字符来表达：

    |语义字符|转义字符|
    |--------|-------|
    |<       |\&lt;  |
    |>       |\&gt;  |
    |&       |\&amp; |
    |'       |\&apos;|
    |"       |\&quot;|

    _**注：**_ 当有大段包含语义字符的文本时，更方便的做法是用CDATA语句包裹起来，示例如下：

   ```xml
   <Truth>
   <![CDATA[ if y & z > x, that's mean x < y & z ]]>
   </Truth>
   ```

8. 除以上转义字符外，不存在其他以&开头;结尾的字段
   </br>

上述8条是XML的基本语法，也是决定XML文档是否**格式良好**的关键。
这些规则**不是强制的**，但违反规则会严重影响该文件的通用性

## 文档类型定义（DTD）

~过时，但必要。~

在纯XML中，由于元素是用户自定义的，因此解释器只能检查基本语法。
如果元素、属性或数据写错了，解释器就会无法判断。
而DTD的作用，就是定义文档的结构和内容，帮助解释器做额外的检查。

### DTD的使用方式

要在XML文档中使用DTD，则需要在XML声明之后、根元素之前使用DTD声明。
DTD声明有两种类型，一种是直接定义约束的声明，如：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 这里定义person元素应有三个子元素name、age、address-->
<!-- 所有子元素的内容都应当是字符串-->
<!DOCTYPE person [
    <!ELEMENT person (name, age, address)>
    <!ELEMENT name (#PCDATA)>
    <!ELEMENT age (#PCDATA)>
    <!ELEMENT address (#PCDATA)>
]>
```

另一种类型是引用外部DTD文件的声明，如：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE person SYSTEM "person.dtd">
```

```dtd
<!-- person.dtd文件内容 -->
<!ELEMENT person (name, age, address)>
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT address (#PCDATA)>
```

_**注：**_ 虽然引用的dtd文件往往来自用户自定义，但也可以在引号内填写超链接来引用线上的DTD，如：

```xml
<!DOCTYPE person SYSTEM "www.w3cschool.com.cn/dtd/not.dtd">
```

### DTD的语法规则

1. 所有DTD语句都应以`<!`开头，以`>`结尾。
2. 通过`<!ELEMENT 元素名 内容`语句可以声明元素，其中内容处可以有以下三种字段：
   - `EMPTY`：元素不包含数据
   - `ANY`：元素可以包含任何数据
   - `(内容列表)`: 元素可以包含其他元素或属性，取决于列表项，但不能同时包含两者  
     - `序列列表`：用`,`划分项目的列表叫序列列表，所有项目必须直接依次出现
     - `选择列表`：用`|`划分项目的列表叫选择列表，所有项目中仅出现一项

   在列表项后可以加上量词来限定该项出现的次数，可用的量词如下：
   - `?`：项目可以不出现，最多出现一次
   - `*`：项目可以不出现，可以出现任意次
   - `+`：项目至少出现一次，可以出现任意次
   - 如果没有量词，则该项目只能在对应位置出现一次

3. 通过`<!ATTLIST 元素名称 属性名称 属性类型 属性值>`语句可以为元素声明属性。
   其中属性类型可以是以下之一：
   - `CDATA`：表示该属性的值是文本数据
   - `(en1|en2|..)`：表示该属性的值必须是选择列表中的一个
   - `ID`：表示该属性的值是唯一标识符
   - `IDREF`：表示该属性的值是另一个元素的标识符
   - `IDREFS`：表示该属性的值是由元素标识符构成的列表
   - `NMTOKEN`：表示该属性的值是有效 XML 名称
   - `NMTOKENS`：表示该属性的值是由有效 XML 名称构成的列表
   - `ENTITY`：表示该属性的值是个实体
   - `ENTITIES`：表示该属性的值是实体列表
   - `NOTATION`：表示该属性的值是符号的名称
   - `xml`: 表示该属性的值是预定义的 xml 值
  
   属性值可以是以下之一：
   - 自定义的默认值
   - `#REQUIRED`：表示该属性是必需的
   - `#IMPLIED`：表示该属性是可选的
   - `#FIXED`：表示该属性值是固定的

4. 通过`<!ENTITY 实体名称 "实体值">`语句可以声明实体, 所声明的实体在XML中通过`&实体名称;`来引用。

## 架构定义语言（XSD）

XSD是一种基于XML的DTD替代者，虽然语法更为冗长，但也具有了更强大的功能
XSD本质上是另一种XML文件，因此XSD文件也要遵循XML语法规则。
XSD在约束XML文件的同时，也受到由W3C组织维护的以下四个命名空间的约束：
`http://www.w3.org/XML/1998/namespace`
`http://www.w3.org/2001/XMLSchema`
`http://www.w3.org/2001/XMLSchema-instance`
`http://www.w3.org/2007/XMLSchema-versioning`
由于XSD的作用是约束XML文件，所以解释器必须能以预制方法处理XSD文件，因此XSD语法中含有大量预定义的标签

### XSD的使用方式

在XML中根元素中设置`xmlns`属性来指定提供约束的XSD文件，属性值为XSD文件的`targetNamespace`，如：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="Main main.xsd" 
      xmlns:main="MainSD" />
```

_**注：**_ `xmlns:xsi`的作用给w3约束指定限定符`xsi`，从而使`schemalocation`属性可用
`xsi:schemaLocation`用于指定外来文档的`targetNamespace`和路径，以空格分隔。
`xmlns:main`用于声明一个外来文档的命名空间，并为其指定限定符`main`。当有限定符时，外来文档中定义的元素必须通过限定符来使用（`main:xxx`），否则可以直接使用(`xxx`)。

### 如何写XSD

1. 每个XSD文档中都要有一个名为`schema`的根元素，该元素包含以下常用属性：
    - `xmlns`：声明一个来自其他文档命名空间，值为对应文档的`targetNamespace`，可以有多个，但至少要有一个值为`http://www.w3.org/2001/XMLSchema`的`xmlns`属性，作用是引入对XSD的语法约束
    - `targetNamespace`：该文档的命名空间，当别的文档从该文档中导入类型、属性时使用的名称
    - `elementFormDefault`：元素默认形式，决定在该文档中使用元素时是否必须限定命名空间，可选的值为`qualified`或`unqualified`，后者为默认值
    - `attributeFormDefault`：同上，但适用于属性

2. 当根元素使用了值不为`http://www.w3.org/2001/XMLSchema`的`xmlns`属性时，根元素中还应包括子元素`import`，该元素应使用`namespace`和`schemaLocation`属性，值分别为对应文档的`targetNamespace`和对应文档的路径，路径可以是本地文件路径，也可以是超链接。

3. 通过`element`元素可以声明一个元素约束，该元素可具有以下属性：
   - `name`：元素名，必选，该属性的值在XSD文档中应唯一。XML文档中通过`<元素名>`来使用该元素
   - `type`：元素类型，可选，但除非要引用其他地方已经声明的类型否则不建议选，使用`simpleType`或`complexType`作为子元素可以达到同样效果。
   - `default`：元素默认值，可选。当解析器读取该元素时，若该元素内容为空，则将从XSD中读取默认值。
   - `fixed`：元素固定值，可选。该元素内容不可为空值或与固定值以外的其他值，若为空值则从XSD中读取固定值。
   - `maxOccurs`：元素最大出现次数，可选，值为`unbounded`或正整数，默认值为1。
   - `minOccurs`：元素最小出现次数，可选，值为0或正整数，默认值为1。
   - `nillable`：元素是否可以为空，可选，值为`true`或`false`。
  
    _**注：**_ 由于格式良好的XML中只能有一个根元素，因此能直接出现在`schema`元素中的`element`元素也只有一个。

4. 通过`attribute`元素可以声明一个属性约束，该元素可具有以下属性：
   - `name`：属性名，必选，该属性的值在XSD文档中应唯一。
   - `type`：属性类型，可选，值可以是内置数据类型或用户自定义的`simpleType`的`name`
   - `default`：属性默认值，可选。当解析器读取该属性时，若该属性内容为空，则将从XSD中读取默认值
   - `fixed`：属性固定值，可选。该元素内容不可为空值或与固定值以外的其他值，若为空值则从XSD中读取固定值。
   - `use`：属性必需性，可选
     - 默认值为`optional`，意味着该元素所声明的属性不必须被指定
     - 值为`required`时，该元素所声明的属性必须被指定

5. 一个具有简单类型的元素意味着其内容只能是文本（无论是什么类型的文本）。通过`simpleType`元素可以声明简单类型，该元素必须具有属性`type`，属性的值可以是以下之一：
   - `boolean`：布尔类型
   - `string`：字符串类型
   - `integer`：整数类型
   - `decimal`：浮点数类型
   - `float`：单精度浮点数类型
   - `double`：双精度浮点数类型
   - `date`：日期类型
   - `time`：时间类型
   - `dateTime`：日期时间类型
   - `duration`：持续时间类型
   - `gYear`：年类型
   - `gYearMonth`：年月类型
   - `gMonth`：月类型
   - `gMonthDay`：月日类型
   - `gDay`：日类型
   - `anyURI`：统一资源标识符类型
   - `QName`：限定名类型
   - `NOTATION`：符号类型
   - `hexBinary`：十六进制二进制类型
   - `base64Binary`：Base64二进制类型
   - 由其他文档中引入的类型

    该元素在schema元素中可以不限次数的出现，但在element元素中只能出现一次，因为一个元素只能有一个类型。
    该元素中不能通过包含任何子元素来声明属性。

6. 一个具有复杂类型的元素，意味着其内容既可以是文本，也可以是其他元素。通过`complexType`元素可以声明复杂类型，该元素可以具有以下属性：
   - `name`：类型名，可选
   - `ID`: 类型ID，可选
   - `final`: 类型是否为最终类型，可选。
     - 默认值为`None`，意味着可以任意派生出新类型。
     - 值为`extension`时，不能通过扩展派生出新类型
     - 值为`restriction`时，不能通过限制派生出新类型
     - 值为`all`时，不能以任何方式派生出新类型
   - `abstract`: 类型是否为抽象类型，可选。
     - 默认值为`false`,意味着类型可以实例化。
     - 值为`true`时，该类型不能实例化
   - `mixed`: 子元素之间是否可以夹杂文本，可选。
     - 默认值为`false`，意味着子元素之间只能是元素。
     - 值为`true`时，子元素可以是元素或文本。
   - `block`: 阻止对元素内容的约束，可选。
     - 默认值为`#all`，意味着不能对元素内容增加任何约束。
     - 值为`extension`时，不能通过扩展来增加对元素内容的约束
     - 值为`restriction`时，不能通过限制来增加对元素内容的约束

    该元素在schema元素中可以不限次数的出现，但在element元素中只能出现一次，因为一个元素只能有一个类型。
    该元素可以包含的子元素有：
    - `(simpleContent | complexContent)`
    - `(group| choice | sequence | all)`
    在该元素不包含`simpleContent`或`complexContent`时，可以包含以下元素来声明属性：
    - `(attribute , attributeGroup)`
    - `anyAttribute`
    在该元素包含`simpleContent`或`complexContent`时，该元素将被视为原有类型的一个派生类型

## 一些帮助性的规则

- 所有元素和属性都应使用大驼峰命名法，且应避免使用连字符、空格。
- 可读性比标签长度更重要。
- 避免使用缩写，除非该缩写在你业务领域内众所周知，如 ID（标识符）
- 所有类型名称都已“Type”结尾
- 枚举值应该使用名称而不是数字，并且名称应该是采用大驼峰命名法。
- 名称不应包含包含父元素的名称，例如 CustomerName 应拆解为父元素 Customer 的子元素 Name。
- 如果一个类型只存在于一个地方，则使用匿名复杂类型将其内联定义。
- 仅当元素能够成为 XML 文档中的根元素时才定义根级元素。如果您希望元素具有全局范围，请创建根级 ComplexType 或 SimpleType。
- 使用一致的命名空间别名并避免使用标准定义的前缀：
  - xml（XML 标准中定义）
  - xmlns（在 XML 标准中的命名空间中定义）
  - xs（定义为`http://www.w3.org/2001/XMLSchema`）
  - xsi（定义为`http://www.w3.org/2001/XMLSchema-instance`）
- 尝试在XSD设计早期考虑版本控制。如果XSD的新版本向后兼容很重要，那么架构的所有添加都应该是可选的。如果现有产品能够读取给定文档的较新版本很重要，那么请考虑在定义末尾添加 any 和 anyAttribute 条目。
- 在XSD中定义一个`targerNamespace`，这可以使模块化和重用变得更容易。
- 在XSD的schema元素中设置 elementFormDefault="qualified"。这可使生成的 XML 中的名称空间更易于阅读。

## XML之外

XSLT是一种基于XML的样式表语言，用于为XML提供样式
XQuery是一种基于XML的查询语言，用于从XML中查询数据
XLink 用于在 XML 文档中创建超链接
WSDL 是一种基于 XML 的用于描述 Web 服务的语言
SOAP 是一种基于 XML 的用于访问 Web 服务的协议
RDF 是一种基于 XML 的用于描述 Web 资源的框架
RSS 是一种基于XML的用于发布和更新网站的信息来源格式规范
Ajax是一种基于XML和JAVA的、为 Web 应用程序构建交互式用户界面的技术
