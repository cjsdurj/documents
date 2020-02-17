### sockshop 调研

#### 架构图
![](https://upload-images.jianshu.io/upload_images/14961220-d39ae5d95abe93ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### payment: 
/health  get 健康检查    ; /paymentAuth  post 授权支付.

支付逻辑：根据商品的amount是否>0,判断支付是否成功。
主观确定主题：1.支付相关，Payment,amount,Authorisation,message... 2.健康检测:. health, check,time,
,mongo,db,app,status...  


#### user:
##### interface:
- /login get
- /register post
- /customers get
- /customers/{id} get,delete

- /customers/{id}/cards get
- /customers/{id}/addresses get
- /cards get,post
- /cards/{id}  get,delete
- /addresses get,post
- /addresses/{id} get.delete

主观确定主题： 1.用户注册登陆相关：register，login，user,password...
          2.客户基本信息管理： firstName，lastName，username，email, card, address,longNum,expires,ccv,street,city,country,postcode..
           3.服务监控：endpoint ...
#### catalogue:
##### interface:
- /catalogue get
- /catalogue/{id} get
- /catalogue/size get
- /tags get

主观确定主题 : 1.商品目录相关： description,price,count,tag,catalogue,item,imageUrl,description,
2.服务监控 endpoint，...

#### cart
##### interface:
- /carts/{customerId} get,delete
- /carts/{customerId}/items post,patch
- /carts/{customerId}/items/{itemId} delete
-/health get 

主观确定主题: 用户购物车相关，item, cart , quantity ,unitprice, customer,
健康检测

#### orders
##### interface:
- /orders  post,get
- /health  get

主观确定主题: 1.创建订单， oreder，item，customer,status, card ,address, submit ,payment,shipment...
2.健康检查:

#### shipping
##### interface:
- /shipping get,post
- /shipping/{id} get 
- /health

主观确定主题： 
物流相关： shipping ,queue,rabbit,...
健康检查：

#### queue-master  
模拟消费 shipping queue
健康检测：


###主题模型运行结果
1. 根据perplexity，确定主题数k值
![](https://upload-images.jianshu.io/upload_images/14961220-b7184052347329db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 设置主题数8：
![](https://upload-images.jianshu.io/upload_images/14961220-0ec4cecb1257a2f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/14961220-f4ff0cbab1cad15d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.预处理过程：



![](https://upload-images.jianshu.io/upload_images/14961220-9dc6c8739544db75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
删除停用词包括 java,go关键字， nltk库提供的常见英文停用词集， csdn查到的比较全的英文停用词集合，以及根据项目情况自己删除了一些词。
![](https://upload-images.jianshu.io/upload_images/14961220-783fe1c796da6cd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)










