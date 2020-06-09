---
layout:     post
title:      "Elastic SQL Access"
date:       2019-11-03T15:57:37+08:00
banner_img: /img/post_banner_common.jpg
index_img: /img/index/Elastic.png
categories:
  - 数据库
tags:
    - elastic
---
> 想通过SQL语句查询ES, 遇到以下错误, 提示需要platinum许可(需要购买)
> java.sql.SQLInvalidAuthorizationSpecException: current license is non-compliant for [jdbc] 

## Elastic SQL Access
#### 1. 安装Elastic
/Users/Ely/SDK/elasticsearch-7.4.0

#### 2. 篡改证书验证代码
a. Github源码
* [LicenseVerifier.java](https://github.com/elastic/elasticsearch/blob/master/x-pack/plugin/core/src/main/java/org/elasticsearch/license/LicenseVerifier.java)
* [XPackBuild.java](https://github.com/elastic/elasticsearch/blob/master/x-pack/plugin/core/src/main/java/org/elasticsearch/xpack/core/XPackBuild.java)

b. 修改源码
```java
/*
 * Copyright Elasticsearch B.V. and/or licensed to Elasticsearch B.V. under one
 * or more contributor license agreements. Licensed under the Elastic License;
 * you may not use this file except in compliance with the Elastic License.
 */
package org.elasticsearch.license;

import org.apache.lucene.util.BytesRef;
import org.apache.lucene.util.BytesRefIterator;
import org.elasticsearch.common.bytes.BytesReference;
import org.elasticsearch.common.xcontent.ToXContent;
import org.elasticsearch.common.xcontent.XContentBuilder;
import org.elasticsearch.common.xcontent.XContentFactory;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.core.internal.io.Streams;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.ByteBuffer;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.Signature;
import java.security.SignatureException;
import java.util.Arrays;
import java.util.Base64;
import java.util.Collections;

/**
 * Responsible for verifying signed licenses
 */
public class LicenseVerifier {

    /**
     * verifies the license content with the signature using the packaged
     * public key
     * @param license to verify
     * @return true if valid, false otherwise
     */
    public static boolean verifyLicense(final License license, byte[] publicKeyData) {
			return true;
    }

    public static boolean verifyLicense(final License license) {
			return true;
    }
}
```

```java
/*
 * Copyright Elasticsearch B.V. and/or licensed to Elasticsearch B.V. under one
 * or more contributor license agreements. Licensed under the Elastic License;
 * you may not use this file except in compliance with the Elastic License.
 */
package org.elasticsearch.xpack.core;

import org.elasticsearch.common.SuppressForbidden;
import org.elasticsearch.common.io.PathUtils;

import java.io.IOException;
import java.net.URISyntaxException;
import java.net.URL;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.jar.JarInputStream;
import java.util.jar.Manifest;

/**
 * Information about the built version of x-pack that is running.
 */
public class XPackBuild {

    public static final XPackBuild CURRENT;

    static {
		final Path path = getElasticsearchCodebase();
        String shortHash = null;
        String date = null;
        Label_0157: {
            shortHash = "Unknown";
            date = "Unknown";
        }
        CURRENT = new XPackBuild(shortHash, date);
    }

    /**
     * Returns path to xpack codebase path
     */
    @SuppressForbidden(reason = "looks up path of xpack.jar directly")
    static Path getElasticsearchCodebase() {
        URL url = XPackBuild.class.getProtectionDomain().getCodeSource().getLocation();
        try {
            return PathUtils.get(url.toURI());
        } catch (URISyntaxException bogus) {
            throw new RuntimeException(bogus);
        }
    }

    private String shortHash;
    private String date;

    XPackBuild(String shortHash, String date) {
        this.shortHash = shortHash;
        this.date = date;
    }

    public String shortHash() {
        return shortHash;
    }

    public String date() {
        return date;
    }
}
```

c. 编译源码

```shell
javac -cp "./lib/lucene-core-8.2.0.jar:./lib/elasticsearch-7.4.0.jar:./lib/elasticsearch-core-7.4.0.jar:./lib/elasticsearch-x-content-7.4.0.jar:./modules/x-pack-core/x-pack-core-7.4.0.jar" LicenseVerifier.java
javac -cp "./lib/lucene-core-8.2.0.jar:./lib/elasticsearch-7.4.0.jar:./lib/elasticsearch-core-7.4.0.jar:./lib/elasticsearch-x-content-7.4.0.jar:./modules/x-pack-core/x-pack-core-7.4.0.jar" XPackBuild.java
```

d. 替换源Jar包中的class文件

```shell
cd $ES_HOME/modules/x-pack-core
cp x-pack-core-7.4.0.jar x-pack-core-7.4.0.jar.bak // 备份

// 把编译后的源码放入到
$ES_HOME/modules/x-pack-core/org/elasticsearch/xpack/core/XPackBuild.class
$ES_HOME/modules/x-pack-core/org/elasticsearch/license/LicenseVerifier.class

// 替换jar包中的class (当前路径为$ES_HOME/modules/x-pack-core)
jar uvf x-pack-core-7.4.0.jar org/elasticsearch/xpack/core/XPackBuild.class
jar uvf x-pack-core-7.4.0.jar org/elasticsearch/license/LicenseVerifier.class
```

e. 重新启动elasticsearch

#### 下载证书
> 2种方式都行

1. 前往 https://license.elastic.co/registration 注册一个免费的, 获取证书JSON(邮箱查收)
2. 复制以下内容(这是我从上面这个网址注册的)

```
{"license":{"uid":"1a2bdedf-c99b-42af-8101-920e950287c0","type":"basic","issue_date_in_millis":1572739200000,"expiry_date_in_millis":1604447999999,"max_nodes":100,"issued_to":"Ely Xiao (ELY.ME)","issuer":"Web Form","signature":"AAAAAwAAAA36w4sLJgX5N9JBopEEAAABmC9ZN0hjZDBGYnVyRXpCOW5Bb3FjZDAxOWpSbTVoMVZwUzRxVk1PSmkxaktJRVl5MUYvUWh3bHZVUTllbXNPbzBUemtnbWpBbmlWRmRZb25KNFlBR2x0TXc2K2p1Y1VtMG1UQU9TRGZVSGRwaEJGUjE3bXd3LzRqZ05iLzRteWFNekdxRGpIYlFwYkJiNUs0U1hTVlJKNVlXekMrSlVUdFIvV0FNeWdOYnlESDc3MWhlY3hSQmdKSjJ2ZTcvYlBFOHhPQlV3ZHdDQ0tHcG5uOElCaDJ4K1hob29xSG85N0kvTWV3THhlQk9NL01VMFRjNDZpZEVXeUtUMXIyMlIveFpJUkk2WUdveEZaME9XWitGUi9WNTZVQW1FMG1DenhZU0ZmeXlZakVEMjZFT2NvOWxpZGlqVmlHNC8rWVVUYzMwRGVySHpIdURzKzFiRDl4TmM1TUp2VTBOUlJZUlAyV0ZVL2kvVk10L0NsbXNFYVZwT3NSU082dFNNa2prQ0ZsclZ4NTltbU1CVE5lR09Bck93V2J1Y3c9PQAAAQAJLS+Eh3LGnBI3TZF649WireO3M5sZ1t1LnIGAYBr3mNnNXv3snIsGK+2a9T3UfErf/L4y8ikx71pAVb8+KQM802fcHodNBmAQGnIOCeKwv5YqHfIUbF6di/zFYx6SZOATi/0r7eoJ4c8g46rTnf8x2ZLYhxryR9p1KDmS4cNE9X84SA8o8ixUn8vUx3vQg9hVF7uJ+KxiB5Me+W5dLW2UNfI1bAfKq5HFggepC/7+zystidatotnulohwJxWgJPlkO70ngGNV1a/tKoOI/YhXzlA841XjmSHLc7kTDdk/E/SQmYtFG+4cgaN9QbvTGDkhtTxm0sHwptYF4zs+IUsu","start_date_in_millis":1572739200000}}
```

#### 修改证书
修改上述步骤中JSON内容`"type":"platinum"`, `"expiry_date_in_millis":3107746200000`


#### 上传证书
curl -XPUT   'http://127.0.0.1:9200/_xpack/license?acknowledge=true' -d @license.json


#### 查看证书
http://127.0.0.1:9200/_license








#### 认证
1. 关闭认证

    编辑 config/elasticsearch.yml

```
xpack.security.enabled: false
```

2. 开启认证, 则要设置密码

```
bin/elasticsearch-setup-passwords auto|interactive
```

