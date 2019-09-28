---
title: Usar Espacios de almacenamiento directo en una máquina virtual
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Cómo implementar Espacios de almacenamiento directo en un clúster invitado de máquina virtual, por ejemplo, en Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: ab0ce792c5a948e763a48493a78ccdac7a6fe74c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366049"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Uso de Espacios de almacenamiento directo en clústeres de máquinas virtuales invitadas

> Se aplica a: Windows Server 2019 y Windows Server 2016

Puede implementar Espacios de almacenamiento directo en un clúster de servidores físicos o en clústeres invitados de máquinas virtuales, como se describe en este tema. Este tipo de implementación ofrece almacenamiento virtual compartido en un conjunto de máquinas virtuales sobre una nube privada o pública para que las soluciones de alta disponibilidad de la aplicación se puedan usar para aumentar la disponibilidad de las aplicaciones.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Implementación de clústeres invitados de máquinas virtuales de IaaS de Azure

[Las plantillas de Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) se han publicado. reduzca la complejidad, configure los procedimientos recomendados y acelere las implementaciones de espacios de almacenamiento directo en una máquina virtual de IaaS de Azure. Esta es la solución recomendada para implementar en Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Requisitos

Las consideraciones siguientes se aplican al implementar Espacios de almacenamiento directo en un entorno virtualizado.

> [!TIP]
> Las plantillas de Azure configurarán automáticamente las siguientes consideraciones para usted y son la solución recomendada al implementar en máquinas virtuales de IaaS de Azure.

-   Mínimo de 2 nodos y un máximo de 3 nodos

-   las implementaciones de 2 nodos deben configurar un testigo (testigo de nube o testigo de recurso compartido de archivos)

-   las implementaciones de tres nodos pueden tolerar un nodo inactivo y la pérdida de 1 o más discos en otro nodo.  Si se cierran dos nodos, los discos virtuales estarán sin conexión hasta que se devuelva uno de los nodos.  

-   Configurar las máquinas virtuales que se van a implementar en los dominios de error

    -   Azure: configuración del conjunto de disponibilidad

    -   Hyper-V: configuración de AntiAffinityClassNames en las máquinas virtuales para separar las máquinas virtuales entre nodos

    -   VMware: configure la regla de antiafinidad de VM-VM mediante la creación de una regla de DRS de tipo "independiente Virtual Machines" para separar las máquinas virtuales en los hosts de ESX. Los discos que se presentan para su uso con Espacios de almacenamiento directo deben usar el adaptador de paravirtual SCSI (PVSCSI). Para obtener compatibilidad con PVSCSI con Windows Server, consulte https://kb.vmware.com/s/article/1010398.

-   Aproveche el almacenamiento de baja latencia y alto rendimiento: se requieren Azure Premium Storage Managed Disks

-   Implementar un diseño de almacenamiento plano sin dispositivos de almacenamiento en caché configurados

-   Mínimo de 2 discos de datos virtuales presentados a cada máquina virtual (VHD/VHDX/VMDK)

    Este número es diferente de las implementaciones sin sistema operativo, ya que los discos virtuales se pueden implementar como archivos que no son susceptibles de errores físicos.

-   Deshabilite las funciones de sustitución automática de unidad en el Servicio de mantenimiento ejecutando el siguiente cmdlet de PowerShell:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   No compatible: Instantánea o restauración de disco virtual de nivel de host

    En su lugar, use soluciones tradicionales de copia de seguridad de nivel de invitado para realizar copias de seguridad y restaurar los datos en los volúmenes de Espacios de almacenamiento directo.

-   Aumente el valor de tiempo de espera de e/s de espacios de almacenamiento para proporcionar mayor resistencia a la latencia de almacenamiento VHD/VHDX/VMDK posible en los clústeres invitados.

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    El equivalente decimal del hexadecimal 7530 es 30000, que es de 30 segundos. Tenga en cuenta que el valor predeterminado es 1770 hexadecimal o 6000 decimal, que es de 6 segundos.

## <a name="see-also"></a>Vea también

[Plantillas adicionales de máquinas virtuales de IaaS de Azure para implementar espacios de almacenamiento directo, vídeos y guías paso a paso](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

[Información general adicional Espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
