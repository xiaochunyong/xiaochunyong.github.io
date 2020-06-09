---
layout:     post
title:      "Feign 启用日志"
date:       2019-08-13T11:14:50+08:00
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - SpringBoot
    - Feign
---
> Spring中集成openfeign, 打印日志

> Feign 的日子有两层检查


```kotlin
feign.SynchronousMethodHandler {
    fun executeAndDecode() {
        //...    
        if (logLevel != Logger.Level.NONE) {
          logger.logRequest(metadata.configKey(), logLevel, request);
        }
        //...
    }
}
```

```kotlin
feign.slf4j.Slf4jLogger : feign.Logger {

    //...
    fun logRequest() {
        if (this.logger.isDebugEnabled()) {
            super.logRequest(configKey, logLevel, request);
        }
    }

    //...
}

```

### 配置
```kotlin
@Bean
fun feignLoggerLevel(): Logger.Level {
    return Logger.Level.FULL
}
```

```yml
logging:
  level:
    com.aihuishou.creative.itm.client: DEBUG
```