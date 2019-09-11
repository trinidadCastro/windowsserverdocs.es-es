---
title: Actualizar un clúster de Espacios de almacenamiento directo a Windows Server 2019
description: 'Cómo actualizar un clúster de Espacios de almacenamiento directo a Windows Server 2019: ya sea mientras mantiene las máquinas virtuales en ejecución o mientras están detenidas.'
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9966ee0fd3c0a2d1df0180bf177dec03343efc14
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867191"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Actualizar un clúster de Espacios de almacenamiento directo a Windows Server 2019

En este tema se describe cómo actualizar un clúster de Espacios de almacenamiento directo a Windows Server 2019. Hay cuatro enfoques para actualizar un clúster de Espacios de almacenamiento directo de Windows Server 2016 a Windows Server 2019, mediante el [proceso de actualización gradual del sistema operativo del clúster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) , dos que implican mantener las máquinas virtuales en ejecución y dos que implican la detención de todas las máquinas virtuales. Cada enfoque tiene puntos fuertes y débiles diferentes, por lo que seleccione el que mejor se adapte a las necesidades de su organización:

- **Actualización en contexto mientras las máquinas virtuales se ejecutan** en cada servidor del clúster: esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero tendrá que esperar a que se completen los trabajos de almacenamiento (reparación del reflejo) una vez que se haya actualizado cada servidor.

- **Instalación limpia del sistema operativo mientras se ejecutan las máquinas virtuales** en cada servidor del clúster: esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero tendrá que esperar a que se completen los trabajos de almacenamiento (reparación del reflejo) una vez que se actualice cada servidor, y tendrá que configurar cada servidor y todo su aplicaciones y roles de nuevo.

- **Actualización en contexto mientras se detienen las máquinas virtuales** en cada servidor del clúster: esta opción incurre en tiempo de inactividad de la máquina virtual, pero no es necesario esperar los trabajos de almacenamiento (reparación del reflejo), por lo que es más rápido.

- **Limpiar: instalación del sistema operativo mientras se detienen las máquinas virtuales** en cada servidor del clúster: esta opción incurrirá en tiempo de inactividad de la máquina virtual, pero no es necesario esperar los trabajos de almacenamiento (reparación del reflejo) para que sea más rápido.

## <a name="prerequisites-and-limitations"></a>Requisitos previos y limitaciones

Antes de continuar con una actualización:

- Compruebe que ha utilizado copias de seguridad en caso de que haya problemas durante el proceso de actualización.

- Compruebe que el proveedor de hardware tiene un BIOS, firmware y controladores para los servidores que serán compatibles en Windows Server 2019.

Existen algunas limitaciones en cuanto al proceso de actualización que debe tener en cuenta:

- Para habilitar Espacios de almacenamiento directo con las compilaciones de Windows Server 2019 anteriores a 176693,292, es posible que los clientes necesiten ponerse en contacto con el soporte técnico de Microsoft para las claves del registro que Espacios de almacenamiento directo permiten la funcionalidad de redes definidas por el software y Para obtener más información, vea el artículo de Microsoft Knowledge base [4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking).

- Al actualizar un clúster con volúmenes ReFS, hay algunas limitaciones:

- La actualización es totalmente compatible con los volúmenes ReFS; sin embargo, los volúmenes actualizados no se beneficiarán de las mejoras de ReFS en Windows Server 2019. Estas ventajas, como el aumento del rendimiento de la paridad con aceleración de reflejo, requieren un volumen de ReFS de Windows Server 2019 recién creado. En otras palabras, tendría que crear nuevos volúmenes con el cmdlet o `New-Volume` el administrador del servidor. Estas son algunas de las mejoras de ReFS que obtendrían los nuevos volúmenes:

    - **Asignación de registro: bypass**: mejora del rendimiento en ReFS que solo se aplica a los sistemas (espacios de almacenamiento directo) en clúster y no se aplica a los grupos de almacenamiento independientes.

    - Mejoras en la eficiencia de la **compactación** en Windows Server 2019 que son específicas de los volúmenes con varios resistentes.

- Antes de actualizar un servidor de clúster de Espacios de almacenamiento directo de Windows Server 2016, se recomienda poner el servidor en modo de mantenimiento de almacenamiento. Para obtener más información, consulte la sección evento 5120 de [solucionar problemas espacios de almacenamiento directo](troubleshooting-storage-spaces.md). Aunque este problema se ha corregido en Windows Server 2016, se recomienda poner cada Espacios de almacenamiento directo servidor en modo de mantenimiento de almacenamiento durante la actualización como procedimiento recomendado.

