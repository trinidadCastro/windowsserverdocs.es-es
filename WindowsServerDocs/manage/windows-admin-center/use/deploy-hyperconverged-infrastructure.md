---
title: Implementar la infraestructura de hiperconvergida con el centro de administración de Windows
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.prod: windows-server
ms.technology: manage
ms.date: 11/04/2019
ms.openlocfilehash: 088fb7b8f03ab7e575b562572f2e29e1b5774760
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474492"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Implementar la infraestructura de hiperconvergida con el centro de administración de Windows

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Puede usar el centro de administración de Windows [versión 1910](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) o posterior para implementar la infraestructura de hiperconvergida con dos o más servidores de Windows adecuados. Esta nueva característica adopta la forma de un flujo de trabajo de varias fases que le guía a través de la instalación de características, la configuración de redes, la creación del clúster y la implementación de Espacios de almacenamiento directo y/o redes definidas por software (SDN) si se seleccionan.

![Captura de pantalla del flujo de trabajo de creación de clúster](../media/deploy-hyperconverged-infrastructure/deployment-workflow-screenshot.png)

  > [!Important]
  > Esta característica se encuentra en desarrollo activo. Está disponible en versión preliminar para que pueda probarla pronto y compartir sus comentarios.

## <a name="preview-the-workflow"></a>Obtener una vista previa del flujo de trabajo

### <a name="1-prerequisites"></a>1. Prerrequisitos

El flujo de trabajo de creación de clústeres en el centro de administración de Windows no realiza la instalación del sistema operativo sin sistema operativo, por lo que primero debe instalar Windows Server en cada servidor. Las versiones admitidas son Windows Server 2016, Windows Server 2019 y Windows Server Insider Preview. También debe unir cada servidor al mismo Active Directory dominio en el que se ejecuta el centro de administración de Windows antes de iniciar el flujo de trabajo.

### <a name="2-install-windows-admin-center"></a>2. instalar el centro de administración de Windows

Siga las instrucciones para [Descargar e instalar](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) la versión más reciente del centro de administración de Windows.

### <a name="3-install-the-cluster-creation-extension"></a>3. instalar la extensión de creación de clústeres

El flujo de trabajo de creación de clústeres se entrega y actualiza a través de la fuente de extensión del centro de administración de Windows. Esto es para que podamos abordar sus comentarios y realizar mejoras más rápidamente durante la fase de versión preliminar.

Para instalar la extensión, inicie el centro de administración de Windows y, a continuación:

1.  En la esquina superior derecha, seleccione el icono de configuración.
2.  Vaya a extensiones
3.  Búsqueda de la extensión de creación de clústeres
4.  Seleccionar instalar

![Capturas de pantallas de los cuatro pasos para instalar la extensión de creación de clústeres](../media/deploy-hyperconverged-infrastructure/how-to-install.png)

Una vez instalado, seleccione la extensión de creación de clústeres en la lista desplegable de la parte superior izquierda:

![Captura de pantalla del inicio de la extensión de creación de clústeres](../media/deploy-hyperconverged-infrastructure/how-to-launch.png)

## <a name="limitations-of-the-preview-release"></a>Limitaciones de la versión preliminar

El flujo de trabajo de creación del clúster está en el desarrollo activo; en otras palabras, no ha terminado.

Estas son algunas limitaciones de la versión preliminar actual:

