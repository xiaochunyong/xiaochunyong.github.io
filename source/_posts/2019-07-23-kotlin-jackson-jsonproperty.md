---
layout:     post
title:      "Jackson/FastJSON处理Kotlin Data Class字段别名"
date:       2019-07-23T12:05:23+08:00
banner_img: /img/post_banner_common.jpg
index_img: /img/index/Kotlin.png
categories:
  - Kotlin
tags:
    - kotlin
    - json
    - 工作随笔
---
## 问题
#### Jackson/FastJSON 处理 Kotlin Data Class时, JsonProperty/JSONField没有正常工作
> Jackson/FastJSON 在处理 Kotlin Data Class 时, 如果需要别名, 直接使用@JsonProperty是不行的.

```
data class User(
    val id: Int,
    val name: String,
    @JSONField(name = "isEnable") @JsonProperty("isEnable") val enable: Boolean 
)
```

> 必须得:

```
data class User(
    val id: Int,
    val name: String,
    @get:JSONField(name = "isEnable") @get:JsonProperty("isEnable") val enable: Boolean 
)
```

#### 参考
[Stackoverflow](https://stackoverflow.com/a/56516952/4525918) `没有足够的Stackoverflow reputation无法点赞..所以记录一下..`
