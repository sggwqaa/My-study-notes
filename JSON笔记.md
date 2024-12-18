# JSON笔记

[toc]

## JSON是什么

JSON是一种的用于数据存储和传输的语言数据
JSON基于JS，可以表示任何JS对象，且能直接被JS解释器解析
JSON是**轻量级**语言，相比于传统的XML而言字符量小，传输快

## JSON的语法

JSON中的对象表示为**键值对**，用花括号保存
键和值用冒号分隔，键必须是字符串，值可以是：

- 字符串（不能含有`\`和`"`，需要用转义字符表示）
- 数值
- 布尔值
- 数组
- 对象
- null

数组用方括号保存，数组中的数据以逗号分隔

```json
{
  "name": "John",
  "age": 30,
  "hobbies": ["reading", "swimming", "running"],
  "friend": 
  [
    {"name": "lily",
    "age": 30,
    "hobbies": [, "swimming", "running"]},
    {"name": "jack",
    "age": 25,
    "hobbies": ["reading", "swimming"]}
  ]
}
```

而同样的数据，在XML中的表示是

```xml
<person>
  <name>John</name>
  <age>30</age>
  <hobbies>
  <hobbie>reading</hobbie>
  <hobbie>swimming</hobbie>
  <hobbie>running</hobbie>
  </hobbies>
  <friend>
    <person>
      <name>lily</name>
      <age>30</age>
      <hobbies>
        <hobbie>swimming</hobbie>
        <hobbie>running</hobbie>
      </hobbies>
    </person>
    <person>
      <name>jack</name>
      <age>25</age>
      <hobbies>
        <hobbie>swimming</hobbie>
        <hobbie>running</hobbie>
      </hobbies>
    </person>
  </friend>
</person>
```

## 其他轻量化数据存储语言

用于配置信息存储的TOML和YAML。应用范围更窄，但数据更简短、可读性更高。
