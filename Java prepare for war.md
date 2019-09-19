## Java prepare for war

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

![](https://github.com/NolanJcn/Java-Is-Simple/blob/master/%5Bimg%5DJava%20prepare%20for%20war/%E7%B3%BB%E7%BB%9F%E5%8A%9F%E8%83%BD%E5%9B%BE.png?raw=true)





**系统基础架构：**





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