- Existe un problema conocido con los entornos de red definidos por software que usan conmutadores establecidos. Este problema implica la migración en vivo de máquinas virtuales de Hyper-V de Windows Server 2019 a Windows Server 2016 (migración en vivo a un sistema operativo anterior). Para asegurarse de que las migraciones en vivo se realicen correctamente, se recomienda cambiar la configuración de red de la máquina virtual en las máquinas virtuales que se migran en vivo desde Windows Server 2019 a Windows Server 2016. Este problema se ha corregido para Windows Server 2019 en el paquete acumulativo de revisiones de 2019-01D, también conocido como Build 17763,292. Para obtener más información, vea el artículo de Microsoft Knowledge base [4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976).

Debido a los problemas conocidos descritos anteriormente, algunos clientes pueden considerar la posibilidad de compilar un nuevo clúster de Windows Server 2019 y copiar datos del clúster anterior, en lugar de actualizar los clústeres de Windows Server 2016 con uno de los cuatro procesos descritos a continuación.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Realización de una actualización en contexto mientras se ejecutan las máquinas virtuales

Esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero tendrá que esperar a que se completen los trabajos de almacenamiento (reparación del reflejo) después de actualizar cada servidor. Aunque los servidores individuales se reiniciarán secuencialmente durante el proceso de actualización, los demás servidores del clúster, así como todas las máquinas virtuales, seguirán ejecutándose.

1. Compruebe que todos los servidores del clúster tengan instaladas las últimas actualizaciones de Windows. Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Como mínimo, instale el [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de Microsoft Knowledge base (19 de febrero de 2019). El número de compilación ( `ver` vea el comando) debe ser 14393,2828 o superior.

2. Si usa redes definidas por software con switches SET, abra una sesión de PowerShell con privilegios elevados y ejecute el siguiente comando para deshabilitar las comprobaciones de comprobación de la migración en vivo de máquinas virtuales en todas las máquinas virtuales del clúster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   1. Use la migración en vivo de máquinas virtuales de Hyper-V para trasladar las máquinas virtuales en ejecución fuera del servidor que va a actualizar.

   2. PAUSE el servidor de clúster con el siguiente comando de PowerShell: tenga en cuenta que algunos grupos internos están ocultos. Se recomienda que este paso sea cauto: Si aún no ha migrado en vivo las máquinas virtuales del servidor, este cmdlet lo hará por usted, por lo que puede omitir el paso anterior si lo prefiere.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Coloque el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Ejecute el siguiente cmdlet para comprobar que el valor de **OperationalStatus** está **en modo de mantenimiento**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Realice una instalación de actualización de Windows Server 2019 en el servidor mediante la ejecución de **setup. exe** y el uso de la opción "mantener archivos y aplicaciones personales". Una vez completada la instalación, el servidor permanece en el clúster y el servicio de clúster se inicia automáticamente.

   6. Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019. Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). El número de compilación ( `ver` vea el comando) debe ser 17763,292 o superior.

   7. Quite el servidor del modo de mantenimiento de almacenamiento mediante el siguiente comando de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Reanude el servidor con el siguiente comando de PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Espere a que finalicen los trabajos de reparación de almacenamiento y que todos los discos vuelvan a un estado correcto. Esto puede tardar un tiempo considerable en función del número de máquinas virtuales que se ejecutan durante la actualización del servidor. Estos son los comandos que se ejecutan:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Actualice el siguiente servidor del clúster.

5. Una vez actualizados todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tiene hasta cuatro semanas para hacerlo.

6. Una vez actualizado el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento. En este momento, los nuevos cmdlets como `Get-ClusterPerf` estarán totalmente operativos en cualquier servidor del clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, actualice los niveles de configuración de la máquina virtual mediante `Update-VMVersion` la detención de cada máquina virtual, con el cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

8. Si usa redes definidas por software con los conmutadores establecidos y las comprobaciones de migración en vivo de máquinas virtuales deshabilitadas como se indicó anteriormente, use el siguiente cmdlet para volver a habilitar las comprobaciones de verificación en directo de la máquina virtual:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Compruebe que el clúster actualizado funciona según lo previsto. Los roles deben conmutar por error correctamente y, si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar en vivo correctamente.

10. Valide el clúster mediante la ejecución de la`Test-Cluster`validación de clústeres () y el examen del informe de validación de clúster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Realizar una instalación limpia del sistema operativo mientras se ejecutan las máquinas virtuales

Esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero tendrá que esperar a que se completen los trabajos de almacenamiento (reparación del reflejo) después de actualizar cada servidor. Aunque los servidores individuales se reiniciarán secuencialmente durante el proceso de actualización, los demás servidores del clúster, así como todas las máquinas virtuales, seguirán ejecutándose.

