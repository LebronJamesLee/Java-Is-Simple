## Java prepare for war ZJ

你简历写再多NB的技术栈、背再多面试题，也没有比你去完全理解一个项目要好！

切记这一点。

> 这里从我学习的一个项目开始说起，我做过后台的武林秘籍管理系统、企业管理系统、电商分布式系统、购物平台等，前前后后做过的项目总共有：7个。发现一点问题就是，学习是归学习，看视频也好，模仿别人也好，自己实现一个完全差不多的系统，不难！但是很少会去思考这个项目的深层次问题，项目会面临哪些问题，该如何去设计和解决。

这里不对上面做过的项目进行分析了，就以我最近学习的一个项目说起吧：

**项目名称：秒杀系统方案优化。**

项目所使用到的所有技术有：

> 前台：html、jQuery、bootstrap、thymeleaf。
>
> 后台：springboot、mysql、MyBatis、Druid、Redis、JSR303、RabbitMQ、nginx。

项目技术点：

> Result结果封装、json输出封装、通用缓存key封装。明文密码两次MD5处理、JSR303参数校验、全局异常处理器、分布式session、页面缓存+URL缓存+对象缓存、redis预减库存、内存标记减少redis访问、rabbitMQ队列缓存、异步下单、秒杀接口地址隐藏、接口防刷。



#### 好了，开始分析吧：

##### 首先呢？我们应该来画个系统架构图（这是一个基础能力，面试官很可能这样要求你的）：

**系统功能图：**

