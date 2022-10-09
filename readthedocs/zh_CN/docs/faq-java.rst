Java SDK FAQ
============

即Maven上下载的weidentity-java-sdk

----------------

- weidentity-java-sdk 跟能在哪些版本的 JDK 下运行?

详见\ `WeIdentity Java SDK 与各版本JDK的兼容性 <./weid-compatibility-test.html>`__\ 。


--------------

-  **智能合约和 SDK 版本号相关问题**

详见\ `版本号管理 <./styleguides/versioning-management.html>`__\ 。

--------------

-  **Smart Contract（智能合约）的如何升级？**

WeIdentity 智能合约是按分层设计的，分为：\ *数据层，逻辑层，权限层*\ 。
数据层合约被当做类似数据库的表来使用，充分预留的字段，所以数据层合约基本稳定。
在大多数情况下，WeIdentity 智能合约的升级的时候只会升级逻辑层和权限层，
所以升级的时候之前的数据完全保留，部署新的合约（需要传入升级前数据合约的合约地址）后，直接使用新的合约即可。
如果需要升级数据层合约，则需要做数据迁移或者在逻辑层做特殊逻辑。更多信息请看 \ `WeIdentity智能合约设计 <./weidentity-contract-design.html>`__\ 。

--------------

-  **如何查看当前 WeIdentity JAVA SDK（weid-java-sdk）的版本号？**

三种方式可以查看 SDK 的版本号：

1. 下载的jar包会带有版本号，例如：\ ``weidentity-java-sdk-1.0.1.jar``\ ，版本号是v1.0.1。
2. 通过 ``MANIFEST.MF`` 查看，可以运行下面的命令，查看输出项
   ``Manifest-Version`` 显示的版本号：

   .. code:: shell

       unzip -p weid-java-sdk-1.0.1.jar META-INF/MANIFEST.MF

3. 如果是下载的源代码，则项目的 ``build.gradle`` 文件里面的变量
   ``version`` 定义了 SDK 的版本号。

--------------

-  **Authoritzation是什么？**

这里要区分两种授权类型：

1. 一是 WeIdentity DID
   层面的授权，即实体可以（暂时地）将自己的身份的部分或者全部控制权授权给另一个实体，另一个实体可以代替原实体进行相关的操作的过程。
2. 用户在A机构的数据需要流转到B机构，需要得到用户的授权，这是不同于1的机制。例如用户的处方信息存储在机构A，需要流转到机构B，这个时候用户可以授权这个数据流转，并叫授权操作上链。

--------------

-  **执行某些接口的时候为什么要传入私钥？**

调用合约的时候需要调用方（WeIdentity DID持有者）的私钥，用来签名交易。签发Credential的时候亦然。

--------------

-  **目前支持哪些密钥管理套件？**

目前，我们使用ECDSA
SECP256套件进行密钥生成、签名验证操作。未来会加入国密版本套件。

--------------

-  **为什么要使用 tx.origin？我听说它有很多问题！**

使用tx.origin是因为比起传统的RBAC/ABAC（Role/ABAC Based Access
Control），WeIdentity的权限控制合约需要支持更大的可扩展性，以支持更多公众联盟链的参与成员实现不同的控制合约。在这种情况下，只有tx.origin才能稳定地追踪到调用者的WeIdentity
DID，也即“WeIdBAC”（WeIdentity Based Access
Control）。此外，在一个不需要token的区块链世界里，tx.origin有着比msg.sender更广泛的适用性。

--------------

-  **如何运行JAVA SDK单元测试？**

详见\ `如何运行单元测试 <./how-to-run-unit-test.html>`__\ 。

--------------

- **如何在无Internet外网连接状态下部署WeIdentity？**

详见\ `网络访问受限时的部署方法 <./how-to-run-without-internet.html>`__\ 。

--------------

- **我原来的配置文件是applicationContext.xml，现在怎么兼容properties？**

详见\ `从ApplicationContext.xml兼容properties的配置方式 <./from-application-context-to-properties.html>`__\ 。

--------------

- **我使用的是maven方式集成，怎么集成WeID SDK相关依赖？**

请您参考源代码根目录下build.gradle的依赖，按照以下格式添加到您的pom.xml里。添加之前请先确定要使用的SDK版本。

   .. code:: shell

       <dependency>
           <groupId>com.webank</groupId>
           <artifactId>weid-java-sdk</artifactId>
           <version>1.4.0</version>
       </dependency>

--------------

-  **同一个节点证书node.crt，是否可以放到不同的节点上？**

