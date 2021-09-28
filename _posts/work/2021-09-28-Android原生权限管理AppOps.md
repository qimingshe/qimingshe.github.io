1.AppOps简介

AppOps全称是Application Operations，类似我们平时常说的应用程序的操作（权限）管理。AppOps是Google原生Android包含的功能，但是Google在每次版本更新时都会隐藏掉AppOps的入口。

在今年的Google IO大会上，Google透露Android M ( Android 6.0 )会加入Application Permission Manage的功能，该功能应该就是基于AppOps实现的。

注意：AppOps虽然涵盖了App的权限管理，但是Google原生的设计并不仅仅是对“权限”的管理，而是对App的“动作”的管理。我们平时讲的权限管理多是针对具体的权限（App开发者在Manifest里申请的权限），而AppOps所管理的是所有可能涉及用户隐私和安全的操作，包括access notification, keep weak lock,  activate vpn, display toast等等，有些操作是不需要Manifest里申请权限的。

2.功能效果

Setting UI：

AppOps的权限设置是在系统的Settings App里, Settings -> Security -> AppOps.

点击某一app，可以查看该app的权限管理详情



如前面所说，这一入口默认已经被google屏蔽了，而且屏蔽的手段越来越严格，很多辅助打开工具已经不好用了~

但也有个别厂商重新打开了入口，也可能改了名字~

使用效果：

AppOps默认给用户提供了两个设置选项：

允许该项权限/禁止该项权限

而其实代码逻辑里，有三种可选项：

允许/禁止/提示

用户选择“提示”选项，则该app在执行这一操作时，系统会给用户相应的提示，待用户选择后app继续执行。

我修改源码把appops的“提示”设置项重新打开后，效果如下：

(禁止百度地图的定位权限)


3.AppOps总体概览

核心服务：AppOpsService

系统服务，系统启动时该服务会启动运行。

参考以下ActivityManagerService.java，ActivityManagerService启动过程中：


配置文件：appops.xml  appops_policy.xml

Appops.xml位于/data/system/目录下，存储各个app的权限设置和操作信息。

Appops_policy.xml位于/system/etc/目录下，该文件只在appops strict mode enable时才会存在和使用。（根据源码的描述是这样的，还没有具体分析内容）

API接口：AppOpsManager

AppOpsService实现了大部分的核心功能逻辑，但它不能被其他模块直接调用访问，而是通过AppOpsManager提供访问接口。

UI层：AppOpsSummary，AppOpsCategory等

上传UI显示以及基本逻辑处理。

4.结构图


AppOps整体的工作框架基本如下：


Setting UI通过AppOpsManager与AppOpsService交互，给用户提供入口管理各个app的操作。

AppOpsService具体处理用户的各项设置，用户的设置项存储在/data/system/appops.xml文件中。

AppOpsService也会被注入到各个相关的系统服务中，进行权限操作的检验。

各个权限操作对应的系统服务（比如定位相关的Location Service，Audio相关的Audio Service等）中注入AppOpsService的判断。如果用户做了相应的设置，那么这些系统服务就要做出相应的处理。

（比如，LocationManagerSerivce的定位相关接口在实现时，会有判断调用该接口的app是否被用户设置成禁止该操作，如果有该设置，就不会继续进行定位。）

5.相关API接口


尽管在Android SDK里能够看到部分AppOps的API接口，但是Google对此解释的很清楚：

This API is not generally intended for third party application developers; most features are only available to system applications. Obtain an instance of it throughContext.getSystemServicewithContext.APP_OPS_SERVICE.

即是说，这些API不是让第三方app使用的，而是供系统应用调用的。

使用Android SDK开发应用，如果要调用这些api的话，也会编译不通过。

但是想使用的话，可以尝试把Android源码里AppOpsManager.java打包一下，把jar包导入自己的工程，就可以使用了。

部分重要的API接口如下：

int

checkOp(Stringop, int uid,StringpackageName)

Op对应一个权限操作，该接口来检测应用是否具有该项操作权限。

int

noteOp(Stringop, int uid,StringpackageName)

和checkOp基本相同，但是在检验后会做记录。

int

checkOpNoThrow(Stringop, int uid,StringpackageName)

和checkOp类似，但是权限错误，不会抛出SecurityException，而是返回AppOpsManager.MODE_ERRORED.

int

noteOpNoThrow(Stringop, int uid,StringpackageName)

类似noteOp，但不会抛出SecurityException。

void setMode( int code, int uid, String packageName, int mode)

这个是我们最需要的方法，改变app的权限设置，但偏偏被google隐藏了。

code代表具体的操作权限，mode代表要更改成的类型（允许/禁止/提示）

正常情况下（如果OEM厂商没有做特殊处理），把AppOpsManager.java打包，引入jar包到工程内，是可以使用上述API接口的，

也即是可以自行设计UI，提供入口来改变app权限。

作者：PixelTogether
链接：https://www.jianshu.com/p/a26f0dd024a6
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
