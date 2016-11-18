title: Retrofit、GsonFormat、RxJava、RxAndroid简单实践
date: 2016-06-15 22:11:54
categories: Retrofit
tags: [Java,RxJava,Android,Retrofit,RxAnroid,GsonFormat]
description: rxjava、java、android、Retrofit、RxAndroid


---

最近看了简书上很多说retrofit与rxjava的文章，所以今天我也来讲下我是如何用这些最新的东西的。废话不多说，直接上代码

<!--more-->

### 引入依赖
开发工具肯定是androidstudio，这点以后都不用说了吧，因为现在github上很多东西都是支持androidstudio，目前使用感觉很方便，简单一行，就能把所有的功能引入进来。接下来我会用到rxjava rxandroid retrofit rxlifecycle  具体的这些事什么请自行去查阅，最后的时候，我会放在相关具体介绍的链接
###### 说明
下面一些*version是我在gradle中定义的变量，防止我每次都要改多次的问题

	ext {
    	butterknifeVersion = '8.0.1'
    	supportVersion = '23.2.1'
    	retrofit2Version = '2.0.2'
    	rxlifecycleVersion = '0.5.0'
	}
下面的拷贝项目的build.gradle 而不是工程的里面（又啰嗦了）

    compile 'io.reactivex:rxjava:1.1.3'
    compile 'io.reactivex:rxandroid:1.1.0'
    compile "com.squareup.retrofit2:retrofit:${retrofit2Version}"
    compile "com.squareup.retrofit2:converter-gson:${retrofit2Version}"
    compile "com.squareup.retrofit2:adapter-rxjava:${retrofit2Version}"
     compile "com.trello:rxlifecycle:${rxlifecycleVersion}"
    compile "com.trello:rxlifecycle-components:${rxlifecycleVersion}"
       
###### rxlifecycle说明
此处使用rxlifecycle来处理一些订阅的回收
>[https://github.com/trello/RxLifecycle](https://github.com/trello/RxLifecycle)就使被用来严格控制由于发布了一个订阅后，由于没有及时取消，导致Activity/Fragment无法销毁导致的内存泄露。

既然我们要使用rxlifecycle就要按照它的规则来，我把目前的我的BaseActivity 继承 RxAppCompatActivity。具体使用的时候，我再说

---
### GsonFormat使用
假如服务端给我个接口，该接口返回json数据。

   
    接口地址：http://ip:端口/customer/getAppCutomerList 
    请求方式：GET
    请求参数(header):
    参数名		   类型	   必填	参数位置		描述
    accessToken	 string	 是	 header		token
    -------------------------------------------
    请求参数(urlParam):
    参数名		 类型	   必填	  参数位置	 描述
    login_name	string	是		urlParam	登录名
    pageNumber	int	   是		urlParam	页数
    name		  string	是		urlParam	查询参数
    
 找个网络请求工具，我用的是Postmain谷歌浏览器的插件,填上对应的数据，按下send就能拿到对应的json
   ![postmain](http://o8wlu6q9f.bkt.clouddn.com/5131D0FD-7FE4-40DB-81AD-283CC597D00C.png)
   好了，拿到服务端返回json可以创建实体了
   先随便创建个实体，我这里创建CustomerReturnBean，直接在实体名称上，点击右键generate...选择GsonFormat（前提：你已经装了这个插件）填入上面返回的json，看看对应的属性类型是否是你想要的，没有问题，点击ok，生成对应的实体。
   
### Retrofit实例创建
类：AccountsService

    public class AccountsService {

    private static Retrofit.Builder retrofitBuilder = new Retrofit.Builder()
            .baseUrl(UrlContant.getBaseUrl())
            .addConverterFactory(GsonConverterFactory.create())
            .addCallAdapterFactory(RxJavaCallAdapterFactory.create());

    public static AccountsApi createAccountsService() {
        return retrofitBuilder.build().create(AccountsApi.class);
    }
    }
对应的一些请求方法接口：

	public interface AccountsApi {
    /**
     * 客户搜索查询接口
     *
     * @param loginName
     * @param pageNumber
     * @param name       查询关键字
     * @return
     */
    @GET(UrlContant.searchCustomer)
    Observable<CustomerReturnBean> searchCustomer(@Query("login_name") String 	loginName, @Query("pageNumber") int pageNumber, @Query("name") String 	name,@Header("accessToken") String accessToken);
    }
因为我这里很方法都需要login_name accessToken,这个是登录的时候，获取到保存在本地的。我这里设置个辅助类：
类：AccountsApiHelper

    public class AccountsApiHelper {

    private static AccountsApiHelper instance;
    private static AccountsApi service;
    private String userName;
    private String accessToken;

    public static synchronized AccountsApiHelper getInstance() {
        if (instance == null) {
            instance = new AccountsApiHelper();
        }
        //初始化一些参数
        instance.init();
        return instance;
    }

    private void init() {
        if (service == null) {
            service = AccountsService.createAccountsService();
        }

    }

    private void getValue() {
        userName = AppContext.getInstance().getLoginName();
        accessToken = AppContext.getInstance().getAccessToken();
    }
    /**
     * 客户搜索查询接口
     * @param pageNumber
     * @param searchValue
     * @return
     */
    public Observable<CustomerReturnBean> searchCustomer(int pageNumber,String searchValue){
        getValue();
        return service.searchCustomer(userName,pageNumber,searchValue,accessToken);
    }
    }
 这里实例构建完成了
 
### Retrofit+RxJava请求数据
    private void loadData(int pageNumber,final boolean isRefresh,String searchValue) {
        //上面创建的接口
        AccountsApiHelper.getInstance().searchCustomer(pageNumber,searchValue)
                    .subscribeOn(Schedulers.io())
                    .compose(this.<CustomerReturnBean>bindToLifecycle()) //这里用到rxlifecycle
                    .doOnSubscribe(new Action0() { //处理一开始需要的进度条加载，一开始需要的ui里面执行的
                        @Override
                        public void call() {
                            if (!customerWsrefresh.isRefreshing()) {
                                ViewUtils.setViewVisibility(commonLoading, true);
                                newton_cradle_loading.start();
                            }
                        }
                    })
                    .subscribeOn(AndroidSchedulers.mainThread()) //这里的subscribeOn为上面的doOnSubscribe设定执行线程
                    .observeOn(AndroidSchedulers.mainThread())
                    .doOnTerminate(new Action0() { //结束执行 关闭进度加载
                        @Override
                        public void call() {
                            ViewUtils.setViewVisibility(commonLoading, false);
                            newton_cradle_loading.stop();
                        }
                    })
                    .observeOn(AndroidSchedulers.mainThread())
                    .subscribe(new Action1<CustomerReturnBean>() {
                        @Override
                        public void call(CustomerReturnBean customerReturnBean) {
                            //成功执行
                            handlerSuccess(customerReturnBean, isRefresh);
                        }
                    }, new Action1<Throwable>() {
                        @Override
                        public void call(Throwable throwable) {
                            handlerFailure(isRefresh);
                        }
                    });
    }
    

这里拿到数据，具体后面该怎么展示，各位看官随意。

各位感觉上面有什么错误和建议，欢迎提问谢谢
### 补充：
有时候，我们需要延迟执行和放重复点击可以用如下：

    .throttleFirst(10,TimeUnit.SECONDS)//重复点击处理
    .delay(manualCheck?0:3, TimeUnit.SECONDS)//延迟执行

### 开源让世界更美好   
### 感谢
[给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)

[Rxlifecycle使用详解，解决RxJava内存泄露问题](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/1122/3711.html)
