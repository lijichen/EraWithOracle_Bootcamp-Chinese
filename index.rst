.. title:: Databases: Era with Oracle Bootcamp

.. toctree::
   :maxdepth: 2
   :caption: Era with Oracle
   :name: _dbs
   :hidden:

   labsetup/labsetup
   deploy_oracle/deploy_oracle
   deploy_oracle_era/deploy_oracle_era
   patching_oracle/patching_oracle
   admin_oracle/admin_oracle

.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:

  era_rest_api/era_rest_api

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm
  appendix/glossary

.. _getting_started:

---------------
Getting Started
---------------

欢迎来到Era For Oracle数据库训练营！本实验手册将配合讲师课程，为您介绍Nutanix在数据库方面的技术以及许多常见的管理任务。


内容更新
++++++++++

- 本实验手册环境和所用功能已经同步更新到以下软件版本:
    - AOS 5.11.x / 5.15.x / 5.16.x
    - PC 5.16.x

- 可选实验更新:
    - 暂无更新 
    
日程安排
++++++

- 实验手册介绍
- 实验搭建
- Oracle数据库部署
- 通过Era部署Oracle数据库
- 通过Era为Oracle更新补丁
- 通过Era管理Oracle

可选实验:

- Era API资源管理器

个人介绍
+++++++++++++

- 姓名
- 与Nutanix的关系或经历

 初始设定
+++++++++++++

- 记录本次实验会用到的所有 *密码* .
- 登录到您的虚拟桌面(连接方法如下)

环境信息
+++++++++++++++++++

Nutanix动手训练营会利用到远程Nutanix Hosted PoC的设备。为了能够使您更方便的完成联系，您所分配的集群会预先部署所有可用的镜像，网络以及虚拟机资源。

网络
..........

Hosted POC 集群遵循标准的命名约定:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**21**.\ *XYZ*\ .0
- **Cluster IP** - 10.**21**.\ *XYZ*\ .37

如果从市场营销池中配置:

- **Cluster Name** - MKT\ *XYZ*
- **Subnet** - 10.**20**.\ *XYZ*\ .0
- **Cluster IP** - 10.**20**.\ *XYZ*\ .37

例如:

- **Cluster Name** - POC055
- **Subnet** - 10.21.55.0
- **Cluster IP** - 10.21.55.37

在整个训练营中，您需要在多个环节中，使用分配给您的8位网段信息替换实验手册中的 *XYZ* ,例如:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

每个群集配置有2个可用于VM的VLAN:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

密码
...........

.. note::

  每套集群的 *<Cluster Password>* 是唯一的，会在课程中由讲师提供.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

每个群集都有一个专用的域控制器VM， **DC**，负责为 **NTNXLAB.local** 域提供AD服务，域中填充了以下用户和组：

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

 访问说明
+++++++++++++++++++

学员可以通过多种不同方式访问Nutanix托管POC环境:

实验环境访问用户凭证
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*

Frame 桌面
.........

登录: https://frame.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Pulse Secure VPN（Nutanix雇员专用）
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

安装安装端

在Pulse Secure 客户端, 点击 **Add** 一个连接:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanix 版本信息
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.11.2.3
- **PC Version** - 5.11.2.1
