---
title: Implementar la infraestructura de hiperconvergida con el centro de administración de Windows
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.date: 11/04/2019
ms.openlocfilehash: a7c15bd07754d48b7fbffe2cd95edaa871c9bde3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964451"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Implementar la infraestructura de hiperconvergida con el centro de administración de Windows

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Puede usar el centro de administración de Windows [versión 1910](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) o posterior para implementar la infraestructura de hiperconvergida con dos o más servidores de Windows adecuados. Esta nueva característica adopta la forma de un flujo de trabajo de varias fases que le guía a través de la instalación de características, la configuración de redes, la creación del clúster y la implementación de Espacios de almacenamiento directo y/o redes definidas por software (SDN) si se seleccionan.

A partir de la versión 2007 del centro de administración de Windows, el centro de administración de Windows es compatible con el sistema operativo HCI Azure Stack. Obtenga información sobre [cómo implementar un clúster en el centro de administración de Windows en el Azure Stack documentos de HCl](https://docs.microsoft.com/azure-stack/hci/getting-started). Aunque esta documentación se centra en Azure Stack HCl, las instrucciones también son adecuadas para las implementaciones de Windows Server.

## <a name="undo-and-start-over"></a>Deshacer y empezar de nuevo

Use estos cmdlets de Windows PowerShell para deshacer los cambios realizados por el flujo de trabajo y empezar de nuevo.

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>Quitar máquinas virtuales u otros recursos en clúster

Si ha creado máquinas virtuales u otros recursos en clúster, como los controladores de red para redes definidas por software, quítelos primero.

Por ejemplo, para quitar recursos por nombre, use:

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>Deshacer los pasos de almacenamiento

Si ha habilitado Espacios de almacenamiento directo, deshabilítelo con este script:

> [!Warning]
> Estos cmdlets eliminan permanentemente los datos de los volúmenes de Espacios de almacenamiento directo. Esto no se puede deshacer.

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>Deshacer los pasos de agrupación en clústeres

Si ha creado un clúster, quítelo con este cmdlet:

```PowerShell
Remove-Cluster -CleanUpAD
```

Para quitar también los informes de validación de clústeres, ejecute este cmdlet en todos los servidores que formaban parte del clúster:

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>Deshacer los pasos de red

Ejecute estos cmdlets en todos los servidores que formaban parte del clúster.

Si ha creado un conmutador virtual de Hyper-V:

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> El `Remove-VMSwitch` cmdlet quita automáticamente los adaptadores virtuales y deshace la formación de equipos incrustados por el conmutador de adaptadores físicos.

Si ha modificado las propiedades del adaptador de red, como el nombre, la dirección IPv4 y el ID. de VLAN:

> [!Warning]
> Estos cmdlets quitan los nombres de los adaptadores de red y las direcciones IP. Asegúrese de que tiene la información que necesita para conectarse después, como un adaptador para la administración que se excluye del script siguiente. Asegúrese también de que sabe cómo se conectan los servidores en términos de propiedades físicas, como la dirección MAC, no solo el nombre del adaptador en Windows.

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

Ahora está listo para iniciar el flujo de trabajo.

## <a name="additional-references"></a>Referencias adicionales

- [Hola, centro de administración de Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [Implementar espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
