# API

基于RESTFul的设计思想,使用lumen框架实现。

[框架文档](https://lumen.laravel-china.org/docs/5.3) 


## 域名


## HTTP 状态码

[完整状态码查看](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

| 状态码 | 描述 |
|:-----:|:---------|
| 200   | **[GET]** 服务器成功返回用户请求的数据 |
| 201   | **[POST/PUT/PATCH]** 用户新建或修改数据成功 |
| 202   | **[*]** 表示一个请求已经进入后台排队（异步任务）|
| 204   | **[DELETE]** 用户删除数据成功 |
| 400   | **[POST/PUT/PATCH]** 用户发出的请求有错误，服务器没有进行新建或修改数据的操作 |
| 401   | **[*]** 表示用户没有权限（令牌、用户名、密码错误）|
| 403   | **[*]** 表示用户得到授权（与401错误相对），但是访问是被禁止的 |
| 404   | **[*]** 用户发出的请求针对的是不存在的记录，服务器没有进行操作 |
| 406   | **[GET]** 用户请求的格式不正确（如:用户请求JSON格式，但是只有XML格式）|
| 410   | **[GET]** 用户请求的资源被永久删除(如:API删除) |
| 422   | **[POST/PUT/PATCH]** 当创建一个对象时，发生一个验证错误。 |
| 500   | **[*]** 服务器错误，无法判断发出的请求是否成功。 |

## 目录约定
这里只描述涉及业务功能的核心目录及文件
* `/app/config` 配置文件目录,所有配置文件模类型命名,如:database.php表示DB配置
* `/app/Http/routes.php` 路由地址
* `/app/Http/Controllers/` 控制器
* `/app/Http/Requests/` 参数验证,规则验证
* `/app/Http/Middlewarre/` 中间件
* `/app/Http/Listeners/` 监听
* `/app/Http/Modules/*` 模块业务逻辑,按模块创建目录 *表示对应内部模块如用户模块：Users 不强制复数
    * `Facades/` 门面类,如非特别复杂模块门面类只会有一个,注意控制门面内部的大小
        * `*ServiceProvide.php` 服务提供者,`*`为模块名 模块中包含多个字模块的统一一个注册门面
        * `*Facade.php` 服务提供者,`*`为提供的门面
        * `*.php` `*`必须为有代表意义的单词, 可以多个
    * `Exceptions/` 异常处理,非必须
    * `Repositories`
        * `*Repository.php` 业务逻辑核心类,该类为逻辑汇总类
        * `*Trait.php` 内部继续的父类
        * `*.php` 其它相关业务处理类,数量可能会较多,`*`必须为有代表意义的单词
    * `Events` 事件触发参数定义
    * `Listeners` 事件执行逻辑
    * `Models` 数据ORM + 模块中的枚举类Model模型等
    * `apidoc.json` 当前模块的Api文档
    * `routes.php` 当前模块的路由
* `/app/database/migrations` DB变更版本记录,必须存在

## 命名约定

### 全局规则
* `URL地址` 一级资源名/二级资源...,如:company 或 company/invite,全小写
* `文件夹` 首字母大写其余小写
* `命名空间` 驼峰命名规则
* `类名` 驼峰命名
* `方法名` 小驼峰
* `参数、变量`  小驼峰、内部私有参数统一 "_"下划线开头+小驼峰
* 

### 规则示例
* `API地址` 为 `company` 或 `company/invite` 格式,其中`company`为一级资源,`invite`为二级资源,以此类推
* `控制器` 为 `*Controller.php` 格式,如:`CompanyController.php`
* `请求验证` 为 `CreateCompanyPost` 其中`Create`=方法名,`Company`=资源名,`Post`=请求类型
* `Facade` 只有一个时与模块同名,多个Facade类时以`名词全名`,尽量简短门面为外部唯一入口,引用多

## 代码示例

### 控制器与验证规则
在`Http`中创建`request`目录,验证类放在此目录下。在控制器中以类型注入的方式注入`Request`类,控制器方法在执行里会自动验证证。如验证失败服务端会返回`422`的状态码。

`CreateCompnayPost` 这个是验证类,验证类名字必须据有代表意义。

    public function create(CreateCompnayPost $request)
    {
        ...控制器
    }
 

