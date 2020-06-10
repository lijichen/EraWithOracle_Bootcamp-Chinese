.. _管理Oracle数据库:

--------------------------
通过Era管理数据库
--------------------------

现在，我们将看到如何使用Era执行普通的数据库管理任务.

**在本实验中，您将学习如何管理您的ORACLE DB**

探索您的数据库
++++++++++++++++++++++

#. 在 **Era** 中, 从下拉菜单中选择 **Databases** ，并从左侧菜单中选择 **Sources** .

   .. figure:: images/1.png

#. 单击您的 *Initials*\ **-proddb** 实例名称, 系统将回到数据库this will take you back into the Database Summary页面. 此页面提供了有关数据库，数据库访问，Time Machine日程设置和用于配置的计算/网络/软件配置文件等详细信息。

    - **Database Summary:**

    .. figure:: images/2.png

    - **Database Server:**

    .. figure:: images/3.png

    - **Time Machine:**

    .. figure:: images/4.png

    - **Profiles:**

    .. figure:: images/5.png

为数据库创建快照保护
++++++++++++++++++++++

在对数据库进行手动快照之前，我们先在ProdDB中插入一个新表.

在数据库中插入新表
.............................

#. 通过SSH (Terminal/Putty)客户端登陆到 *Initials*\ -proddb虚拟机

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

     ssh oracle@PRODDB IP

#. 启动 **sqlplus**

     .. code-block:: Bash

       sqlplus / as sysdba

#. 执行以下操作创建新表:

     .. code-block:: Bash

       CREATE TABLE testlabtable
       (
       column1 VARCHAR(30),
       column2 DATE
       );

#. 通过执行以下操作列出表来验证新表是否存在:

     .. code-block:: Bash

       select owner as schema_name,
       table_name
       from sys.all_tables
       where table_name like 'TEST%';

手动创建数据库快照
................................

#. 在 **Era** 中, 从下拉菜单中选择 **Databases** 并从左侧菜单中选择 **Sources** .

#. 单击数据库的Time Machine *Initials*\ -proddb_TM

   .. figure:: images/6.png

#. 点击 **Actions > Log Catch Up**.

   .. figure:: images/12.png

#. 点击 **Yes**

#. 完成后, 点击 **Actions > Snapshot**.

   .. Figure:: images/7.png

   - **Snapshot Name** - *Initials*\ -proddb-1st-Snapshot

   .. Figure:: images/8.png

#. 点击 **Create**

#. 从下拉菜单中选 **Operations** 中监控执行进度，此步骤通常需要2-5分钟.

克隆数据库
+++++++++++++++++++++++++++++++++++++

#. 在 **Era** 中, 从下拉菜单中选择 **Time Machines** 然后选择 *Initials*\ -proddb_TM

#. 点击 **Actions > Clone Database**.

   - **Snapshot** - *Initials*\ -proddb-1st-Snapshot (Date Time)

   .. figure:: images/9.png

#. 点击 **Next**

   - **Database Server** - Create New Server
   - **Database Server Name** - *Initials*\ _oracle_prod_Clone1
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary-ORACLE-Network
   - **SSH Public Key Through** - Select **Text**

   ::

      ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNGZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7GyhCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNRuz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5Orw==

   .. figure:: images/10.png

#. 点击 **Next**

   - **Clone Name** - *Initials*\ _proddb_Clone1
   -  **SID** - *Initials*\ prod
   -  **SYS and SYSTEM Password** - Nutanix/4u
   -  **Database Parameter Profile** - ORACLE_SMALL_PARAMS

   .. figure:: images/11.png

#. 点击 **Clone**

#. 从下拉菜单中选择 **Operations** 以监控操作进度，此过程大约30-50分钟.

删除表并刷新克隆
++++++++++++++++++++++++++++++

有时，不可避免的会发生数据库中的一个表空间或其它数据被误删除的事故，我们当然会希望能够用一些手段恢复这些数据。下面我们会尝试删除一个表空间，并通过Era克隆刷新功能从最近一份快照中尝试还原。


删除表空间
............

#. 通过SSH (Terminal/Putty) 终端登陆 *Initials*\ -proddb_Clone1 虚拟机

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

     ssh oracle@PRODDB_Clone1 IP

#. 启动 **sqlplus**

     .. code-block:: Bash

       sqlplus / as sysdba

#. 执行以下操作删除表:

     .. code-block:: Bash

       DROP TABLE testlabtable;

#. 通过执行以下列出表来验证表已被删除:

     .. code-block:: Bash

       select owner as schema_name,
       table_name
       from sys.all_tables
       where table_name like 'TEST%';

克隆刷新
.............

#. 在 **Era** 中, 从下拉菜单中选择 **Databases** 并从左侧菜单中选择 **Clones** .

#. 选择数据库的克隆 *Initials*\ _proddb,然后单击 **Refresh**.

   - **Snapshot** - *Initials*\ _proddb-1st-Snapshot (Date Time)

#. 点击 **Refresh**

#. 从下拉菜单中选择 **Operations** 并监控执行进度，此过程大约需要2-5分钟.

验证表是否恢复
....................

#. 通过SSH (Terminal/Putty) 终端登陆 *Initials*\ -proddb_Clone1 虚拟机

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

     ssh oracle@PRODDB_Clone1 IP

#. 启动 **sqlplus**

     .. code-block:: Bash

       sqlplus / as sysdba

#. 通过执行以下操作列出表来验证表是否已恢复：

     .. code-block:: Bash

       select owner as schema_name,
       table_name
       from sys.all_tables
       where table_name like 'TEST%';

