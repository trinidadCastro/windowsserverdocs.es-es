---
title: Desconectar un servidor de Espacios de almacenamiento directo para su mantenimiento
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 8dba155f8b8d7312a823dedc72d23268d7d13fbf
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955917"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>Desconectar un servidor de Espacios de almacenamiento directo para su mantenimiento

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones sobre cómo reiniciar o apagar correctamente los servidores con [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

Con Espacios de almacenamiento directo, desconectar un servidor (detenerlo) también significa desconectar partes sin conexión del almacenamiento que se comparten entre todos los servidores del clúster. Para ello, es necesario poner en pausa (suspender) el servidor que desea desconectar, mover roles a otros servidores del clúster y comprobar que todos los datos están disponibles en los demás servidores del clúster para que los datos permanezcan seguros y sean accesibles a lo largo del mantenimiento.

Use los procedimientos siguientes para pausar correctamente un servidor en un clúster de Espacios de almacenamiento directo antes de desconectarlo.

   > [!IMPORTANT]
   > Para instalar actualizaciones en un clúster de Espacios de almacenamiento directo, use la actualización compatible con clústeres (CAU), que realiza automáticamente los procedimientos de este tema para que no tenga que hacerlo al instalar las actualizaciones. Para obtener más información, consulte [actualización compatible con clústeres (CAU)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831694(v=ws.11)).

## <a name="verifying-its-safe-to-take-the-server-offline"></a>Comprobando que es seguro desconectar el servidor

Antes de desconectar un servidor para el mantenimiento, compruebe que todos los volúmenes están en buen estado.

Para ello, abra una sesión de PowerShell con permisos de administrador y, después, ejecute el siguiente comando para ver el estado del volumen:

```PowerShell
Get-VirtualDisk
```

Este es un ejemplo del aspecto que podría tener la salida:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Compruebe que la propiedad **HealthStatus** para cada volumen (disco virtual) sea **correcta**.

Para hacer esto en Administrador de clústeres de conmutación por error, vaya a **Storage**  >  **discos**de almacenamiento.

Compruebe que en la columna **Estado** de cada volumen (disco virtual) aparece **en línea**.

## <a name="pausing-and-draining-the-server"></a>Pausar y purgar el servidor

Antes de reiniciar o apagar el servidor, PAUSE y vacíe (saque) cualquier rol, como las máquinas virtuales que se ejecutan en él. Esto también proporciona Espacios de almacenamiento directo una oportunidad para vaciar y confirmar los datos correctamente para asegurarse de que el apagado es transparente para las aplicaciones que se ejecutan en ese servidor.

   > [!IMPORTANT]
   > PAUSE y vacíe siempre los servidores en clúster antes de reiniciarlos o cerrarlos.

En PowerShell, ejecute el siguiente cmdlet (como administrador) para pausar y purgar.

```PowerShell
Suspend-ClusterNode -Drain
```

Para hacer esto en Administrador de clústeres de conmutación por error, vaya a **nodos**, haga clic con el botón secundario en el nodo y seleccione **pausar**  >  **roles de purga**.

![Pausar-Drain](media/maintain-servers/pause-drain.png)

Todas las máquinas virtuales comenzarán a migrar en vivo a otros servidores del clúster. Esta operación puede tardar unos minutos.

   > [!NOTE]
   > Cuando pausa y purga el nodo de clúster correctamente, Windows realiza una comprobación de seguridad automática para asegurarse de que es seguro continuar. Si hay volúmenes en mal estado, se detendrán y le avisarán de que no es seguro continuar.

![Comprobación de seguridad](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>Apagar el servidor

Una vez que se haya agotado el servidor, se mostrará en **pausa** en Administrador de clústeres de conmutación por error y PowerShell.

![En pausa](media/maintain-servers/paused.png)

Ahora puede reiniciarse o cerrarse de forma segura, tal como lo haría normalmente (por ejemplo, mediante los cmdlets de PowerShell restart-Computer o STOP-Computer).

```PowerShell
Get-VirtualDisk

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

El estado operativo incompleto o degradado es normal cuando los nodos se están cerrando o iniciando o deteniendo el servicio de clúster en un nodo y no deben suponer un problema. Todos los volúmenes permanecen en línea y accesibles.

## <a name="resuming-the-server"></a>Reanudando el servidor

Cuando esté listo para que el servidor empiece a hospedar las cargas de trabajo de nuevo, reanude la reanudación.

En PowerShell, ejecute el siguiente cmdlet (como administrador) para reanudar.

```PowerShell
Resume-ClusterNode
```

Para trasladar de nuevo los roles que se estaban ejecutando previamente en este servidor, use la marca **de conmutación por recuperación** opcional.

```PowerShell
Resume-ClusterNode –Failback Immediate
```

Para hacer esto en Administrador de clústeres de conmutación por error, vaya a **nodos**, haga clic con el botón secundario en el nodo y, después, seleccione **reanudar**  >  **errores de roles de nuevo**.

![Reanudar: conmutación por recuperación](media/maintain-servers/resume-failback.png)

## <a name="waiting-for-storage-to-resync"></a>Esperando a que se resincronice el almacenamiento

Cuando el servidor se reanuda, las nuevas escrituras que se hayan producido mientras no estaba disponible deben volver a sincronizarse. Esto sucede automáticamente. Con el seguimiento de cambios inteligente, no es necesario que *todos los* datos se analicen ni sincronicen. solo los cambios. Este proceso se limita para mitigar el impacto en las cargas de trabajo de producción. En función de cuánto tiempo se haya pausado el servidor y de la cantidad de datos nuevos que se escriban, puede tardar varios minutos en completarse.

Debe esperar a que la resincronización se complete antes de dejar sin conexión los demás servidores del clúster.

En PowerShell, ejecute el siguiente cmdlet (como administrador) para supervisar el progreso.

```PowerShell
Get-StorageJob
```
Este es un ejemplo de salida, que muestra los trabajos de resincronización (reparación):
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

El **bytesTotal** muestra la cantidad de almacenamiento necesario para la resincronización. La **PercentComplete** muestra el progreso.

   > [!WARNING]
   > No es seguro desconectar otro servidor hasta que finalicen estos trabajos de reparación.

Durante este tiempo, los volúmenes se seguirán mostrando como **ADVERTENCIA**, que es normal.

Por ejemplo, si usa el `Get-VirtualDisk` cmdlet, es posible que vea el siguiente resultado:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

Una vez completados los trabajos, compruebe que los volúmenes vuelvan a funcionar **correctamente** mediante el `Get-VirtualDisk` cmdlet. A continuación se muestra una salida de ejemplo:

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Ahora es seguro pausar y reiniciar otros servidores del clúster.

## <a name="how-to-update-storage-spaces-direct-nodes-offline"></a>Actualización de nodos de Espacios de almacenamiento directo sin conexión
Siga estos pasos para realizar una ruta de acceso al sistema de Espacios de almacenamiento directo rápidamente. Implica la programación de una ventana de mantenimiento y la desactivación del sistema para la revisión. Si hay una actualización de seguridad crítica que necesita aplicar rápidamente o quizás necesite asegurarse de que la revisión se completa en la ventana de mantenimiento, este método puede ser para usted. Este proceso desconecta el clúster de Espacios de almacenamiento directo, lo revisa y lo vuelve a poner en marcha. La desventaja es el tiempo de inactividad de los recursos hospedados.

1. Planee la ventana de mantenimiento.
2. Desconecte los discos virtuales.
3. Detenga el clúster para desconectar el bloque de almacenamiento. Ejecute el cmdlet **Stop-Cluster** o use administrador de clústeres de conmutación por error para detener el clúster.
4. Establezca el servicio de clúster en **deshabilitado** en Services. msc en cada nodo. Esto evita que se inicie el servicio de clúster mientras se aplica la revisión.
5. Aplique la actualización acumulativa de Windows Server y cualquier actualización de pila de mantenimiento necesaria a todos los nodos. (Puede actualizar todos los nodos al mismo tiempo, sin necesidad de esperar, ya que el clúster está inactivo).
6. Reinicie los nodos y asegúrese de que todo es correcto.
7. Vuelva a establecer el servicio de clúster en **automático** en cada nodo.
8. Inicie el clúster. Ejecute el cmdlet **Start-Cluster** o use administrador de clústeres de conmutación por error.

   Asígnele unos minutos.  Asegúrese de que el grupo de almacenamiento esté en buen estado.
9. Vuelva a poner en línea los discos virtuales.
10. Supervise el estado de los discos virtuales mediante la ejecución de los cmdlets **Get-Volume** y **Get-VirtualDisk** .


## <a name="additional-references"></a>Referencias adicionales

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Actualización compatible con clústeres (CAU)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831694(v=ws.11))