1.  **Requisitos de dominio.** Actualmente, el flujo de trabajo requiere que todos los servidores que agregue ya estén Unidos al mismo Active Directory dominio en el que se ejecuta el centro de administración de Windows.
2.  **Mostrar script.** Con la versión preliminar actual, no puede ver los scripts que el flujo de trabajo ejecuta con la funcionalidad Mostrar script en el centro de administración de Windows, ni puede descargar un informe completo de todo lo que ha sucedido al final. Sabemos que esto es importante para muchos usuarios: Manténgase atento. Mientras tanto, use los cmdlets que se proporcionan a continuación para [ver lo que está ocurriendo](#see-whats-happening).
3.  **Reanudar o volver a empezar.** Aunque puede salir de MSRC durante a través del flujo de trabajo, la versión de vista previa actual no controla la reanudación desde donde se quedó, ni puede limpiarse completamente antes de empezar. Si desea implementar de forma repetida en los mismos servidores para las pruebas, use los cmdlets que se proporcionan a continuación para [Deshacer y volver a empezar](#undo-and-start-over).
4.  **Acceso directo a memoria remota (RDMA).** La versión preliminar actual no puede habilitar redes RDMA, que se suele usar con la infraestructura hiperconvergida. Por ahora, complete el flujo de trabajo sin habilitar RDMA y, a continuación, use otras herramientas para habilitar RDMA tal como lo haría de otra manera.

Además de abordar estas limitaciones, existen innumerables características que podrían agregarse en futuras versiones: aplicar actualizaciones de Windows, configurar un testigo para el cuórum del clúster, importar máquinas virtuales, etc. Para ayudarnos a dar prioridad a las características que desea, [comparta sus comentarios](#feedback) como se describe a continuación.

## <a name="see-whats-happening"></a>Ver lo que está ocurriendo

Use estos cmdlets de Windows PowerShell para continuar y ver lo que está haciendo el flujo de trabajo.

Para ver qué características de Windows están instaladas, use el `Get-WindowsFeature` cmdlet. Por ejemplo:

```PowerShell
Get-WindowsFeature "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "BitLocker"
```

![Captura de pantalla de la salida de PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-1.png)

  > [!Note]
  > Las características que se instalan dependerán del tipo de clúster que seleccione.

Para ver los adaptadores de red y sus propiedades como la de nombre, direcciones IPv4 e identificador de VLAN:

```PowerShell
Get-NetAdapter | Where Status -Eq "Up" | Sort InterfaceAlias | Format-Table Name, InterfaceDescription, Status, LinkSpeed, VLANID, MacAddress
Get-NetAdapter | Where Status -Eq "Up" | Get-NetIPAddress -AddressFamily IPv4 -ErrorAction SilentlyContinue | Sort InterfaceAlias | Format-Table InterfaceAlias, IPAddress, PrefixLength
```

![Captura de pantalla de la salida de PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-2.png)

Para ver los conmutadores virtuales de Hyper-V y cómo se agrupan los adaptadores de red físicos:

```PowerShell
Get-VMSwitch
Get-VMSwitchTeam
```

![Captura de pantalla de la salida de PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-3.png)

Para ver los adaptadores de red virtual del host, si los hay, ejecute:

```PowerShell
Get-VMNetworkAdapter -ManagementOS | Format-Table Name, IsManagementOS, SwitchName
Get-VMNetworkAdapterTeamMapping -ManagementOS | Format-Table NetAdapterName, ParentAdapter
```

![Captura de pantalla de la salida de PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-4.png)

Para ver el clúster de conmutación por error actual y sus nodos miembro, ejecute:

```PowerShell
Get-Cluster
Get-ClusterNode
```

![Captura de pantalla de la salida de PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-5.png)

Para ver si Espacios de almacenamiento directo está habilitado y el grupo de almacenamiento predeterminado que se crea automáticamente:

```PowerShell
Get-ClusterStorageSpacesDirect
Get-StoragePool -IsPrimordial $False
```

![Captura de pantalla de la salida de PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-6.png)

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

## <a name="feedback"></a>Comentarios

Esta versión preliminar está relacionada con sus comentarios. Estas son algunas de las formas en que puede ponerse en contacto con nuestro equipo:

- [Envíe y vote solicitudes de características en UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Únase al foro del centro de administración de Windows en Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Correo electrónico de HCI: implementación [at] microsoft.com
- Tweet hasta[@servermgmt](https://twitter.com/servermgmt)

## <a name="report-an-issue"></a>Notificar un problema

Use los canales enumerados anteriormente para informar de un problema con el flujo de trabajo de creación de clústeres.

Si es posible, incluya la siguiente información para ayudarnos a reproducir y resolver rápidamente el problema:

- Tipo de clúster seleccionado (por ejemplo: *"hiperconvergido"*)
- Paso en el que se encontró el problema (por ejemplo: *"3,2 Create cluster"*)
- Versión de la extensión de creación de clústeres. Vaya a **configuración**  >  **extensiones**  >  **instaladas extensiones** y vea la columna **versión** (por ejemplo: *"1.0.30"*).
- Mensajes de error, ya sea en la pantalla o en la consola del explorador, que puede abrir presionando **F12**.
- Cualquier otra información relevante sobre su entorno

## <a name="additional-references"></a>Referencias adicionales

- [Hola, centro de administración de Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [Implementar espacios de almacenamiento directo](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
