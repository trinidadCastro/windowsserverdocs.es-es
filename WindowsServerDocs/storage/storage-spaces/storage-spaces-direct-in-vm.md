---
title: Uso de espacios de almacenamiento directo en una máquina virtual
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Cómo implementar espacios de almacenamiento directo en un clúster de invitado de máquina virtual, por ejemplo, en Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841026"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Uso de espacios de almacenamiento directo en clústeres invitados de máquina virtual

> Se aplica a: Windows Server 2019, Windows Server 2016

Puede implementar espacios de almacenamiento directo en un clúster de servidores físicos o en clústeres de invitados de máquina virtual como se describe en este tema. Este tipo de implementación ofrece almacenamiento compartido virtual en un conjunto de máquinas virtuales en una nube privada o pública para que se pueden usar soluciones de alta disponibilidad de la aplicación para aumentar la disponibilidad de aplicaciones.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Implementación de clústeres de invitados de máquina virtual de Iaas de Azure

[Las plantillas de Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) ha sido publicado disminuir la complejidad, configure mejores prácticas y la velocidad de las implementaciones de espacios de almacenamiento directo en una máquina virtual de Iaas de Azure. Se trata de la solución recomendada para implementar en Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Requisitos

Se aplican las consideraciones siguientes al implementar espacios de almacenamiento directo en un entorno virtualizado.

> [!TIP]
> Las plantillas de Azure configurará automáticamente la debajo de las consideraciones para usted y son la solución recomendada cuando se implementa en máquinas virtuales de IaaS de Azure.

-   Mínimo de 2 nodos y un máximo de 3 nodos

-   implementaciones de nodo 2 deben configurar a un testigo (testigo en la nube o el testigo del recurso compartido de archivos)

-   las implementaciones de 3 nodos pueden tolerar 1 nodo hacia abajo y la pérdida de 1 o más discos en otro nodo.  Si los 2 nodos están cerrados, a continuación, los discos virtuales se estar sin conexión hasta que uno de los nodos devuelve.  

-   Configurar las máquinas virtuales se implementaran en dominios de error

    -   Azure: configurar un conjunto de disponibilidad

    -   Hyper-V – AntiAffinityClassNames configurar en las máquinas virtuales para separar las máquinas virtuales entre nodos

    -   VMware: regla configurar Antiafinidad de VM de la máquina virtual mediante la creación de un tipo de DRS Rule ' máquinas virtuales independientes "para separar las máquinas virtuales en hosts ESX. Discos presentados para su uso con espacios de almacenamiento directo debe usar el adaptador Paravirtual SCSI (PVSCSI). Para obtener soporte PVSCSI con Windows Server, consulte https://kb.vmware.com/s/article/1010398.

-   Aprovechar latencia baja y administra el almacenamiento de alto rendimiento: Azure Premium Storage se necesitan discos

-   Implementar un diseño de almacenamiento sin formato con ningún dispositivo de almacenamiento en caché configurado

-   Mínimo de 2 discos de datos virtuales presentados para cada máquina virtual (VHD / VHDX y VMDK)

    Este número es diferente de las implementaciones sin sistema operativo porque los discos virtuales se pueden implementar como archivos que no son susceptibles a errores físicos.

-   Deshabilitar las funcionalidades de sustitución automática de unidad en el servicio de mantenimiento, ejecute el cmdlet de PowerShell siguiente:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   No compatible: Nivel de disco virtual de host/restauración de instantáneas

    En su lugar, use las soluciones de copia de seguridad nivel de invitado tradicionales de copia de seguridad y restaurar los datos en los volúmenes de espacios de almacenamiento directo.

-   Para dar mayor resistencia a posibles VHD / VHDX / latencia de almacenamiento VMDK en clústeres de invitados, aumente el valor de tiempo de espera de E/S de espacios de almacenamiento:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    El valor decimal equivalente Hexadecimal 7530 es 30000, lo que es de 30 segundos. Tenga en cuenta que el valor predeterminado es Decimal, Hexadecimal o 6000 del 1770, que es de 6 segundos.

## <a name="see-also"></a>Vea también

[Plantillas de máquina virtual de Iaas de Azure adicionales para implementar espacios de almacenamiento directo, vídeos y guías paso a paso](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Espacios de almacenamiento adicionales directo Overview] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
