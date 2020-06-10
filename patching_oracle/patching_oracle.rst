.. _使用Era为oracle更新补丁:

------------------------
使用Era为oracle更新补丁
------------------------

    在传统环境中，跨数据库服务器维护补丁版本的一致性是一个非常困难的挑战。Era 通过提供一种带版本控制的软件配置文件，可以非常方便的实现数据库引擎的补丁升级操作。一组数据库可以通过Era同时更新补丁或进行回滚，并支持通过网页，CLI或API的方式进行操作。
    每个季度，Oracle都会发布一组补丁，称为PSU。
   **在本实验中，您将逐步了解到如何使用Era部署Oracle 19c数据库和Grid软件并安装补丁**      
     

为基准VM安装补丁   
+++++++++++++++++++++++

在本练习中，您将为之前手动克隆的VM安装十月版本的补丁，然后注册这个数据到Era服务中，将其用作创建新的 *Initials*\ **_ORACLE_19C** 软件配置文件版本的基础.

#. 在 **Prism Central** 中, 记下您的 *Initials*\ **_oracle_patched** VM的地址.

#. 使用以下凭据通过SSH连接到您的 *Initials*\ **_oracle_patched** VM :

   - **User Name** - root
   - **Password** - Nutanix/4u

#. 执行以下脚本以下载并安装Oracle和Grid的October PSU补丁:

    .. code-block:: bash

      cd Downloads
      ./applypsu.sh

#. 请注意，该脚本将首先显示VM的当前补丁版本级别，请注意所显示的发行版本的日期为4月。按任意键继续安装补丁.

   .. figure:: images/7.png

#. 如果出现提示，请在提取补丁时键入 **A** to 以覆盖所有现有文件，然后按照提示按任意键继续。该脚本应运行约20分钟.

   .. figure:: images/8.png

#. 当脚本执行完成后，返回到 **Era > Database Servers > List** 操作菜单.

#. 单击 **+ Register** 并填写以下 **Database Server** 字段:

   - **Engine** - Oracle
   - **IP Address or Name of VM** - *Initials*\ _oracle_patched
   -  **Database Version** - 19.0.0.0
   - **Era Drive User** - oracle
   - **Oracle Database Home** - /u02/app/oracle/product/19.0.0/dbhome_1
   -  **Grid Infrastructure Home** - /u01/app/19.0.0/grid
   - **Provide Credentials Through** - Password
   - **Password** - Nutanix/4u

   .. figure:: images/9.png

#. 点击 **Register**

#. 在 **Operations** 页面监视执行进度，此过程大约需要5分钟.

#. 注册完成后，选择 **Era > Profiles > Software** 然后单击您的 *Initials*\ **_ORACLE_19C** 软件配置文件. 注意Era对操作系统中的安装包，包括 **Database Software** 和 **Grid Software** 都提供了完整的双重校验功能.请注意在 **Database Software** 下，可以看到 **Patches Found** 的信息.

   .. figure:: images/10.png

#. 单击进入 *Initials*\ **_ORACLE_19C** 软件配置文件，然后单击 **+ Create** 以根据您在上一步中注册的 *Initials*\ **_oracle_patched** 创建新版本.

   .. figure:: images/10b.png

#. 填写以下字段，然后单击 **Create**:

   - **Name** - *Initials*\ _ORACLE_19C (Oct19 PSU)
   - Select *Initials*\ **_oracle_patched**

   .. figure:: images/11.png

#. 在 **Operations** 页面监视操作进度，此过程大约需要5分钟.

#. 返回到 **Era > Profiles > Software** 界面，并点击您的 *Initials*\ **_ORACLE_19C** 软件配置文件. 请注意现在将显示的2.0版本，并且可在 **Database Software** and **Grid Infrastructure Software** 下找到另外的补丁文件.

   .. figure:: images/12.png

   在您想将新的补丁软件配置文件应用到您的 *Initials*\ **_oracle_prod** VM之前, 您需要将您的软件配置文件发布,否则Era将不会显示该版本可用或相关更新建议.

#. 选择 **2.0** 配置文件，然后单击 **Update**.

#. 在 **Status** 状态栏下，选择 **Published** 并点击 **Next**.

   .. figure:: images/13.png

#. 作为可选项，此步骤您还可以基于分别适用于操作系统，数据库或Grid软件的相关补丁文件更新相关的备注. 点击 **Next > Update**.

   .. figure:: images/14.png

#. 返回到 **Era > Database Servers > List** 列表，然后单击 *Initials*\ **_oracle_prod** 数据库服务器.

#. 在 **Profiles** 中, 可以看到新的，已发布的软件配置文件，已经列为数据库服务器的一个推荐可用更新. 此处继续点击 **Update**.

   .. figure:: images/15.png

#. 从下拉菜单中选择所需的补丁程序配置文件（在实际环境中，您可能会发布多个选项)然后单击 **Patch 1 Database** 以开始更新进程.

   .. note::

      Era还提供了计划修补程序的功能，使您可以选择预定的维护窗口完成自动升级。对于群集数据库部署，Era支持滚动更新，从而确保整个更新过程中的数据库可访问性不受影响.

      .. figure:: images/17.png

#. 在 **Operations** 页面监视执行进度，此过程大约需要25分钟.

   在应用补丁的过程中，Era 将正常关闭数据库和Grid服务，关闭虚拟机，使用2.0版本的软件配置文件创建的精简克隆来替换相应的虚拟磁盘，最后将数据库恢复在线状态.

   .. figure:: images/18.png

#. 在修补应用操作完成后，您可以轻松验证VM是否运行新的补丁版本的软件程序。使用以下凭据通过SSH进入您的SSH into your *Initials*\ **_oracle_prod** VM:

   - **User Name** - oracle
   - **Password** - Nutanix/4u

#. 执行以下命令以显示已安装的修补程序版本:

   ::

      $ORACLE_HOME/OPatch/opatch lsinventory | egrep 'appl|desc'

   .. figure:: images/19.png

小贴士
+++++++++

我们在本实验室中学到了哪些关键知识？

- 可以对软件配置文件进行版本控制，并用于将一致性的补丁更新部署到现有的数据库服务器中
- 软件配置文件的引入可以有效简化补丁修复过程，大幅降低手动更新补丁的工作量
- 计划更新任务可以用于在固定的变更窗口或满足SLA服务级别的时间窗口来自动进行补丁升级

