---
layout:     post
title:      "Kotlin Data Class 踩坑记录"
date:       2019-08-18T19:26:40+08:00
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - Kotlin
---

> 有如下类

```kotlin
open class BaseEntity {
    var createDt: Date = Date()
    var updateDt: Date = Date()
    var deleted: Boolean = false
    var version: Long = 0
}

data class Device(
    val imei: String,
    val sn: String,
    val productName: String,
    val productType: String
) : BaseEntity()
```

## 1. 数据类调用copy后, 会导致BaseEntity类中的字段丢失, 测试用例如下:

```kotlin
@Test
fun testDataClassCopy() {
    val device = JSON.parse<Device>("""{ "imei": "358730091744037", "sn": "G6TXXNZPKPJ3", "productName": "iPhone OS", "productType": "iPhone11,6", "createDt":"2019-08-18 19:35:00","updateDt":"2019-08-18 19:35:00","deleted":false,"version":3 }""")
    assertEquals(device.version, 3)
    val newDevice = device.copy(imei = "this is imei")
    assertEquals(newDevice.version, 0)
}
```

> 以上测试用例通过. 为什么呢? version字段明明是3, 怎么变成了0呢? 我们看看Device类中copy方法反编译后的java代码

```java
@NotNull
public final Device copy(@NotNull String imei, @NotNull String sn, @NotNull String productName, @NotNull String productType) {
  Intrinsics.checkParameterIsNotNull(imei, "imei");
  Intrinsics.checkParameterIsNotNull(sn, "sn");
  Intrinsics.checkParameterIsNotNull(productName, "productName");
  Intrinsics.checkParameterIsNotNull(productType, "productType");
  return new Device(imei, sn, productName, productType);
}
```

> 从代码可以看出, Device的父类BaseEntity中的字段并没有在copy方法中出现. 导致父类中的字段全部变成了默认值.