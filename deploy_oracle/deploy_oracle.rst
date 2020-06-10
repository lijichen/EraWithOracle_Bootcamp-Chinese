.. _oracle部署:

-----------------
部署Oracle数据库
-----------------

    传统的数据库VM部署类似于下图，该过程通常从数据库的IT请求案例发起开始（可能来自Dev，Test，QA，Analytics等用途），接下来的一个或多个团队将需要部署所需的存储和计算资源。当基础架构资源就绪后，DBA需要配置和配置数据库软件，然后应用最佳实践进行优化并配置数据保护/备份策略。最后这个数据库可以移交给最终用户，这一过程往往需要大量交接工作，并且可能会造成很多摩擦。

.. figure:: images/0.png

但如果您采用Nutanix和Era解决方案，那么用来部署和配置数据库保护策略的时间，不会比阅读这篇实验手册所需时间长。

**在本实验中，您将通过克隆Oracle 19c的源虚拟机来完成部署，这个VM将充当主映像，来创建配置文件并通过Era来部署另一个VM。**

克隆源Oracle VM
++++++++++++++++++++++

该虚拟机正在运行Oracle 19c的版本，并安装了四月的PSU补丁。

#. 在 **Prism Central**, 选择 :fa:`bars` **> Virtual Infrastructure > VMs**.

   .. figure:: images/1.png

#. 选中 **Oracle19cSource** 复选框, 然后单击 **Actions > Clone**.

   .. figure:: images/1b.png

#. 填写以下字段:

   - **Number Of Clones** - 1
   - **Name** - *Initials*\ _oracle_base
   - **Description** - (Optional) Description for your VM.
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 8 GiB

#. 点击 **Save** 以创建虚拟机.

   接下来，您需要再创建一个这个虚拟机的副本，我们随后将用于安装Oracle PSU补丁程序。

#. 当虚拟机创建后，再次点击 **Actions > Clone** .

#. 将VM名字修改为 *Initials*\ **_oracle_patched** 并点击 **Save**.

#. 选中两个VM，点击 **Actions > Power On**.

探索Era的资源
+++++++++++++++++++++++

Era可以作为虚拟实例安装在AHV或ESXi环境中。为了节省实验环境的内存资源，我们已经在本次实验环境中预先部署了共享的Era的实例。

.. note::

   如果您有兴趣，可以在此链接中找到Era的简要安装说明 `链接 <https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Era-User-Guide-v12:era-era-installing-on-ahv-t.html>`_.

#. 在 **Prism Central > VMs > List** 中, 在 **IP Addresses** 列中查看分配给 **EraServer-\*** 的IP地址

#. 在浏览器中打开 \https://*ERA-VM-IP:8443*/ .

#. 用以下凭据登录:

   - **Username** - admin
   - **Password** - nutanix/4u

#. 从 **Dashboard** 的下拉菜单中, 选择 **Administration**.

#. 在 **Cluster Details** 中,请注意已经为您分配的集群配置了Era。

   .. figure:: images/6.png

#. 从左侧菜单中选择 **Era Resources** .

#. 查看已配置的网络，如果在 **VLANs Available for Network Profiles** 下没有网络显示, 请点击 **Add** ，并选择 **Secondary** VLAN并添加 **Add**.

   .. note::

      将 **Manage IP Address Pool** 保持未选中状态，因为我们将使用群集的IPAM管理地址

   .. figure:: images/era_networks_001.png

#. 从下拉菜单中选择 **SLAs**.

   .. figure:: images/7a.png

   Era 有五个内置的SLAs级别 (分别为Gold, Silver, Bronze, Zero, and Brass). SLAs是用来控制如何备份数据库的策略集合，通常包括持续数据保护，每天，每周，每月或每季度的保护间隔。

#. 从下拉菜单中，选择 **Profiles**.

   配置文件可用来预定义资源和配置, 从而使一致的资源部署和避免重复配置变的更加简单。例如，“计算配置文件”可指定数据库服务器的大小，包括诸如vCPU，每个vCPU的核心数和内存之类的详细信息
   
