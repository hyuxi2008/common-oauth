## 此解决方案用于解决微信等开放平台授权域名只能配置一个的问题
## 背景：
商户接入微信公众号支付的时候需要在微信配置授权域名，而这个授权域名只能配置一个，公众号支付需要先获取用户的授权   
获取授权的过程中需要先获取code,同时提供一个redirect_uri供授权成功后回调，这个redirect_uri必须是在这个授权 
域名下，比如我们配置的域名是abc.com，则redirect_uri必须类似 https://abc.com/a/b
## 面临的问题
我们一般开发过程中都会有多套环境，比如最基本的测试环境&生产环境，而且一般测试环境和生产环境域名不同，这样就会 
导致测试过程中需要切换授权域名，而我们一旦发布到生产环境后，再想测试就难啦，如果我们直接改成测试域名去测试就  
会导致生产环境无法正常运行，另外由于我们配置授权域名的过程中，微信会要求我们从微信那下载一个认证文件然后放到  
域名的根目录下，这样就需要分别在测试环境和生产环境去配置，流程特别繁琐 
## 解决方案
既然只能设置一个域名，那我能不能专门搞个域名做这个事情呢，授权域名设置成我的域名，所有请求微信授权的请求都经过 
我，同时微信回调的时候也先回调我，我再回调到业务系统，于是就有了此解决方案，两种方案的对比如下：  
传统授权逻辑  
![传统授权逻辑](https://note.youdao.com/yws/api/personal/file/WEB6701953298f9df163062f54d0e248f80?method=download&shareKey=1a204a0d04db9fc7b2ca56a555435e31)  
改进版授权逻辑  
![改进版授权逻辑](https://note.youdao.com/yws/api/personal/file/WEB56f43bb3eaa813556a7fb3889c10791e?method=download&shareKey=dedd44dd3fedc220071d0827edff62bf)  
## 如何使用
本系统基于spring boot构建了一个轻量级的工程，开发者只需要修改下application.yml中的配置，主要是oauth.domain 
启动端口和url可以按需修改，然后直接启动此jar包，启动方式：java -jar common-oauth-1.0.jar,然后让运维人员配置 
nginx，将特定url指向此服务的ip&port即可

