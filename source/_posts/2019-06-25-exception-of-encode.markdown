---
layout:     post
title:      "数据录入格式错误问题-u202c"
subtitle:   "数据录入格式错误问题-u202c"
date:       2019-06-25T02:53:47+08:00
banner_img: /img/post_banner_common.jpg
categories:
  - 经验
tags:
    - 工作随笔
---

# 数据录入问题
同事在录数据的时候表示无法提交, 报错了.  错误日志如下
## 报错提示:
```
{"timestamp":"2019-06-11 12:22:15","status":400,"error":"Bad Request","message":"JSON parse error: Cannot deserialize value of type `java.lang.Integer` from String \"9290‬\": not a valid Integer value; nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `java.lang.Integer` from String \"9290‬\": not a valid Integer value\n at [Source: (PushbackInputStream); line: 1, column: 1893] (through reference chain: com.xxxx.creative.inspection.server.model.Phone[\"phoneComponents\"]->java.util.ArrayList[12]->com.xxxx.creative.inspection.server.model.PhoneComponent[\"height\"])","path":"/fe/phone"}
```


## 请求数据为:
```json
{"id":24552,"model":"华为 nova 青春版","name":"华为 nova 青春版","brand":"华为 ","height":"146220","width":"72020","thickness":"7220","status":0,"checkable":false,"fillCount":15,"updateDate":"2019-06-11 11:36:43","phoneComponents":[{"exist":true,"phoneSide":"1","height":"1510","width":"1510","thickness":"0","relativeY":"8500","relativeX":"25540","type":1,"index":0},{"exist":true,"phoneSide":"1","height":"3000","width":"3000","thickness":"0","relativeY":"7750","relativeX":"1740","type":2,"index":1},{"exist":false,"phoneSide":"","height":"","width":"","thickness":"","relativeY":"","relativeX":"","type":3,"index":2},{"exist":true,"phoneSide":"2","height":"6040","width":"6040","thickness":"0","relativeY":"7450","relativeX":"19350","type":4,"index":3},{"exist":false,"phoneSide":"","height":"","width":"","thickness":"","relativeY":"","relativeX":"","type":5,"index":4},{"exist":true,"phoneSide":"1","height":"1650","width":"9850","thickness":"0","relativeY":"7450","relativeX":"35100","type":6,"index":5},{"exist":true,"phoneSide":"4","height":"1790","width":"10690","thickness":"0","relativeY":"2920","relativeX":"51780","type":7,"index":6},{"exist":true,"phoneSide":"4","height":"1790","width":"10690","thickness":"0","relativeY":"2920","relativeX":"51780","type":8,"index":7},{"exist":true,"phoneSide":"1","height":"114870","width":"65190","thickness":"0","relativeY":"14500","relativeX":"3080","type":9,"index":8},{"exist":true,"phoneSide":"2","height":"2880","width":"2880","thickness":"0","relativeY":"7220","relativeX":"9920","type":10,"index":9},{"exist":true,"phoneSide":"1","height":"7700","width":"11920","thickness":"0","relativeY":"125720","relativeX":"29290","type":11,"index":10},{"exist":true,"phoneSide":"6","height":"8280","width":"1760","thickness":"500","relativeY":"56740","relativeX":"3290","type":12,"index":11},{"exist":true,"phoneSide":"6","height":"9290‬","width":"1760","thickness":"500","relativeY":"30580","relativeX":"3290","type":13,"index":12},{"exist":true,"phoneSide":"6","height":"9290‬","width":"1760","thickness":"500","relativeY":"41040","relativeX":"3290","type":14,"index":13},{"exist":true,"phoneSide":"4","height":"2350","width":"7180","thickness":"0","relativeY":"2860","relativeX":"34980","type":15,"index":14},{"exist":true,"phoneSide":"3","height":"4400","width":"4400","thickness":"0","relativeY":"3500","relativeX":"44630","type":16,"index":15}]}
```


检查半天发现, 我输入的字符及对应unicode为:

```
"9290"
\u0022\u0039\u0032\u0039\u0030\u0022
```

测试人员输入的字符以及unicode为:

```
‬"9290‬"
\u202c\u0022\u0039\u0032\u0039\u0030\u202c\u0022
```


后来得知他们是从windows计算器复制的数字粘贴到了输入框....

