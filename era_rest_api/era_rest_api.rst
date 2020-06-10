.. _rest_api:

----------------------
Era REST API 资源管理器
----------------------

概览
++++++++

.. note::

  本实验预计完成时间: **15 MINUTES**

在本实验中，您将探索Era REST API资源管理器.

用Era REST API资源管理器
+++++++++++++++++++++++++++++++

Era具有“ API优先”架构，并提供了完整的REST API，可通过外部工具实现其功能的自动化和编排。与Prism相似，Era还提供了Rest API Explorer，可轻松发现和测试API功能。

#. 在菜单栏中，从右上方选择 **Admin > REST API Explorer** .

   .. figure:: images/29.png

#. 展开不同的类别以查看可用的API函数操作，包括注册Nutanix群集，注册和配置数据库，克隆和刷新数据库，更新配置文件和SLA ，以及获取操作和警报信息。

#. 作为一个简单的测试，请展开 **Databases > GET /databases**.

   此函数返回JSON，其中包含有关所有已注册和已配置数据库的详细信息，并且不需要配置其他参数。

#. 点击 **Try it out > Execute**.

   .. figure:: images/30.png

   您应该收到类似于下图的JSON格式的响应正文:

   .. figure:: images/32.png

   该API可通过类似像Nutanix Calm，ServiceNow,Ansible或其它自动化工具创建功能强大的自动化工作流。例如，您可以部署一个包含应用程序Web层的Calm蓝图，并使用Calm eScript工具调用Era来克隆现有数据库，并将新供应的数据库的IP返回给Calm。
  

小贴士
+++++++++

- Era提供了一个REST API，以允许与其他业务流程和自动化工具集成。 
