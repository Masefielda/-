# 天天生鲜项目实战

------

## 1.  需求分析
    类似于京东的生鲜超市，对于用户而言能够实现登陆，注册，商品浏览，购买等，对于商家而言，实现商品的管理，即添加，修改，删除等.
    

**1.1 业务员平台**

    线下：负责商品的信息采集，购买。并将检验过的商品信息归类后交于后台管理人员
        具体商品信息大致如下：
                    新鲜水果 商品的种类名称
                         |----草莓：具体商品名称，价格，描述，单位
                         |----苹果等
                    海产海鲜 
                         |----三文鱼：价格，描述，单位
                         |----扇贝等
                         
**1.2 管理员**

    线下：与业务员对接商品信息，并在此进行审核
    线上：利用后台管理平台进行商品信息的增删改查。
    
**1.3 用户**


    线上：
        具体功能
            身份验证
                |----用户的注册，登录，用户中心
            商品浏览
                |----购物网站首页，某一类商品列表页，具体商品页
            商品购买
                |----商品加入购物车，订单提交，购买

------

## 2. 实现方式

web app：
根据需求将web app分为四块

    daily_fresh_demo
    ----deily_fresh_demo
    ----df_cart    #对商品购物车管理
    ----df_goods　　#商品以及后台管理
    ----df_user    #用户管理
    ----df_order   #订单管理

------

##3. 开发环境：

> * Ubuntu>1.82
> * python>3.5
> * Django>1.8.2
> * pychram
> * navicate  #数据库可视化工具
> * mysql 5.7



------

## 4. model数据库设计：

将具体数据表分化到具体的app中

```
app
    1、df_goods
        # 商品分类信息  水果 海鲜等
        TypeInfo:ttitle # 名称
        # 具体商品信息
        GoodsInfo：gtitle名称 gpic图片 gprice价格 gunit库存 gclick点击量 gjianjie简介 gkucun库存 gcontent介绍 gtype分类(TypeInfo)
        
    2、df_user
        # 用户信息
        Userinfo: uname名字 upwd密码 uemail邮箱 ushou收货地址 uaddress地址 uyoubian邮编 uphone电话
        # 商品浏览：用户浏览过的商品
        GoodsBrowser: user用户名(UserInfo) good商品(GoodsInfo)
        
    3、df_order
        # 大订单：比如一个大订单中包含两斤橘子，三斤苹果等小订单
        OrderInfo：oid订单号 user用户名（UserInfo） odate订单日期 oIspay是否支付 ototal订单总价 oaddress订单收货地址
        小订单：两斤橘子等
        OrderDetailInfo：goods商品（GoodsInfo） order属于哪个大订单(OrderInfo) price价格 count数量
        
    4、df_cart
        # 购物车
        CartInfo
        user用户名(UserInfo) goods商品(GoodsInfo) cou量
```
------
##5. 项目生成
添加环境变量
打开cmd进入工作空间执行命令：

    django-admin.py startproject daily_fresh_demo
执行之后便可以看到工作空间里有两层daily_fresh_demo文件
```
daily_fresh_demo
　　　　----deily_fresh_demo
　　　　----__init__.py
　　　　----settings.py
　　　　----urls.py
　　　　----wsgi.py
```
------
##6.配置数据库

6.1、使用sqlite
这里由于数据量较少的关系使用本地自带的sqlite

**使用mysql：**

前提是要在安装pymsql，cmd中输入

    pip install pymysql

**设置中**

        'ENGINE': 'django.db.backends.mysql',   # 数据库引擎
        'HOST'# 主机
        'PORT'# 数据库使用的端口
        'USER'# 数据库用户名
        'PASSWORD'# 密码
        'NAME'# 你要存储数据的库名，事先要创建之
       


------
##7. 创建相应的app

    python manage.py startapp df_user
    
    python manage.py startapp df_order
    
    python manage.py startapp df_cart
    
    python manage.py startapp df_goods
    
**工程目录结构如下**

![tool-editor](https://images2018.cnblogs.com/blog/1367382/201806/1367382-20180627093113442-2004072605.png)

------
## 8. 数据models建立


需要注意的是：开发过程中大多是首先设计用户models的。


ORM说的专业一点就是对象关系映射，操作数据库不需要写sql，而是写成函数由Django映射翻译成sql语句


**8.1 执行迁移**
最后在Terminal终端先后执行

    # 1. 创建更改的文件
    python manage.py makemigrations
    # 2. 将生成的py文件应用到数据库
    python manage.py migrate
进行迁移即可。


**8.2  执行结果**

在navicate就可以看到我们想要的结果，相应的数据表

![tool-editor](https://images2018.cnblogs.com/blog/1367382/201807/1367382-20180723204322077-904482322.png)

以“df_”开头的为我们自己创建的，其他为django系统自动生成的。
