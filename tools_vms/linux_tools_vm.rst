.. _linux_tools_vm:

---------------
Linux Tools VM
---------------

概览
+++++++++

此CentOS镜像，会作为预备实验环境的一部分，会在多个实验中被用到.

如果在 **Lab Setup** 章节发现相关指导，请将此VM部署到您的集群中，并以您的姓名首字母简写作为唯一标识.

.. raw:: html

  <strong><font color="red"> 我们通常只需要部署此虚拟机一次，在本章节实验结束后无需删除，因为可能还会在其它实验章节中重复使用.</font></strong>

部署CentOS
++++++++++++++++

在 **Prism Central** > 选择 :fa:`bars` **> Virtual Infrastructure > VMs**,然后点击 **Create VM**.

根据以下提示完成填写:

- **Name** - *Initials*-Linux-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 2 GiB

- 选择 **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - CentOS7.qcow2
    - 选择 **Add**

.. -------------------------------------------------------------------------------------
.. 以下选项只针对当前环境，当5.11版本 GA后，我们会在此处选择默认的UEFI选项!

.. - **Boot Configuration**
 ..  - Leave the default selected **Legacy Boot**

   .. .. note::
   ..  在以下链接中，您可以找到可支持UEFI的操作系统版本
   ..  http://my.nutanix.com/uefi_boot_support

.. -------------------------------------------------------------------------------------


- 选择 **Add New NIC**
    - **VLAN Name** - Secondary
    - 选择 **Add**

点击 **Save** 以创建VM.

并开启VM的电源.

安装工具
++++++++++++++++

使用以下登录信息通过SSH或Console工具登录虚拟机:

- **Username** - root
- **password** - nutanix/4u

通过运行以下命令来安装所需的软件：

.. code-block:: bash

  yum update -y
  yum install -y ntp ntpdate unzip stress nodejs python-pip s3cmd awscli
  npm install -g request
  npm install -g express


配置NTP
...............

通过运行以下命令来启用和配置NTP:

.. code-block:: bash

  systemctl start ntpd
  systemctl enable ntpd
  ntpdate -u -s 0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org
  systemctl restart ntpd

禁用防火墙和SELinux服务
..............................

通过运行以下命令来禁用防火墙和SELinux服务:

.. code-block:: bash

  systemctl disable firewalld
  systemctl stop firewalld
  setenforce 0
  sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config

安装Python环境
.................

通过运行以下命令来安装 Python环境:

.. code-block:: bash

  yum -y install python36
  python3.6 -m ensurepip
  yum -y install python36-setuptools
  pip install -U pip
  pip install boto3
