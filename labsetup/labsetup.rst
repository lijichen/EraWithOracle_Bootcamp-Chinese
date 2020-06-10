.. _labsetup:

----------------------
Oracle Lab 设置
----------------------

欢迎来到数据库训练营，该训练营旨在为您提供初步的经验，以了解Nutanix为什么是数据库工作负载的最佳理想平台。

从历史上看，对Oracle进行虚拟化一直是一个挑战，因为在传统虚拟化堆栈上运行Oracle数据库的成本很高，而且基于SAN存储的三层体系结构也可能会对性能产生影响。企业及其IT部门一直在努力平衡成本，操作简便性和一致的可预测性能之间的关系。

Nutanix企业云消除了许多这些挑战，并使对关键业务应用程序（例如Oracle）实现虚拟化更加容易。Acropolis分布式存储结构（DSF）是一种软件定义的解决方案，可提供企业级SAN存储能期望实现的所有功能，却无需担忧SAN的物理限制和性能瓶颈。DSF功能对于Oracle数据库场景有主要如下收益:

- 数据本地化功能以及对索引和关键数据库文件使用闪存加速功能，可以有效降低操作延迟。
- 一种高度分布式的存储算法，可以高效处理随机和顺序型工作负载，无需性能妥协
- 能够添加新节点并扩展基础架构，而不会造成系统停机或性能影响
- Nutanix数据保护和灾难恢复工作流程，简化了备份操作和业务连续性流程

除了解决用于托管关键应用程序的常见基础架构的问题之外，Nutanix还寻求解决与管理数据库相关的许多关键难题。

.. figure:: images/4.png

根据IDC在2018年对全美大于1000员工的500家中大型企业进行调研后的数据估计:

- 77%的组织在生产环境中拥有超过200多个数据库实例
- 超过82%的数据库有超过10个以上的副本
- 占总存储容量45%-60%的存储空间被用来存储副本数据
- 32%的数据库克隆每天需要刷新以进行开发/测试分析
- 预计2020年全年，复制数据副本会导致IT组织花费556.3亿美元

维持现状会导致存储使用效率低下，并大幅浪费数据库管理员的宝贵时间。 

.. figure:: images/5.png

Nutanix Era为您的企业云提供DBaaS。利用Nutanix企业云操作系统，我们能够利用全栈的强大功能-融合数据，计算和软件。Nutanix Era隐藏了数据库操作的复杂性，并为多个数据库引擎提供了常见的API，CLI和用户级GUI体验。它使诸如克隆之类的数据库操作变得高效，从而为我们的客户降低了数据库管理的总体拥有成本



.. 配置一个Project
  +++++++++++++++++++++

  在本实验中，您将利用多个预先构建的Calm Blueprints来调配您的应用程序

  #. 在 **Prism Central** 中, 选择 :fa:`bars` **> Services > Calm**.\

  #. 从左侧菜单中选择 **Projects** 并点击 **+ Create Project**.

     .. figure:: images/2.png

  #. 填写以下字段:

     - **Project Name** - *Initials*\ -Project
     - 在 **Users, Groups, and Roles** 栏中, 选择 **+ User**
        - **Name** - Administrators
        - **Role** - Project Admin
        - **Action** - Save
     - 在 **Infrastructure** 栏中, 选择 **Select Provider > Nutanix**
     - 在 **Select Clusters & Subnets** 栏中
     - 选择 *Your Assigned Cluster*
     - 在 **Subnets** 栏中, 选择 **Primary**, **Secondary**, 并点击 **Confirm**
     - 通过点击  :fa:`star` 标记 **Primary** 为默认网络

     .. figure:: images/3.png

  #. 点击 **Save & Configure Environment**.

部署 Windows Tools VM
++++++++++++++++++++++++++++

在本章节中的某些练习将需要利用Windows Tools VM。请按照以下步骤从磁盘映像配置您的个人VM

#. 在 **Prism Central** 中, 选择 :fa:`bars` **> Virtual Infrastructure > VMs**.

#. 点击 **+ Create VM**.

#. 填写以下字段以完成用户VM的创建请求:

   - **Name** - *Initials*\ -WinToolsVM
   - **Description** - Manually deployed Tools VM
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 4 GiB

   - 选择 **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - WinToolsVM.qcow2
      - 选择 **Add**

   - 选择 **Add New NIC**
      - **VLAN Name** - Secondary
      - 选择 **Add**

#. 点击 **Save** 以创建新的VM.

#. 打开 *Initials*\ **-WinToolsVM** 虚拟机的电源。
