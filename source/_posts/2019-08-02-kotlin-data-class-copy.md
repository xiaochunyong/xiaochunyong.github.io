---
layout:     post
title:      "Kotlin 数据类Merge其它对象的属性"
subtitle:   "Kotlin data class merge properties from another object"
date:       2019-08-02T23:58:13+08:00
banner_img: /img/post_banner_common.jpg
index_img: /img/index/Kotlin.png
categories:
  - Kotlin
tags:
    - Kotlin
---
> 在使用Kotlin + SpringBoot做Web项目时, 需要将前端传来的数据塞到新创建的实体类对象中. 先看类的结构:

```kotlin
@MappedSuperclass
open class BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long? = null  //主键

    /**
     * 创建时间
     */
    @field:CreationTimestamp
    @Column(name = "create_dt", nullable = false, updatable = false)
    var createDt: Date = Date() // 默认值在做插入时会被覆盖

    /**
     * 更新时间
     */
    @field:UpdateTimestamp
    @Column(name = "update_dt", nullable = false)
    var updateDt: Date = Date()

    /**
     * 逻辑删除标记
     */
    var deleted: Boolean = false

    /**
     * 乐观锁版本号
     */
    @Version
    var version: Long = 0
}


@Entity
@Table(name = "user")
data class User(
        var username: String,
        val password: String,
        val nickname: String,
        val mobile: String,
        val status: Int
) : BaseEntity()

// 创建时前端传过来的数据类
data class UserCreateRequestModel(
        val username: String,
        val password: String,
        val nickname: String,
        val mobile: String,
        val version: Int
)
// 修改时前端传过来的数据类
data class UserUpdateRequestModel (
        val username: String,
        val nickname: String,
        val mobile: String
)
```

> 最开始的写法是这样的

```kotlin
@PostMapping("/")
@ApiOperation(nickname = "CreateUser", value = "Create User")
fun create(@RequestBody model: UserCreateRequestModel): SimpleResponse {
    logger.info("create user params: $model")
    userService.save(User(
        username = model.username, 
        password = model.password,
        nickname = model.nickname,
        mobile = model.mobile
    ))
    return ResponseCode.SUCCESS.toResponse()
}

@PutMapping("{id}")
@ApiOperation(nickname = "UpdateUser", value = "Update User")    
fun update(@PathVariable id: Long, @RequestBody model: UserUpdateRequestModel): SimpleResponse {
    logger.info("update user params: $id - $model")
    val opt = userService.findById(id)
    if (opt.isPresent) {
        val item = opt.get()
        userService.save(item.copy(
            username = model.username,
            nickname = model.nickname,
            mobile = model.mobile
        ))
        return ResponseCode.SUCCESS.toResponse()
    }
    return ResponseCode.NOT_FOUND.toResponse()
}
```

> 代码量特多, 还尽是一些重复劳动, 还容易漏写出错. 然后我想到了Java中的BeanUtils, 动手写个扩展函数~

```kotlin
// v1: 使用了Spring BeanUtils
fun <T : Any> T.merge(source: Any): T {
    BeanUtils.copyProperties(source, this)
    return this
}

// Update方法改为
userService.save(item.merge(model))

// Create 方法改为
userService.save(User().merge(model)) // Error, 编译不通过!, 因为User是数据类, 必须给每个参数赋值(即使使用no-arg编译器插件, 运行时才会有无参构造, 无法通过编译器)
```

> 由于无法直接通过字面量写法User()直接对象, 想到编译器是有无参构造的, 于是再加一个辅助扩展函数new~

```kotlin
inline fun <reified T: Any> new(): T {
    return T::class.java.getDeclaredConstructor().newInstance()
}
```

> 有了new()这个扩展函数后,  create方法就可以这么写了~

```kotlin
// create
userService.save(new<User>().merge(model))
// update
userService.save(item.merge(model))
```

> 运行完后...又遇到另一个问题...model中的属性并没有复制到User对象中去, 这是为什么呢??? 
> 查看BeanUtils源码+调试后发现由于User类各属性的writeMethod为null导致....想到User类的属性都是val定义...

![-w819](/img/in-post/post-kotlin-data-class-copy/15647641082128.jpg)

> 将User类的Kotlin字节码反编译, 发现属性都是final, 并且没有set方法

![-w847](/img/in-post/post-kotlin-data-class-copy/15647643796506.jpg)

> 查了一番资料, 没有看到如何直接读写属性的(又或者因为是final的, 即使可以, 也无法读写),  看了kotlin数据类copy()方法的原理后, 决定自己写一个
> 为了描述方便, 在代码`a.merge(b)`中a为target, b为source

```kotlin
// v2: 通过反射target的类型, 获取主构造函数, 合并source的属性. 重新创建一个target对象
inline infix fun <reified T : Any, reified S : Any> T.merge(source: S): T {
    val nameToPropertySource = S::class.memberProperties.associateBy { it.name }
    val nameToPropertyTarget = T::class.memberProperties.associateBy { it.name }
    val primaryConstructor = T::class.primaryConstructor!!
    val args = primaryConstructor.parameters.associate { parameter ->
        val propertySource = nameToPropertySource[parameter.name]
        val propertyTarget = nameToPropertyTarget[parameter.name]
        parameter to (propertySource?.get(source) ?: propertyTarget?.get(this))
    }
    return primaryConstructor.callBy(args)
}
```

