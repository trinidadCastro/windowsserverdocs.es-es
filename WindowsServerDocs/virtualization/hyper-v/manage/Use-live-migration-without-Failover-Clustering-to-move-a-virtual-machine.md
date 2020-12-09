---
title: Usar la migración en vivo sin clústeres de conmutación por error para migrar una máquina virtual
description: Proporciona requisitos previos e instrucciones para realizar una migración en vivo en un entorno independiente.
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/17/2017
ms.openlocfilehash: f006b20c023f009bc366da97b3f7982b985aa2c4
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865764"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Usar la migración en vivo sin clústeres de conmutación por error para migrar una máquina virtual

>Se aplica a: Windows Server 2016

En este artículo se muestra cómo trasladar una máquina virtual mediante una migración en vivo sin usar clústeres de conmutación por error. Una migración en vivo mueve máquinas virtuales en ejecución entre hosts de Hyper-V sin ningún tiempo de inactividad apreciable.

Para ello, necesitará lo siguiente:

- Una cuenta de usuario que sea miembro del grupo local Administradores de Hyper-V o del grupo administradores tanto en el equipo de origen como en el de destino.

- El rol Hyper-V en Windows Server 2016 o Windows Server 2012 R2 instalado en los servidores de origen y destino, y configurado para migraciones en vivo. Puede realizar una migración en vivo entre los hosts que ejecutan Windows Server 2016 y Windows Server 2012 R2 si la máquina virtual es al menos la versión 5.

    Para obtener instrucciones de actualización de la versión, consulte [actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server 2016](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obtener instrucciones de instalación, consulte [configuración de hosts para la migración en vivo](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

- Las herramientas de administración de Hyper-V están instaladas en un equipo que ejecuta Windows Server 2016 o Windows 10, a menos que las herramientas estén instaladas en el servidor de origen o de destino y se ejecutarán desde allí.

## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>Usar el administrador de Hyper-V para trasladar una máquina virtual en ejecución

1.  Abra el administrador de Hyper-V. (En Administrador del servidor, haga clic en **herramientas**  >> **Administrador de Hyper-V**).

2.  En el panel de navegación, seleccione uno de los servidores. (Si no aparece, haga clic con el botón secundario en **Administrador de Hyper-V**, haga clic en **conectar al servidor**, escriba el nombre del servidor y haga clic en **Aceptar**. Repita este procedimiento para agregar más servidores).

3.  En el panel **virtual machines** , haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **movimiento**. Se abre el Asistente para movimiento.

4.  Use las páginas del Asistente para elegir el tipo de movimiento, el servidor de destino y las opciones.

5.  En la página **Resumen**, revise las selecciones realizadas y, a continuación, haga clic en **Finalizar**.

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Usar Windows PowerShell para trasladar una máquina virtual en ejecución

En el ejemplo siguiente se usa el cmdlet Move-VM para mover una máquina virtual llamada *LMTest* a un servidor de destino llamado *TestServer02* y mueve los discos duros virtuales y otro archivo, como los puntos de control y los archivos de paginación inteligente, al directorio *D:\LMTest* en el servidor de destino.

```
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest
```

## <a name="troubleshooting"></a>Solución de problemas

### <a name="failed-to-establish-a-connection"></a>No se pudo establecer una conexión

Si no ha configurado la delegación restringida, debe iniciar sesión en el servidor de origen para poder trasladar una máquina virtual. Si no lo hace, se producirá un error en el intento de autenticación, se producirá un error y se mostrará este mensaje:

"Error en la operación de migración de la máquina virtual en el origen de la migración.
No se pudo establecer una conexión con el *nombre del equipo* host: no hay credenciales disponibles en el paquete de seguridad 0x8009030E ".

 Para solucionar este problema, inicie sesión en el servidor de origen y vuelva a intentar el movimiento. Para evitar tener que iniciar sesión en un servidor de origen antes de realizar una migración en vivo, configure la delegación restringida. Necesitará credenciales de administrador de dominio para configurar la delegación restringida. Para obtener instrucciones, consulte [configuración de hosts para la migración en vivo](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>Error debido a que el hardware del host no es compatible

 Si una máquina virtual no tiene la compatibilidad con el procesador activada y tiene una o más instantáneas, se produce un error en el movimiento si los hosts tienen versiones de procesador diferentes. Se produce un error y se muestra este mensaje:

**No se puede migrar la máquina virtual al equipo de destino. El hardware del equipo de destino no es compatible con los requisitos de hardware de esta máquina virtual.**

 Para corregir este problema, apague la máquina virtual y active la configuración de compatibilidad del procesador.

1. En el administrador de Hyper-V, en el panel **virtual machines** , haga clic con el botón secundario en la máquina virtual y haga clic en configuración.
2. En el panel de navegación, expanda **procesadores** y haga clic en **compatibilidad**.
3. Active **migrar a un equipo con una versión de procesador diferente**.
4. Haga clic en **OK**.

   Para usar Windows PowerShell, use el cmdlet [set-VMProcessor](/powershell/module/hyper-v/set-vmprocessor) :

   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```
