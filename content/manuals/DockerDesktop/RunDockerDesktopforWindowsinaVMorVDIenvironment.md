+++
title = "在 VM 或 VDI 环境中运行 Docker Desktop for Windows"
date = 2024-10-23T14:54:40+08:00
weight = 130
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/vm-vdi/](https://docs.docker.com/desktop/vm-vdi/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Run Docker Desktop for Windows in a VM or VDI environment - 在 VM 或 VDI 环境中运行 Docker Desktop for Windows

In general, we recommend running Docker Desktop natively on either Mac, Linux, or Windows. However, Docker Desktop for Windows can run inside a virtual desktop provided the virtual desktop is properly configured.

​	一般来说，我们推荐在 Mac、Linux 或 Windows 上本地运行 Docker Desktop。然而，只要虚拟桌面配置正确，Docker Desktop for Windows 也可以在虚拟桌面中运行。

To run Docker Desktop in a virtual desktop environment, it is essential nested virtualization is enabled on the virtual machine that provides the virtual desktop. This is because, under the hood, Docker Desktop is using a Linux VM in which it runs Docker Engine and the containers.

​	要在虚拟桌面环境中运行 Docker Desktop，必须在提供虚拟桌面的虚拟机上启用嵌套虚拟化。这是因为 Docker Desktop 在底层使用了一个 Linux 虚拟机来运行 Docker 引擎和容器。

## 虚拟桌面支持 Virtual desktop support

> **Note**
>
> 
>
> Support for running Docker Desktop on a virtual desktop is available to Docker Business customers, on VMware ESXi or Azure VMs only.
>
> ​	Docker 仅为 Docker Business 客户提供在 VMware ESXi 或 Azure 虚拟机上运行 Docker Desktop 的支持。

The support available from Docker extends to installing and running Docker Desktop inside the VM, once the nested virtualization is set up correctly. The only hypervisors we have successfully tested are VMware ESXi and Azure, and there is no support for other VMs. For more information on Docker Desktop support, see [Get support]({{< ref "/manuals/DockerDesktop/Getsupport" >}}).

​	在正确设置嵌套虚拟化后，Docker 的支持范围涵盖在虚拟机内安装和运行 Docker Desktop。我们仅对 VMware ESXi 和 Azure 成功进行了测试，不支持其他虚拟机。更多关于 Docker Desktop 支持的信息，请参见 [获取支持]({{< ref "/manuals/DockerDesktop/Getsupport" >}})。

For troubleshooting problems and intermittent failures that are outside of Docker's control, you should contact your hypervisor vendor. Each hypervisor vendor offers different levels of support. For example, Microsoft supports running nested Hyper-V both on-prem and on Azure, with some version constraints. This may not be the case for VMWare ESXi.

​	对于 Docker 无法控制的问题和偶发故障，请联系您的虚拟化平台供应商。每个供应商的支持级别不同。例如，Microsoft 支持在本地和 Azure 上运行嵌套的 Hyper-V，但有一些版本限制。VMware ESXi 可能不适用。

Docker does not support running multiples instances of Docker Desktop on the same machine in a VM or VDI environment.

​	Docker 不支持在同一台机器上运行多个 Docker Desktop 实例于 VM 或 VDI 环境中。

## 启用嵌套虚拟化 Turn on nested virtualization

You must turn on nested virtualization before you install Docker Desktop on a virtual machine.

​	在虚拟机上安装 Docker Desktop 前，必须先启用嵌套虚拟化。

### 在 VMware ESXi 上启用嵌套虚拟化 Turn on nested virtualization on VMware ESXi

Nested virtualization of other hypervisors like Hyper-V inside a vSphere VM [is not a supported scenario](https://kb.vmware.com/s/article/2009916). However, running Hyper-V VM in a VMware ESXi VM is technically possible and, depending on the version, ESXi includes hardware-assisted virtualization as a supported feature. For internal testing, we used a VM that had 1 CPU with 4 cores and 12GB of memory.

​	在 vSphere 虚拟机中嵌套虚拟化其他虚拟化程序（如 Hyper-V）[并不受支持](https://kb.vmware.com/s/article/2009916)。不过，从技术上讲，在 VMware ESXi 虚拟机中运行 Hyper-V 虚拟机是可能的，且根据版本，ESXi 包含了硬件辅助虚拟化作为受支持的功能。内部测试中，我们使用了一个具有 1 个 CPU、4 核心和 12GB 内存的虚拟机。

For steps on how to expose hardware-assisted virtualization to the guest OS, [see VMware's documentation](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html).

​	有关如何向客户操作系统公开硬件辅助虚拟化的步骤，请参阅 [VMware 文档](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html)。

### 在 Azure 虚拟机上启用嵌套虚拟化 Turn on nested virtualization on an Azure Virtual Machine

Nested virtualization is supported by Microsoft for running Hyper-V inside an Azure VM.

​	Microsoft 支持在 Azure 虚拟机中运行嵌套虚拟化的 Hyper-V。

For Azure virtual machines, [check that the VM size chosen supports nested virtualization](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes). Microsoft provides [a helpful list on Azure VM sizes](https://docs.microsoft.com/en-us/azure/virtual-machines/acu) and highlights the sizes that currently support nested virtualization. For internal testing, we used D4s_v5 machines. We recommend this specification or above for optimal performance of Docker Desktop.

​	对于 Azure 虚拟机，请检查所选的虚拟机大小是否支持嵌套虚拟化。[Microsoft 提供了一个 Azure VM 尺寸列表](https://docs.microsoft.com/en-us/azure/virtual-machines/acu)，并突出显示了当前支持嵌套虚拟化的大小。在内部测试中，我们使用了 D4s_v5 机器。我们建议选择此配置或更高，以确保 Docker Desktop 的最佳性能。

