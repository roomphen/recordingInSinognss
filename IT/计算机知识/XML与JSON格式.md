[TOC]

### 序言

#### JAVA语言

```
service(
        name:'test',do [
            set(name:'a',value:1);
            set(name:'b',value:0);
            def(name:'c',value:0);
            if(condition:'a==0',do[
                set(name:'b',value:1);
                test(desc:'b1');
            ]) 
            else(condition:'a==2',do [
                    set(name:'b',value:2);
                    test(desc:'b2');
            ]) 
            else( do [
                set(name:'b',value:3);
                set(name:'c',value:3);
                def(name:'d',value:3);
                test(desc:'b3');
                if(condition:'b==2',do [
                        set(name:'e',value:1);
                        test(desc:'e1');
                ])
                else(do [
                        set(name:'e',value:2);
                        test(desc:'e2');
                ])
            ]);
            test(desc:'d4')
    ])
```

#### Json语言

```
{
        service: {
            name: 'test',
            do: [
                {set: {name: 'a',value: 1}},
                {set: {name: 'b',value: 0}},
                {def: {name: 'c',value: 0}},
                {if: {condition: 'a==0',
                    do: [
                        {set: {name: 'b',value: 1}},
                        {test: {desc: 'b1'}}
                    ]}
                },
                {else: { condition: 'a==2',
                    do: [
                        {set: {name: 'b',value: 2}},
                        {test: {desc: 'b2'}}
                    ]}
                },
                {else: {
                    do: [
                        {set: {name: 'b',value: 3}},
                        {set: {name: 'c',value: 3}},
                        {def: {name: 'd',value: 3}},
                        {test: {desc: 'b3'}},
                        {if: { condition: 'b==2',
                                do: [
                                    {set: {name: 'e',value: 1}},
                                    {test: {desc: 'e1'}}
                                ]}
                        },
                        {else: {
                                do: [
                                    {set: {name: 'e',value: 2}},
                                    {test: {desc: 'e2'}}
                                ]}
                            }
                        ]
                    }
                },
                {test: {desc: 'd4'}}
            ]
        }
    }
```

#### xml语言

```
<!--注释1：申明是xml报文，指名版本和编码-->
<?xml version="1.0" encoding="UTF-8" ?>
    <class>
        <name>提高班</name>
        <no>1801</no>
        <students>
            <student>
                <name>宋海娇</name>
                <sex>女</sex>
            </student>
            <student>
                <name>毛伟伟</name>
                <sex>男</sex>
            </student>
        </students>
    </class>
```

http报文

```
HTTP/1.1 200 OK
Server: nginx
Date: Mon, 22 Jan 2018 12:11:32 GMT
Content-Type: text/html;charset=UTF-8
Connection: keep-alive
Vary: Accept-Encoding
P3P: CP="IDC DSP COR ADM DEVi TAIi PSA PSD IVAi IVDi CONi HIS OUR IND CNT"
Cache-Control: no-cache
Content-Length: 1286

{
    "message": "ok",
    "nu": "3350862539854",
    "ischeck": "1",
    "condition": "F00",
    "com": "shentong",
    "status": "200",
    "state": "3",
    "data": [{
        "time": "2018-01-16 12:46:12",
        "ftime": "2018-01-16 12:46:12",
        "context": "上海市 本人-已签收，感谢您选择申通快递，期待再次为您服务。",
        "location": null
    },{
        "time": "2018-01-15 20:08:52",
        "ftime": "2018-01-15 20:08:52",
        "context": "金华市 浙江义乌公司(4009266666)-菜鸟面单2-已收件",
        "location": null
    }]
}
```



### XML

#### 样例

```
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

#### xml与html异同

xml被用来传输和存储数据，其焦点是数据的内容

HTML被用来显示数据，其焦点是数据的外观

#### 语法

1. xml声明
2. 根元素
3. 所有元素必须关闭
4. 注释

```
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <!-- This is a comment -->
  <body>Don't forget me this weekend!</body>
</note>
```

#### 元素

1. 属性
2. 文本
3. 子元素

```
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

#### 验证

1. 添加验证
2. DTD
3. Schema

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE note SYSTEM "Note.dtd">
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

```
<!DOCTYPE note
[
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
]>
```

```
<xs:element name="note">
<xs:complexType>
<xs:sequence>
<xs:element name="to" type="xs:string"/>
<xs:element name="from" type="xs:string"/>
<xs:element name="heading" type="xs:string"/>
<xs:element name="body" type="xs:string"/>
</xs:sequence>
</xs:complexType>
</xs:element>
```

#### 查看

1. 浏览器
2. notepad
3. MyEclipse









### JSON

#### 样例

```
{
"sites": [
        { "name":"菜鸟教程" , "url":"www.runoob.com" }, 
        { "name":"google" , "url":"www.google.com" }, 
        { "name":"微博" , "url":"www.weibo.com" }
    ]
}
```

#### json与xml的异同

```
1. 没有结束标签
2. 更短
3. 读写的速度更快
4. 能够使用内建的 JavaScript eval() 方法进行解析
5. 使用数组
6. 不使用保留字
```

#### json语法

```
1. 数据在名称/值对中
2. 数据由逗号分隔
3. 大括号保存对象
4. 中括号保存数组
```

```
{
"sites": [
        { "name":"菜鸟教程" , "url":"www.runoob.com" }, 
        { "name":"google" , "url":"www.google.com" }, 
        { "name":"微博" , "url":"www.weibo.com" }
    ]
}
```





### JSON与XML对比

JSON数据

```

“{
          "kpzdbs": "开票终端标识",
          "fplxdm": "发票类型代码",
          "fpdm": "发票代码",
          "fphm": "发票号码",
          "dylx": "打印类型",
          "dyfs": "打印方式",
          "printername": "打印机名称"
 
}”;
```

XML数据

```
“<?xml version="1.0" encoding="gbk"?>
<business>
<body >
<kpzdbs>开票终端标识</kpzdbs>
<fplxdm>发票类型代码</fplxdm>
<fpdm>发票代码</fpdm>
<fphm>发票号码</fphm>
<dylx>打印类型</dylx>
<dyfs>打印方式</dyfs>
<printername>打印机名称</printername>
</body>
</business>”；
```







































































