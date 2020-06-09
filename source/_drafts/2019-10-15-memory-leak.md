---
layout:     post
title:      "内存泄漏分析"
subtitle:   "Memory Leak Analysis"
date:       2019-10-28T11:08:11+08:00
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - JVM
    - Java
    - FastJSON
    - Jersey
---
> desc..
> desc..

记一次内存泄漏分析.

FastJSON
产生大量类, 加载到了Metaspace, 由于Metaspace存放在native memory中, 没有参与回收. 也没有Metaspace的大小.  导致泄露



com.alibaba.fastjson.util.ASMClassLoader  假定它是平级类加载器

JerseyClient 每次

foundation-service 每次获取JerseyClient 都会返回一个新的, 这个新的里面包含了一个 FastJSON序列化和反序列化时,  类型与序列化器一个表.  会根据指定的反序列化类型, 创建类型 以及 类型中的每个字段 所对应的的序列化器
ParserConfig.deserializers
SerializeConfig.serializers
由于每次请求都会产生一个新的JerseyClient, 重新初始化 2个大表. 重新加载指定类型


new JerseyClient  -> new FastJsonProvider() -> new ParserConfig & new SerializeConfig -> deserializers & serializers & loadClass
最终导致metaspace内存上涨


FastJSON会为需要序列化或反序列化的类型创建Serilizer

ASMDeserializerFactory.createJavaBeanDeserializer 会动态创建指定class的序列化类 (asm) 



```kotlin
    private val clientCache = ConcurrentHashMap<String, JerseyClient>()

    var endpoint: String? = null

    private fun getClient(): JerseyClient? {
        if (clientCache[endpoint] == null) {
            synchronized(AbstractAhsService::class.java) {
                if (clientCache[endpoint] == null) {
                    val factory = JerseyClientFactory()
                    factory.serviceUrl = endpoint
                    factory.readTimeout = 10000
                    factory.threadPoolSize = 50
                    clientCache[endpoint!!] = factory.`object`!!
                }
            }
        }
        return clientCache[endpoint]
    }
```