#. 如果在 **Network** 下看不到任何定义的网络, 点击 **+ Create**.

   .. figure:: images/8.png

#. 填写以下字段，然后点击 **Create**:

   - **Engine** - ORACLE
   - **Type** - Single Instance
   - **Name** - Primary_ORACLE_NETWORK
   - **Public Service VLAN** - Secondary

   .. figure:: images/9.png

通过Era注册Oracle 服务器
+++++++++++++++++++++++++++++++

在本练习中，将注册您之前创建的四月PSU版本的Oracle VM，并创建为Oracle 19c软件配置文件的Version 1.0版本。软件配置文件可以作为一个包含操作系统和数据库软件的模板，可以用来部署额外的数据库。

#. 在 **Era** 中, 从下拉菜单中选择 **Database Servers** 并从左侧菜单中选择 **List** 。

#. 单击 **+ Register** 并按提示填写以下 **Database Server** 字段:

   - **Engine** - Oracle
   - **IP Address or Name of VM** - *Initials*\ _oracle_base
   - **Database Version** - 19.0.0.0
   - **Era Drive User** - oracle
   - **Oracle Database Home** - /u02/app/oracle/product/19.0.0/dbhome_1
   - **Grid Infrastructure Home** - /u01/app/19.0.0/grid
   - **Provide Credentials Through** - Password
   - **Password** - Nutanix/4u

   .. note::

      Era驱动器用户可以是VM上的具备sudo权限的并设置为NoPASSWD的任意用户，Era会使用该用户的凭据执行各种操作，例如拍摄快照。

      Oracle Database Home是Oracle数据库软件的安装目录，并且是注册数据库服务器时所需的必备参数。

      Grid Infrastructure Home是Oracle Grid Infrastructure软件的安装目录。这个目录仅适用于Oracle RAC或 SIHA数据库。

   .. figure:: images/2.png

#. 点击 **Register**

#. 从下拉菜单中选择 **Operations** 以观察注册进度，此过程大约需要5分钟。等待注册操作成功完成后，再继续下一步操作。

   当 *Initials*\ **_oracle_base** 服务器在Era成功注册后，我们需要创建一个Software Profile，用来部署其它的Oracle VM.
   
#. 从下拉菜单中选择 **Profiles** ，并从左侧菜单中选择 **Software** .

#. 点击 **+ Create** 并填写以下字段:

   - **Engine** - Oracle
   - **Type** - Single Instance
   - **Name** - *Initials*\ _ORACLE_19C
   - **Description** - (Optional)
   - **Database Server** - Select your registered *Initials*\ _oracle_base VM

   .. figure:: images/3.png

#. 点击 **Create**.

#. 从下拉菜单中选择 **Operations** 以观察注册进度，此过程大约需要5分钟

注册数据库
++++++++++++++++++++++

#. 在 **Era** 中, 从下拉菜单中选择 **Databases** 并从左侧菜单中选择 **Sources** .

   .. figure:: images/11.png

#. 点击 **+ Register** 并填写以下字段:

   - **Engine** - ORACLE
   - **Database is on a Server that is:** - Registered
   - **Registered Database Servers** - Select your registered *Initials*\ _oracle_base VM

   .. figure:: images/12.png

#. 点击 **Next**

   - **Database Name in Era** - *Initials*\ -orcl
   - **SID** - orcl19c

   .. note::

     Oracle系统ID（SID）是系统中的特定数据库的唯一标识。因此，一个计算机系统上不能有多个具有相同SID的数据库。使用RAC时，属于同一数据库的所有实例都必须具有唯一的SID。
     
   .. figure:: images/13.png

#. 点击 **Next**

   - **Name** - *Initials*\ -orcl_TM
   - **SLA** - DEFAULT_OOB_BRASS_SLA (no continuous replay)

   .. figure:: images/14.png

#. 点击 **Register**

#. 从下拉菜单中选择 **Operations** 以观察注册进度，此过程大约需要5分钟。