不可以。每个节点证书都有一个唯一的node id，作为节点的唯一标识，不同的节点不能用相同的node id，如果把一个node.crt放到多个节点上，由于node id的冲突，会导致链直接起不来。


--------------

-  **多个resources目录需要注意！**

多个resources目录需要注意，只会有一个resources生效，后面的会覆盖前面的，注意把配置文件放入正确的resources下，不然会出现因为读取不到配置文件而不能上链问题。

--------------

-  **weid-java-sdk 1.6.7/1.7.0使用问题 **

weid-java-sdk 1.6.7/1.7.0对应的weid-build-tools版本分别为1.0.20/1.0.21，在使用weid-build-tools此两个版本的时候会出现无法查询WeID列表问题。
解决方案下载对应的特定版本替换原fisco-bcos版本：
Centos系统下载地址： https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/WeIdentity/fisco-bcos-getBlockReceipts.tar.gz
Ubuntu系统下载地址： https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/WeIdentity/fisco-bcos.tar.gz

报错如下:

   .. code:: shell
java.io.IOException: METHOD_NOT_FOUND: The method being requested is not available on this server
        at org.fisco.bcos.web3j.protocol.channel.ChannelEthereumService.send(ChannelEthereumService.java:81) [web3sdk-2.4.4.jar:?]
        at org.fisco.bcos.web3j.protocol.core.Request.send(Request.java:90) [web3sdk-2.4.4.jar:?]
        at com.webank.weid.service.impl.engine.fiscov2.WeIdServiceEngineV2.getTransactionReceipts(WeIdServiceEngineV2.java:606) [weid-java-sdk-1.7.0.jar:?]
        at com.webank.weid.service.impl.engine.fiscov2.WeIdServiceEngineV2.getWeIdListByBlockNumber(WeIdServiceEngineV2.java:621) [weid-java-sdk-1.7.0.jar:?]
        at com.webank.weid.service.impl.engine.fiscov2.WeIdServiceEngineV2.getWeIdList(WeIdServiceEngineV2.java:721) [weid-java-sdk-1.7.0.jar:?]
        at com.webank.weid.service.impl.WeIdServiceImpl.getWeIdList(WeIdServiceImpl.java:953) [weid-java-sdk-1.7.0.jar:?]
        at com.webank.weid.service.BuildToolService.getWeIdList(BuildToolService.java:337) [weid-build-tools-1.0.21.jar:?]
        at com.webank.weid.controller.BuildToolController.getWeIdList(BuildToolController.java:426) [weid-build-tools-1.0.21.jar:?]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_201]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_201]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_201]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_201]

--------------

-  **weid-java-sdk 1.7.1及以上版本使用问题**

weid-java-sdk 1.7.1对应的weid-build-tools版本为1.0.22，在使用weid-build-tools-1.0.22及以上版本时，需要配套使用FISCO-BCOS2.7.1及以上版本。

报错如下:
   .. code:: shell
java.io.IOException: METHOD_NOT_FOUND: The method being requested is not available on this server
        at org.fisco.bcos.web3j.protocol.channel.ChannelEthereumService.send(ChannelEthereumService.java:81) [web3sdk-2.6.3.jar:?]
        at org.fisco.bcos.web3j.protocol.core.Request.send(Request.java:90) [web3sdk-2.6.3.jar:?]
        at com.webank.weid.service.impl.engine.fiscov2.WeIdServiceEngineV2.getTransactionReceipts(WeIdServiceEngineV2.java:606) [weid-java-sdk-1.7.1.jar:?]
        at com.webank.weid.service.impl.engine.fiscov2.WeIdServiceEngineV2.getWeIdListByBlockNumber(WeIdServiceEngineV2.java:621) [weid-java-sdk-1.7.1.jar:?]
        at com.webank.weid.service.impl.engine.fiscov2.WeIdServiceEngineV2.getWeIdList(WeIdServiceEngineV2.java:721) [weid-java-sdk-1.7.1.jar:?]
        at com.webank.weid.service.impl.WeIdServiceImpl.getWeIdList(WeIdServiceImpl.java:953) [weid-java-sdk-1.7.1.jar:?]
        at com.webank.weid.service.BuildToolService.getWeIdList(BuildToolService.java:337) [weid-build-tools-1.0.22.jar:?]
        at com.webank.weid.controller.BuildToolController.getWeIdList(BuildToolController.java:426) [weid-build-tools-1.0.22.jar:?]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_201]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_201]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_201]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_201]

--------------