1. Compruebe que todos los servidores del clúster ejecuten las actualizaciones más recientes. Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Como mínimo, instale el [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de Microsoft Knowledge base (19 de febrero de 2019). El número de compilación ( `ver` vea el comando) debe ser 14393,2828 o superior.

2. Si usa redes definidas por software con switches SET, abra una sesión de PowerShell con privilegios elevados y ejecute el siguiente comando para deshabilitar las comprobaciones de comprobación de la migración en vivo de máquinas virtuales en todas las máquinas virtuales del clúster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   1. Use la migración en vivo de máquinas virtuales de Hyper-V para trasladar las máquinas virtuales en ejecución fuera del servidor que va a actualizar.

   2. PAUSE el servidor de clúster con el siguiente comando de PowerShell: tenga en cuenta que algunos grupos internos están ocultos. Se recomienda que este paso sea cauto: Si aún no ha migrado en vivo las máquinas virtuales del servidor, este cmdlet lo hará por usted, por lo que puede omitir el paso anterior si lo prefiere.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Coloque el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Ejecute el siguiente cmdlet para comprobar que el valor de **OperationalStatus** está **en modo de mantenimiento**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Expulsar el servidor del clúster mediante la ejecución del siguiente comando de PowerShell:  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Realice una instalación limpia de Windows Server 2019 en el servidor: formatee la unidad del sistema, ejecute **setup. exe** y use la opción "Nothing". Tendrá que configurar la identidad del servidor, los roles, las características y las aplicaciones una vez finalizada la instalación y se reinicie el servidor.

   5. Instale el rol de Hyper-V y la característica de clústeres de conmutación por error en el servidor `Install-WindowsFeature` (puede usar el cmdlet).

   6. Instale los controladores de red y almacenamiento más recientes para el hardware aprobado por el fabricante del servidor para su uso con Espacios de almacenamiento directo.

   7. Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019. Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). El número de compilación ( `ver` vea el comando) debe ser 17763,292 o superior.

   8. Vuelva a unir el servidor al clúster mediante el siguiente comando de PowerShell:

       ```PowerShell
       Add-ClusterNode
       ```

   9. Quite el servidor del modo de mantenimiento de almacenamiento mediante los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Espere a que finalicen los trabajos de reparación de almacenamiento y que todos los discos vuelvan a un estado correcto. Esto puede tardar un tiempo considerable en función del número de máquinas virtuales que se ejecutan durante la actualización del servidor. Estos son los comandos que se ejecutan:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Actualice el siguiente servidor del clúster.

5. Una vez actualizados todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tiene hasta cuatro semanas para hacerlo.

6. Una vez actualizado el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento. En este momento, los nuevos cmdlets como `Get-ClusterPerf` estarán totalmente operativos en cualquier servidor del clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, actualice los niveles de configuración de la máquina virtual mediante `Update-VMVersion` la detención de cada máquina virtual y el uso del cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

8. Si usa redes definidas por software con los conmutadores establecidos y las comprobaciones de migración en vivo de máquinas virtuales deshabilitadas como se indicó anteriormente, use el siguiente cmdlet para volver a habilitar las comprobaciones de verificación en directo de la máquina virtual:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Compruebe que el clúster actualizado funciona según lo previsto. Los roles deben conmutar por error correctamente y, si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar en vivo correctamente.

10. Valide el clúster mediante la ejecución de la`Test-Cluster`validación de clústeres () y el examen del informe de validación de clúster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Realización de una actualización en contexto mientras se detienen las máquinas virtuales

Esta opción incurre en el tiempo de inactividad de la máquina virtual, pero puede tardar menos tiempo que si las máquinas virtuales se ejecutan durante la actualización, ya que no es necesario esperar a que se completen los trabajos de almacenamiento (reparación del reflejo) después de actualizar cada servidor. Aunque los servidores individuales se reiniciarán secuencialmente durante el proceso de actualización, los demás servidores del clúster seguirán ejecutándose.

