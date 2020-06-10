.. _windows_tools_vm:

----------------
Windows Tools VM
----------------

概览
+++++++++

此 Windows Server 2012 R2 的镜像已经预先安装了一系列必备的工具，包括:

- Microsoft Remote Server Administration Tools (RSAT)
- PuTTY, CyberDuck, WinSCP
- Sublime Text 3, Visual Studio Code
- OpenOffice
- Python
- pgAdmin
- Chocolatey Package Manager

如果在 **Lab Setup** 章节发现相关指导，请将此VM部署到您的集群中，并以您的姓名首字母简写作为唯一标识.

.. raw:: html

  <strong><font color="red">我们通常只需要部署此虚拟机一次，在本章节实验结束后无需删除，因为可能还会在其它实验章节中重复使用.</font></strong>

部署Windows Tools VM
++++++++++++++++++

在 **Prism Central** > 中选择 :fa:`bars` **> Virtual Infrastructure > VMs**, 然后点击 **Create VM**.

根据以下提示完成填写:

- **Name** - *Initials*-Windows-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 4 GiB

- Select **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - ToolsVM.qcow2
    - Select **Add**

.. -------------------------------------------------------------------------------------
.. 以下选项只针对当前环境，当5.11版本 GA后，我们会在此处选择默认的UEFI选项

.. - **Boot Configuration**
 ..  - Leave the default selected **Legacy Boot**

   .. .. note::
   ..  在以下链接中，您可以找到可支持UEFI的操作系统版本
   ..  http://my.nutanix.com/uefi_boot_support

.. -------------------------------------------------------------------------------------

- 选择 **Add New NIC**
    - **VLAN Name** - Secondary
    - 选择 **Add**

点击 **Save** 以创建 VM.

并开启VM的电源.

使用以下用户凭证，通过RDP戒Console登录VM:

- **Username** - NTNXLAB\\Administrator
- **password** - nutanix/4u
