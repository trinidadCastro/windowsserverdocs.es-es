---
title: Usar la migración en vivo sin clústeres de conmutación por error para mover una máquina virtual
description: Proporciona los requisitos previos e instrucciones para realizar una migración en vivo en un entorno independiente.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: 9be61fbc860e9d8c5cbc020d6dd4082722e32509
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812095"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Usar la migración en vivo sin clústeres de conmutación por error para mover una máquina virtual

>Se aplica a: Windows Server 2016

Este artículo muestra cómo mover una máquina virtual mediante una migración en vivo sin usar clústeres de conmutación por error. Una migración en vivo mueve máquinas virtuales en ejecución entre los hosts de Hyper-V sin tiempo de inactividad apreciable.   
  
Para poder hacer esto, necesitará:   

- Una cuenta de usuario que sea miembro del grupo local Administradores de Hyper-V o del grupo Administradores en equipos de origen y destino. 
  
- El rol de Hyper-V en Windows Server 2016 o Windows Server 2012 R2 instalado en los servidores de origen y destino y configura para migraciones en vivo. Puede hacer una migración en vivo entre hosts que ejecutan Windows Server 2016 y Windows Server 2012 R2, si la máquina virtual sea al menos la versión 5.

    Para obtener instrucciones de actualización de versión, consulte [versión actualización máquina virtual de Hyper-V en Windows 10 o Windows Server 2016](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obtener instrucciones de instalación, consulte [configurar hosts para migración en vivo](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

- Las herramientas de administración de Hyper-V instaladas en un equipo que ejecuta Windows Server 2016 o Windows 10, a menos que las herramientas se instalan en el servidor de origen o destino y se ejecutará desde allí.  
   
## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>Use el Administrador de Hyper-V para mover una máquina virtual en ejecución  
  
1.  Abre el Administrador Hyper-V. (Desde el administrador del servidor, haga clic en **herramientas** >>**Administrador de Hyper-V**.)  
  
2.  En el panel de navegación, seleccione uno de los servidores. (Si no aparece, haga clic en **Administrador de Hyper-V**, haga clic en **conectar al servidor**, escriba el nombre del servidor y haga clic en **Aceptar**. Repita el proceso para agregar más servidores.)  
  
3.  Desde el **máquinas virtuales** panel, haga clic en la máquina virtual y, a continuación, haga clic en **mover**. Se abrirá al Asistente para mover. 
  
4.  Use las páginas del Asistente para elegir el tipo de movimiento, servidor de destino y las opciones.
  
5.  En la página **Resumen**, revise las selecciones realizadas y, a continuación, haga clic en **Finalizar**.  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Usar Windows PowerShell para mover una máquina virtual en ejecución
  
En el ejemplo siguiente se usa el cmdlet Move-VM para mover una máquina virtual denominada *LMTest* a un servidor de destino denominado *TestServer02* y mueve los discos duros virtuales y otros archivos, estos puntos de control y Inteligente a los archivos de paginación, la *D:\LMTest* directorio en el servidor de destino.  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>Solución de problemas

### <a name="failed-to-establish-a-connection"></a>No se pudo establecer una conexión 

Si no ha configurado la delegación restringida, debe iniciar sesión en el servidor de origen antes de poder mover una máquina virtual. Si no lo hace, el intento de autenticación se produce un error, se produce un error, y se muestra este mensaje:  
  
"Error de operación de migración de máquina Virtual en el origen de la migración.  
No se pudo establecer una conexión con el host *nombre_equipo*: No hay credenciales están disponibles en el paquete de seguridad 0x8009030E".
  
 Para corregir este problema, inicie sesión en el servidor de origen y pruebe el movimiento a intentarlo. Para evitar tener que iniciar sesión en un servidor de origen antes de realizar una migración en vivo, configure la delegación restringida. Necesitará credenciales de administrador de dominio para configurar la delegación restringida. Para obtener instrucciones, consulte [configurar hosts para migración en vivo](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md). 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>Error en el hardware del host no es compatible
 
 Si una máquina virtual no tiene compatibilidad de procesador activado y tiene una o varias instantáneas, el cambio produce un error si los hosts tienen diferentes versiones de procesador. Se produce un error y se muestra este mensaje:
 
**No se puede mover la máquina virtual en el equipo de destino. El hardware del equipo de destino no es compatible con los requisitos de hardware de esta máquina virtual.**
 
 Para corregir este problema, apague la máquina virtual y activar la configuración de compatibilidad de procesador.
 
1. Desde el Administrador de Hyper-V, en la **máquinas virtuales** panel, haga clic en la máquina virtual y haga clic en configuración.
2. En el panel de navegación, expanda **procesadores** y haga clic en **compatibilidad**.
3. Comprobar **migrar a un equipo con una versión de procesador distinta**.
4. Haga clic en **Aceptar**.
 
   Para usar Windows PowerShell, use el [Set-VMProcessor](https://technet.microsoft.com/library/hh848533.aspx) cmdlet:
 
   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```