> 看起来没什么问题了, 但是还有几个潜藏的问题没有解决
> 1. 继承到的属性无法复制, 比如说User类从BaseEntity继承了id, createDt, updateDt, deleted, version这几个属性, 但是它们不在并没有在构造函数中, 所以即使UserCreateRequestModel中有这几个属性, 也无法merge
> 2. 如果BaseEntity中

```kotlin
// v3: 解决数据类继承属性无法复制的问题
inline infix fun <reified T : Any, reified S : Any> T.merge(source: S): T {
    val nameToPropertySource = S::class.memberProperties.associateBy { it.name }
    val nameToPropertyTarget = T::class.memberProperties.associateBy { it.name }
    val primaryConstructor = T::class.primaryConstructor!!
    val primaryConstructorParams = mutableListOf<String>()
    val args = primaryConstructor.parameters.associate { parameter ->
        primaryConstructorParams.add(parameter.name!!)
        val propertySource = nameToPropertySource[parameter.name.toString()]
        val propertyTarget = nameToPropertyTarget[parameter.name.toString()]
        parameter to (propertySource?.get(source) ?: propertyTarget?.get(this))
    }
    val newObj = primaryConstructor.callBy(args)
    nameToPropertyTarget.forEach { (name, propertyOfTarget) ->
        if (!primaryConstructorParams.contains(name) && propertyOfTarget is KMutableProperty1) {
            val mutableProperty = propertyOfTarget as KMutableProperty1<T, Any?>
            val propertySource = nameToPropertySource[name]
            if (propertySource != null) {
                mutableProperty.set(newObj, propertySource.get(source))
            } else {
                try {
                    mutableProperty.set(newObj, propertyOfTarget.get(this))
                } catch (e: Exception) {
                    // lateinit 属性如果没有值, 在这里会报错, 目前还没有找到判断是否有值的方式
                    // 压制异常 e.printStackTrace()
                }
            }
        }
    }
    return newObj
}
```

> 当前版本使用方式如下

```kotlin
new<User>().merge(model) // 不够优雅, 能否new<User>(model)呢?

item.merge(model)
```

> 由于当前版本, 在创建新对象的同时merge属性台啰嗦, 所以我们在对new函数做一次优化
> 同时为了适配新版的new函数, 导致的souce的类型擦除问题, 我们针对source反射部分采用了JDK8带的Introspector.getBeanInfo来解决

```kotlin
// 最终版
inline fun <reified T: Any> new(source: Any? = null): T {
    val obj = T::class.java.getDeclaredConstructor().newInstance()
    if (source == null) {
        return obj 
    }
    return obj.merge(source)
}

inline infix fun <reified T : Any, reified S : Any> T.merge(source: S): T {
    val beanInfo = Introspector.getBeanInfo(source.javaClass)
    val nameToPropertyOfSource = beanInfo.propertyDescriptors.associateBy { it.name }
    val nameToPropertyOfTarget = T::class.memberProperties.associateBy { it.name }

    // 根据目标类的主构造函数创建对象, 不在主构造函数中的var属性在下一步处理, 不在主构造函数中的val属性会略过.
    val primaryConstructor = T::class.primaryConstructor!!
    val primaryConstructorParams = mutableListOf<String>()
    val args = primaryConstructor.parameters.associate { parameter ->
        val name = parameter.name!!
        primaryConstructorParams.add(name)
        val readMethodOfSource = nameToPropertyOfSource[name]?.readMethod
        val propertyOfTarget = nameToPropertyOfTarget[name]
        parameter to (readMethodOfSource?.invoke(source) ?: propertyOfTarget?.get(this))
    }
    val obj = primaryConstructor.callBy(args)

    // 为不在主构造函数中的字段赋值(比如继承的字段)
    nameToPropertyOfTarget.forEach { (name, propertyOfTarget) ->
        // 只处理, 没有在构造函数中出现, 并且是var修饰的属性. 没有在构造中出现的val属性无法处理
        if (!primaryConstructorParams.contains(name) && propertyOfTarget is KMutableProperty1) {
            val mutableProperty = propertyOfTarget as KMutableProperty1<T, Any?>
            val readMethodSource = nameToPropertyOfSource[name]?.readMethod
            if (readMethodSource != null) {
                mutableProperty.set(obj, readMethodSource.invoke(source))
            } else {
                try {
                    mutableProperty.set(obj, propertyOfTarget.get(this))
                } catch (e: Exception) {
                    // lateinit 属性如果没有值, 在这里会报错, 目前还没有找到判断是否有值的方式
                    // 压制异常 e.printStackTrace()
                }
            }
        }
    }
    return obj
}
```


#### 结论
> 现在User构造函数中的属性, 继承到的var属性都能解决. 
> lateinit var以及val属性目前都还未解决, 所以在使用的时候得注意一下.