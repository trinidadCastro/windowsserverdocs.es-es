---
title: Desconectar el servidor de Espacios de almacenamiento directo para realizar trabajos de mantenimiento
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 2ccf8d809354f96277701cd365966ba5e914f64b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857538"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>Desconectar el servidor de Espacios de almacenamiento directo para realizar trabajos de mantenimiento

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones para reiniciar o apagar servidores de forma correcta con [Espacios de almacenamiento directo](storage-spaces-direct-overview.md).

Si usas Espacios de almacenamiento directo para desactivar un servidor (apagarlo), ten en cuenta que también se desactivarán la partes del almacenamiento que se hayan compartido en todos los servidores del clúster. Para esto, debes pausar (suspender) el servidor que quieras desconectar, mover los roles a otros servidores del clúster y comprobar que todos los datos estén disponibles en los otros servidores del clúster; gracias a ello, los datos permanecerán seguros y podrás acceder a ellos durante la tarea de mantenimiento.

Sigue los siguientes pasos para pausar correctamente un servidor en un clúster de Espacios de almacenamiento directo antes de desactivarlo. 

   > [!IMPORTANT]
   > Para instalar actualizaciones en un clúster de Espacios de almacenamiento directo, usa la Actualización compatible con clústeres (CAU); esta característica se encargará de realizar automáticamente los pasos que se detallan en este tema, para que tú no tengas que hacerlo. Para obtener más información, consulta [Actualización compatible con clústeres (CAU)](https://technet.microsoft.com/library/hh831694.aspx).

## <a name="verifying-its-safe-to-take-the-server-offline"></a>Comprobar que sea seguro desconectar el servidor

Antes de desconectar el servidor para realizar trabajos de mantenimiento, comprueba que todos los volúmenes están en buenas condiciones.

Para ello, abre una sesión de PowerShell con los permisos de administrador y ejecuta el siguiente comando para ver el estado de los volúmenes:

```PowerShell
Get-VirtualDisk 
```

Aquí te mostramos un ejemplo de lo que verás:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Comprueba que la propiedad **HealthStatus** de todo volumen (disco virtual) es **Correcta**.

Para realizar esto en el Administrador de clústeres de conmutación por error, ve a **Almacenamiento** > **Discos**.

Comprueba que la columna **Estado** de cada volumen (disco virtual) se muestre **En línea**.

## <a name="pausing-and-draining-the-server"></a>Pausar y purgar el servidor

Antes de reiniciar o apagar el servidor, pausa y purga (retira) cualquier tipo de rol como, por ejemplo, las máquinas virtuales que se ejecutan en el servidor. Gracias a esto, Espacios de almacenamiento directo podrá vaciar y confirmar los datos correctamente, para así garantizar que el apagado se realice sin percances teniendo en cuenta las aplicaciones que se ejecutan en ese servidor.

   > [!IMPORTANT]
   > Recuerda que debes pausar y purgar los servidores en clúster antes de reiniciarlos o apagarlos.

En PowerShell, ejecuta el siguiente cmdlet (como administrador) para pausar y purgar.

```PowerShell
Suspend-ClusterNode -Drain
```

Para hacer esto en el Administrador de clústeres de conmutación por error, ve a **Nodos**, haz clic con el botón derecho en el nodo y, a continuación, selecciona **Pausar** > **Purgar roles**.

![Pausar y purgar](media/maintain-servers/pause-drain.png)

Todas las máquinas virtuales comenzarán a migrar a otros servidores del clúster. Esto puede tardar varios minutos.

   > [!NOTE]
   > Cuando pauses y purgues el nodo del clúster, Windows comprobará de forma automática la seguridad para garantizar que puedas continuar sin problemas. Si hay volúmenes incorrectos, se detendrá el proceso y recibirás una alerta indicando que no es seguro continuar.

![Comprobación de seguridad](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>Apagar el servidor

Una vez purgado el servidor, se mostrará **En pausa** en PowerShell y en el Administrador de clústeres de conmutación por error.

![En pausa](media/maintain-servers/paused.png)

Ya puedes reiniciarlo o apagarlo con seguridad, tal como lo harías normalmente (por ejemplo, mediante los cmdlets Restart-Computer o Stop-Computer PowerShell).

```PowerShell
Get-VirtualDisk 

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

El estado operativo incompleto o degradado es normal cuando los nodos se están cerrando o iniciando o deteniendo el servicio de clúster en un nodo y no deben suponer un problema. Todos los volúmenes permanecen en línea y son accesibles.

## <a name="resuming-the-server"></a>Reanudar el servidor

Cuando estés listo para que el servidor vuelva a hospedar cargas de trabajo, reanúdalo.

En PowerShell, ejecuta el siguiente cmdlet (como administrador) para reanudar el proceso.

```PowerShell
Resume-ClusterNode
```

Para mover los roles que se ejecutaban anteriormente en esta copia del servidor, usa la marca opcional **-Failback**.

```PowerShell
Resume-ClusterNode –Failback Immediate
```

Para hacer esto en el Administrador de clústeres de conmutación por error, ve a **Nodos**, haz clic con el botón derecho en el nodo y, a continuación, selecciona **Reanudar** > **Conmutación por recuperación de roles**.

![Resume-Failback](media/maintain-servers/resume-failback.png)

## <a name="waiting-for-storage-to-resync"></a>Esperar a que se sincronice de nuevo el almacenamiento

Cuando el servidor se reanuda, las nuevas escrituras que se hayan producido mientras no estaba disponible deben volver a sincronizarse. Esto sucede automáticamente. Si usas la opción de seguimiento de cambios inteligente, no es necesario analizar o sincronizar *todos* los datos; solo deberás tener en cuenta los cambios. Este proceso está limitado para mitigar el impacto en las cargas de trabajo de producción. Dependiendo del tiempo que el servidor esté en pausa y la cantidad de datos nuevos escritos, es posible que el proceso tarde unos minutos en completarse.

Debes esperar a que la resincronización se complete antes de desactivar otros servidores del clúster.

En PowerShell, ejecuta el siguiente cmdlet (como administrador) para supervisar el proceso.

```PowerShell
Get-StorageJob
```
Aquí verás unos ejemplos que muestran los trabajos de resincronización (reparación):
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

**BytesTotal** muestra la cantidad de espacio de almacenamiento que necesitas para resincronizar. **PercentComplete** muestra el progreso del proceso.

   > [!WARNING]
   > Recuerda que no es seguro desconectar ningún otro servidor hasta que finalicen los trabajos de reparación.

Durante este tiempo, los volúmenes seguirán mostrando un mensaje de **advertencia**, pero esto es normal. 

Por ejemplo, si usas el cmdlet `Get-VirtualDisk`, es posible que veas el siguiente resultado:
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

Una vez finalizados los trabajos, comprueba que los volúmenes muestran de nuevo el aviso **Correcto** mediante el cmdlet `Get-VirtualDisk`. Este es un ejemplo de los resultados:

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Ya puedes pausar y reiniciar otros servidores del clúster.

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


## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Actualización compatible con clústeres (CAU)](https://technet.microsoft.com/library/hh831694.aspx)