![系统功能图](https://user-images.githubusercontent.com/46838937/65655245-212a4500-e04e-11e9-87c9-42903e24f329.png)



**系统基础架构：**

![系统基础架构图](https://user-images.githubusercontent.com/46838937/65655273-369f6f00-e04e-11e9-8344-881cf4a3178e.png)

**系统逻辑组件图：**

![](https://user-images.githubusercontent.com/46838937/65655112-ad883800-e04d-11e9-993d-5bbb07bb7b80.png)



**控制层方法时序图：**

**LoginController_doLogin：**

![LoginController_doLogin](https://user-images.githubusercontent.com/46838937/65655386-a57cc800-e04e-11e9-958c-361890c1d014.png)



**GoodsController_list:**

![GoodsController_list](https://user-images.githubusercontent.com/46838937/65655423-bf1e0f80-e04e-11e9-9def-eb8718a39460.png)

**GoodsController_detail:**

![GoodsController_detail](https://user-images.githubusercontent.com/46838937/65655456-dfe66500-e04e-11e9-8318-6c59151b6c01.png)



**GoodsController_detail2:**

![GoodsController_detail2](https://user-images.githubusercontent.com/46838937/65655469-ed035400-e04e-11e9-9c75-ed57e5c8ac5e.png)

**MiaoshaController_afterPropertiesSet:**

![MiaoshaController_afterPropertiesSet](https://user-images.githubusercontent.com/46838937/65655493-00aeba80-e04f-11e9-8968-fe9e827e5470.png)

**MiaoshaController_reset:**

![MiaoshaController_reset](https://user-images.githubusercontent.com/46838937/65655549-2e93ff00-e04f-11e9-8a64-33feea28dc7d.png)

**MiaoshaController_miaosha:**

![MiaoshaController_miaosha](https://user-images.githubusercontent.com/46838937/65655580-47041980-e04f-11e9-8119-5d7901fd1f2f.png)

**MiaoshaController_miaoshaResult:**

![MiaoshaController_miaoshaResult](https://user-images.githubusercontent.com/46838937/65655634-73b83100-e04f-11e9-8ade-e9bf6a75260a.png)

**MiaoshaController_getMiaoshaPath:**

![MiaoshaController_getMiaoshaPath](https://user-images.githubusercontent.com/46838937/65655668-8d597880-e04f-11e9-94e2-de046f589fc5.png)

**MiaoshaController_getMiaoshaVerifyCod:**

![MiaoshaController_getMiaoshaVerifyCod](https://user-images.githubusercontent.com/46838937/65655696-a06c4880-e04f-11e9-99bb-35665fe49d22.png)

**OrderController_info:**

![OrderController_info](https://user-images.githubusercontent.com/46838937/65655714-b8dc6300-e04f-11e9-8fc5-76fdd1fa1078.png)

**SampleController_dbGet:**

![SampleController_dbGet](https://user-images.githubusercontent.com/46838937/65655738-d1e51400-e04f-11e9-8873-9e64f2440e33.png)

**SampleController_redisGet:**

![SampleController_redisGet](https://user-images.githubusercontent.com/46838937/65655759-df020300-e04f-11e9-8de9-19d5b96f8fdf.png)

**SampleController_redisSet:**

![SampleController_redisSet](https://user-images.githubusercontent.com/46838937/65655776-ee814c00-e04f-11e9-9b2d-3345b406065b.png)

这样的好处在于哪里呢？

能让你知道在这个项目中你负责了什么、做了什么、担任了什么角色、使用到了那些技术，学会了那些新技术的使用，遇到棘手问题的时候如何解决，利用什么技术实现什么功能。



**项目详细结构：**

```
com.jcn.seckill
​		|-access
			AccessInterceptor 
			AccessLimit
			UserContext
​		|-config
			UserArgumentResolver
			WebConfig
​		|-controller
			GoodsController
			LoginController
			MiaoshaController
			OrderController
			SampleController
			UserController
​		|-dao
			GoodsDao
			MiaoshaUserDao
			OrderDao
			UserDao
​		|-domain
			Goods
			MiaoshaGoods
			MiaoshaOrder
			MiaoshaUser
			OrderInfo
			User
​		|-exception
			GlobalException
			GlobalExceptionHandler
​		|-rabbitmq
			MiaoshaMessage
			MQConfig
			MQReceiver
			MQSender
​		|-redis
			AccessKey
			BasePrefix
			GoodsKey
			KeyPrefix
			MiaoshaKey
			MiaoshaUserKey
			OrderKey
			RedisConfig
			RedisPoolFactory
			RedisService
			UserKey
​		|-result
			CodeMsg
			Result
​		|-service
			GoodsService
			MiaoshaService
			MiaoshaUserService
			OrderService
			UserService
​		|-util
			DBUtil
			MD5Util
			UserUtil
			UUIDUtil
			ValidatorUtil
​		|-validator
			IsMoblie
			IsMoblieValidator
​		|-vo
			GoodsDetailVo
			GoodsVo
			LoginVo
			OrderDerailVo
		MainApplication	
```



**好了现在我们针对该项目来进行问题的回答：**



#### **一、先介绍下你的项目吧。**

> **1.对项目整体的感受：**
>
> 项目的业务逻辑不是很复杂，就一个秒杀的业务，用户登录进入商品的列表页，随后查看商品详情秒杀倒计时，进行秒杀，然后生成订单。根据压测来模仿大并发的环境，随后就通过一些优化手段来提高系统的吞吐率，从而能够去应对大的并发量。
>
> **2.在项目中你负责什么、做了什么、担任什么角色：**
>
> 负责秒杀功能的设计与开发，对功能进行实现，压测系统，然后优化系统。
>
> **3.从这个项目中你学会了哪些东西，使用到了哪些技术，学会了哪些新技术的使用：**
>
> 我从这个项目学会了如何通过消息队列进行异步的下单，还有利用redis来对系统进行缓存优化啊，再对秒杀接口地址做了隐藏，还有一些防刷限流手段。

我的需求分析：

做这个秒杀系统的时候，功能是实现了，在本机测试的时候，刚开始也只是用了100个并发，在这一百个并发中发送10万个请求（3字节的尝试），大概使用1.5秒左右，每秒大概有5万个请求吧，然后使用100字节的请求进行测试，每秒大概可以有6万多的请求。只测SET和PUSH操作，也是大概每秒6-7万的请求量。

刚开始考虑到了使用乐观锁来改进，但是在本地测试中达到5000并发量运行10次的时候就出现了负载超标/13-15，进程几乎被阻塞了，查看了下进程，忽略程序进程和JMeter进程，发现瓶颈出现在了mysql数据库里面，对于应用程序只要做下横向扩展就能解决并发问题，但是数据库就比较麻烦了，第一次测试QPS在600左右，第二次在1200左右，然后使用第二次压测结果作为基准，接着就对秒杀接口进行压测，负载大概在10左右，QPS在1300左右，查看数据库的时候发现出现了**卖超**的情况。

> **秒杀接口：**首先会判断有没有库存，然后判断会不会重复秒杀，接着进行秒杀生成订单。

>  **卖超：** 单线程的时候，程序是没有问题，判断库存，接着判断是否重复秒杀了，这两步是没有问题的，但是在多线程的时候，例如当库存只有1个的时候，突然来了多个用户进行秒杀，判断完库存后，都发现库存还有一个，接着进行是否秒杀了的判断，都没有秒杀过后，那么这多个用户就会同时进行减库存了，然后就出现超卖的情况了。

出现了这些问题后，我就想，要去减少数据库的访问，就应该对页面进行缓存优化（浏览器的缓存、CDN缓存、服务器缓存、redis缓存、页面缓存——url缓存——对象缓存）

让页面静态化，项目前后端分离，静态资源优化，CDN优化。

但是并发量太大的时候，仅仅是对页面进行优化还是不够的，还要对接口进行优化（redis预减库存减少数据库访问，内存标记减少redis访问，rabbitMQ队列缓存、削峰、降流，异步下单，nginx水平扩展）

然后进行压测，发现mysql负载变小了很多，负载大多是在redis里。

由于本地服务器性能受限，所以不足以完全对大并发量进行准确的评测。

> 如何更加准确的评估：将mysql、redis、MQ、项目应用程序  放入到不同的服务器上。

以第二次压测数据为基准，还是以5000*10这个数据进行压测，QPS为2100左右。

收本地的硬件所限制，效果可能不是很明显。

对系统进行了优化了只有，还要做一些安全措施：

> 秒杀接口地址隐藏
>
> 数学公式验证码
>
> 接口的防刷限流



#### **二、项目可问知识点：**

**悲观锁、乐观锁，项目中如何实现？**

> 

**缓存雪崩什么情况下会发生？如何避免？**

> 

**更新数据库的同时为什么不马上更新缓存，而是删除缓存？**

> 

**数据库：索引、慢查询优化。**

> 

**Java多线程/并发包：J.U.C**

> 

**如何考虑你的项目架构的呢？**

> 

**为什么秒杀地址在开始前不应暴露给用户？**

> 

**我现在是个黑客，我在秒杀开始时写好了脚本，运行一万个线程获取秒杀地址，这样是不是也不公平呢？**

> 

**LRU缓存**

> 

**那我可不可以创建一个ip代理池和很多用户来抢购呢？假设我有很多手机号的账户？**

> 

**把生成订单和减库存两条sql语句放在一个事务里，都操作成功了则认为秒杀成功。你这个订单表和商品库存表是在一个数据库的吧，那如果在不同的数据库中呢？分布式锁、消息队列。**

> 

**你有没有考虑到一种情况，假如说你的缓存刚刚失效，大量流量就来查缓存，你的数据库会不会炸？消息队列来解决，对流量削峰填谷。**

> 



#### **三、如何实现你说的这些优化呢？**

> **如何解决超卖的情况？**
>
> 页面进行缓存优化（浏览器的缓存、CDN缓存、服务器缓存、redis缓存、页面缓存——url缓存——对象缓存）让页面静态化，项目前后端分离，静态资源优化，CDN优化。
>
> **如何做接口优化？**
>
> （redis预减库存减少数据库访问，内存标记减少redis访问，rabbitMQ队列缓存、削峰、降流，异步下单，nginx水平扩展）
>
> **安全措施：**
>
> 秒杀接口地址隐藏、数学公式验证码、接口的防刷限流。



**四、项目技术的衍生？**

> redis、rabbitmq、nginx、mysql。

















