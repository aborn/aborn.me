# ehcache调用

@(技术笔记)

[ehcache](http://www.ehcache.org/)是非常优秀的、轻量级的、本地缓存方案，它可以解决大并发情况下静态资源的快速保存与访问。

## 引入ehcache jar包
这里引入最新版本的ehcache jar包：
```
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>2.10.0</version>
    <type>pom</type>
</dependency>     
```
## 基本概念
**1. CacheManager**
CacheManager是缓存的管理器，使用ehcache必需创建一个CacheManager，它的创建方式有两种：
a. 单例模式：
```
private static volatile CacheManager cacheManager = CacheManager.create()
```
b. ehcache 2.x以后允许多个cacheManager存在，可以通过.xml配置新的cacheManager，如下demo-ehcache.xml
```
<ehcache name="DemoCacheManager">
</ehcache>
```
通过.xml文件初始化非单例模式的cacheManager，如下：
```
    private static volatile CacheManager cacheManager = CacheManager.newInstance(this.class.getClassLoader().getResourceAsStream("demo-ehcache.xml"));

```

**2. cache(缓存)**
一个CacheManager可以管理多个缓存，每个缓存在内存中都有其自己的属性，例如缓存元素（Element）的最大个数、缓存在内存中的最长存活时间、缓存的淘汰算法等。一个cache可以存储多个缓存元素（Element）。缓存在使用前都要进行注册，注册有两种方式：
* 通过xml配置文件注册。配置文件可配置缓存的名字，缓存在堆中的最大个数，缓存的淘汰算法等，参数非常之多，下面是一个最简单的例子：
```
<ehcache>
    <cache name="demo"
           maxEntriesLocalHeap="10000"
           eternal="false"
           timeToIdleSeconds="300"
           timeToLiveSeconds="600"
           memoryStoreEvictionPolicy="LFU"
            >
    </cache>
</ehcache>
```
上面配置了一个缓存名为demo的缓存，它在内存中的最大个数为10000个，最长存活时间timeToLiveSeconds为10分钟(600s)，最长可访问时间timeToIdleSeconds为5分钟(300s)。当个数超过最大个数限制时采用LFU算法进行淘汰。eternal表示缓存是否永久不失效，当这个值为true时，上面的最大存活时间和最长可访问时间无效。
* 通过代码动态注册
下面是一个例子，与通过xml文件注册效果一样：
```
  Cache newMemCache = new Cache("demo",
          1000,         // 内存中最大元素个数
          MemoryStoreEvictionPolicy.LFU,   // 淘汰策略
          false,                 // 是否存到disk
          "/data/appdatas",                    // 存在disk的位置
          fale,   // 是否永久有效
          600,    // timeToLiveSeconds
          300,    // timeToIdleSeconds
          false,  // 是否在JVM重启的时候,是否保存到disk
          60 * 60,  // 多长时间跑一次disk超时的线程(这个参数没用)
          null    // 注册事件
  );
  cacheManager.addCache(newMemCache);
```
**3. Element缓存元素**
缓存元素是存储数据的最小单位，由其ElementKey和对象组成，如下定义：
```
Element element = new Element(elementkey, object);
```
注意：这里的object可以为任意对象！！

## 调用说明
**1. add操作**
往缓存中添加Element,如下：
```
  Cache cache = cacheManager.getCache("demo");
  if (cache != null) {
      Element element = new Element("demoElementKey", "demo object");
      cache.put(element);
  }
```

**2. get操作**
从缓存中取元素
```
  Cache cache = cacheManager.getCache("demo");
  if (null != cache) {
      Element element = cache.get("demoElementKey");
  }
```

## 具体例子
具体例子请参考我写的[ehcahce-client](https://github.com/aborn/ehcache-client/blob/master/src/main/java/org/popkit/mobile/ehcache/EhCacheService.java)
