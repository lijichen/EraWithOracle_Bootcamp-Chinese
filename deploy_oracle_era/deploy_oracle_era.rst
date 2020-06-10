.. _部署Oracle Era:

-------------------------
通过Era部署Oracle数据库
-------------------------

Oracle每个季度都会发布一组补丁，称为PSU. **在本实验中，您将逐步学习如何使用Era部署Oracle 19c数据库和Grid软件并进行补丁修复。** 

使用Era创建Oracle数据库
+++++++++++++++++++++++++++++

在本练习中，您将使用 *Initials*\ **_ORACLE_19C** 1.0软件配置文件来部署一套全新的Oracle数据库.

#. 从下拉菜单中选择 **Databases** ，并从左侧菜单中选择 **Sources** .

#. 点击 **+ Provision > Single Node Database**.

#. 在 **Provision a Database** 向导中, 填写以下字段以配置数据库服务器:

   - **Engine** - Oracle
   - **Database Server** - Create New Server
   - **Database Server Name** - *Initials*\ _oracle_prod
   - **Description** - (Optional)
   - **Software Profile** - *Initials*\ _ORACLE_19C
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary_ORACLE_NETWORK
   - Select **Enable High Availability**
   - **SYS ASM Password** - oracle
   - **SSH Public Key for Node Access** - Select **Text**

   ::

      ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNGZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7GyhCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNRuz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5Orw==


   .. note::

         当选择启用高可用选项时，Oracle Grid将被作为数据库部署的一部分执行，并且Oracle自动存储管理（ASM）也会被部署用于卷管理。如果没有启用高可用性，数据库存储将会用Linux LVM和文件系统的方式。如果部署集群的Oracle Rac，Grid和ASM为必装项.

   .. figure:: images/4.png

#. 点击 **Next**, 然后填写以下字段以配置数据库:

   -  **Database Name** - *Initials*\ _proddb
   -  **SID** - *Initials*\ prod
   -  **SYS and SYSTEM Password** - Nutanix/4u
   -  **Database Parameter Profile** - ORACLE_SMALL_PARAMS

   .. figure:: images/5.png

   .. note::

      对于Era可支持的每一个数据库引擎，您都可以选择在数据库创建之前或之后运行自定义脚本来实现定制化。常见的用例包括:

      - 数据屏蔽脚本
      - 向数据库监视解决方案注册数据库
      - 用于更新DNS/IPAM的脚本
      - 用于自动执行应用程序设置的脚本，例如Oracle PeopleSoft的应用程序级克隆

      在数据库层开启 **Encryption** 静态加密功能，一般会用于一些合规性要求，或者有效阻止潜在的攻击者绕过数据库直接从存储中读取敏感信息的风险。


#. 点击 **Next** 然后填写以下字段为数据库配置Time Machine:

   - **Name** - *Initials*\ _proddb_TM (Default)
   - **Description** - (Optional)
   - **SLA** - DEFAULT_OOB_GOLD_SLA
   - **Schedule** - (Defaults)

   .. figure:: images/6.png

#. 单击 **Provision** 开始创建新的数据库服务器VM和 *Initials*\ **_proddb** 数据库.

#. 从下拉菜单中选择 **Operations** 以监控操作进度。此过程通常需要60分钟（具体取决于您的群集配置）.

#. 配置数据库时，请继续以下练习.