1. Compruebe que todos los servidores del clúster ejecuten las actualizaciones más recientes. Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Como mínimo, instale el [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de Microsoft Knowledge base (19 de febrero de 2019). El número de compilación ( `ver` vea el comando) debe ser 14393,2828 o superior.

2. Detenga las máquinas virtuales que se ejecutan en el clúster.

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   1. PAUSE el servidor de clúster con el siguiente comando de PowerShell: tenga en cuenta que algunos grupos internos están ocultos. Se recomienda este paso para tener cuidado.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Coloque el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Ejecute el siguiente cmdlet para comprobar que el valor de **OperationalStatus** está **en modo de mantenimiento**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Realice una instalación de actualización de Windows Server 2019 en el servidor mediante la ejecución de **setup. exe** y el uso de la opción "mantener archivos y aplicaciones personales".  
   Una vez completada la instalación, el servidor permanece en el clúster y el servicio de clúster se inicia automáticamente.

   5.  Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019.  
   Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   El número de compilación ( `ver` vea el comando) debe ser 17763,292 o superior.

   6.  Quite el servidor del modo de mantenimiento de almacenamiento mediante los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Reanude el servidor con el siguiente comando de PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Espere a que finalicen los trabajos de reparación de almacenamiento y que todos los discos vuelvan a un estado correcto.  
   Esto debe ser relativamente rápido, ya que las máquinas virtuales no se están ejecutando. Estos son los comandos que se ejecutan:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Actualice el siguiente servidor del clúster.
5. Una vez actualizados todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tiene hasta cuatro semanas para hacerlo.

6. Una vez actualizado el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento.  
   En este momento, los nuevos cmdlets como `Get-ClusterPerf` estarán totalmente operativos en cualquier servidor del clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Inicie las máquinas virtuales en el clúster y compruebe que funcionan correctamente.

8. Opcionalmente, actualice los niveles de configuración de la máquina virtual mediante `Update-VMVersion` la detención de cada máquina virtual y el uso del cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

9. Compruebe que el clúster actualizado funciona según lo previsto.  
   Los roles deben conmutar por error correctamente y, si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar en vivo correctamente.

10. Valide el clúster mediante la ejecución de la`Test-Cluster`validación de clústeres () y el examen del informe de validación de clúster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Realizar una instalación limpia del sistema operativo mientras se detienen las máquinas virtuales

Esta opción incurre en el tiempo de inactividad de la máquina virtual, pero puede tardar menos tiempo que si las máquinas virtuales se ejecutan durante la actualización, ya que no es necesario esperar a que se completen los trabajos de almacenamiento (reparación del reflejo) después de actualizar cada servidor. Aunque los servidores individuales se reiniciarán secuencialmente durante el proceso de actualización, los demás servidores del clúster seguirán ejecutándose.

1. Compruebe que todos los servidores del clúster ejecuten las actualizaciones más recientes.  
   Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   Como mínimo, instale el [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de Microsoft Knowledge base (19 de febrero de 2019). El número de compilación ( `ver` vea el comando) debe ser 14393,2828 o superior.

2. Detenga las máquinas virtuales que se ejecutan en el clúster.

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   2. PAUSE el servidor de clúster con el siguiente comando de PowerShell: tenga en cuenta que algunos grupos internos están ocultos.  
      Se recomienda este paso para tener cuidado.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Coloque el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Ejecute el siguiente cmdlet para comprobar que el valor de **OperationalStatus** está **en modo de mantenimiento**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Expulsar el servidor del clúster mediante la ejecución del siguiente comando de PowerShell:  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Realice una instalación limpia de Windows Server 2019 en el servidor: formatee la unidad del sistema, ejecute **setup. exe** y use la opción "Nothing".  
      Tendrá que configurar la identidad del servidor, los roles, las características y las aplicaciones una vez finalizada la instalación y se reinicie el servidor.

   7. Instale el rol de Hyper-V y la característica de clústeres de conmutación por error en el servidor `Install-WindowsFeature` (puede usar el cmdlet).

   8. Instale los controladores de red y almacenamiento más recientes para el hardware aprobado por el fabricante del servidor para su uso con Espacios de almacenamiento directo.

   9. Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019.  
      Para obtener más información, consulte el [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      El número de compilación ( `ver` vea el comando) debe ser 17763,292 o superior.

   10. Vuelva a unir el servidor al clúster mediante el siguiente comando de PowerShell:

      ```PowerShell
      Add-ClusterNode
      ```

   11. Quite el servidor del modo de mantenimiento de almacenamiento mediante el siguiente comando de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Espere a que finalicen los trabajos de reparación de almacenamiento y que todos los discos vuelvan a un estado correcto.  
       Esto puede tardar un tiempo considerable en función del número de máquinas virtuales que se ejecutan durante la actualización del servidor. Estos son los comandos que se ejecutan:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Actualice el siguiente servidor del clúster.

5. Una vez actualizados todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tiene hasta cuatro semanas para hacerlo.

6. Una vez actualizado el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento.  
   En este momento, los nuevos cmdlets como `Get-ClusterPerf` estarán totalmente operativos en cualquier servidor del clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Inicie las máquinas virtuales en el clúster y compruebe que funcionan correctamente.

8. Opcionalmente, actualice los niveles de configuración de la máquina virtual mediante `Update-VMVersion` la detención de cada máquina virtual y el uso del cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

9. Compruebe que el clúster actualizado funciona según lo previsto.  
   Los roles deben conmutar por error correctamente y, si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar en vivo correctamente.

10. Valide el clúster mediante la ejecución de la`Test-Cluster`validación de clústeres () y el examen del informe de validación de clúster.
