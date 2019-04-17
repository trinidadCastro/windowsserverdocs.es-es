---
title: Con espacios de almacenamiento directo en una máquina virtual
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
ms.sourcegitcommit: 52883945fd6e6bb5c24bd81944ca4bc0c5e6a216
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "8964617"
---
# Con espacios de almacenamiento directo en clústeres de máquina virtual de invitado

> Se aplica a: Windows Server 2019, Windows Server 2016

Puedes implementar espacios de almacenamiento directo en un clúster de servidores físicos o en clústeres de invitados de máquina virtual como se explica en este tema. Este tipo de implementación ofrece almacenamiento compartido virtual a través de un conjunto de máquinas virtuales encima de una nube pública o privada para que las soluciones de alta disponibilidad de la aplicación pueden usarse para aumentar la disponibilidad de las aplicaciones.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## Implementación en clústeres de invitados de Iaas VM de Azure

[Plantillas de Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) han sido publicado reducir la complejidad, configurar mejor recomendados y la velocidad de las implementaciones de espacios de almacenamiento directo en una VM de Iaas de Azure. Se trata de la solución recomendada para la implementación en Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## Requisitos

Las consideraciones siguientes se aplican al implementar espacios de almacenamiento directo en un entorno virtualizado.

> [!TIP]
> Plantillas de Azure configurará automáticamente las siguientes consideraciones para usted y son la solución recomendada al implementar en máquinas virtuales de IaaS de Azure.

-   Mínimo de 2 nodos y un máximo de 3 nodos

-   las implementaciones de 2 nodos deben configurar a un testigo (testigo en la nube o un testigo de recurso compartido de archivo)

-   las implementaciones de 3 nodos pueden tolerar 1 nodo hacia abajo y la pérdida de 1 o más discos en otro nodo.  Si 2 nodos están apagado, los discos virtuales se ser sin conexión hasta que uno de los nodos devuelve.  

-   Configurar las máquinas virtuales implementarse en todos los dominios de error

    -   Azure: configurar el conjunto de disponibilidad

    -   Hyper-V: configurar AntiAffinityClassNames en las máquinas virtuales para separar las máquinas virtuales en todos los nodos

    -   VMware: regla de configurar una máquina virtual otra anti afinidad mediante la creación de una Rule DRS de tipo ' máquinas virtuales independientes "para separar las máquinas virtuales entre hosts ESX. Los discos que se presentan para su uso con espacios de almacenamiento directo deben usar el adaptador de Paravirtual SCSI (PVSCSI). Para obtener soporte PVSCSI con Windows Server, consulte https://kb.vmware.com/s/article/1010398.

-   Sacar provecho de baja latencia / almacenamiento de información de alto rendimiento - almacenamiento Premium de Azure administrada se necesitan discos

-   Implementar un diseño de almacenamiento plana con ningún dispositivo de almacenamiento en caché configurado

-   Mínimo de 2 discos de datos virtual presentados a cada máquina virtual (VHD / VHDX / VMDK)

    Este número es diferente a las implementaciones de reconstrucción completa porque los discos virtuales pueden implementarse como archivos que no son susceptibles de sufrir errores físicos.

-   Deshabilitar las funcionalidades de reemplazo de la unidad automática en el servicio de mantenimiento ejecutando el siguiente cmdlet de PowerShell:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   No se admite: hospedar instantánea y restaurar el nivel de disco virtual

    En su lugar, usa las soluciones de copia de seguridad nivel de invitado tradicionales de copia de seguridad y restaurar los datos en los volúmenes espacios de almacenamiento directo.

-   Para proporcionar mayor resistencia a posible VHD / VHDX o aumentar la latencia de almacenamiento VMDK en clústeres de invitados, el valor de tiempo de espera de E/S de espacios de almacenamiento:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    El equivalente decimal de 7530 Hexadecimal es 30000, que es de 30 segundos. Ten en cuenta que el valor predeterminado es Decimal de 1770 hexadecimales o 6000, que es de 6 segundos.

## Ver también

[Plantillas de Iaas VM de Azure para implementar espacios de almacenamiento, vídeos y guías paso a paso](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Espacios de almacenamiento adicionales directo Overview] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
