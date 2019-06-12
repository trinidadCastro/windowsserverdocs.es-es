---
title: Actualizar un clúster de espacios de almacenamiento directo en Windows Server 2019
description: Cómo actualizar un clúster de espacios de almacenamiento directo en Windows Server 2019, ya sea al tiempo que mantiene las máquinas virtuales que ejecutan o mientras se está detenido.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9db92aa33cde9b2beed11149dae06bb3af2b5a03
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455409"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Actualizar un clúster de espacios de almacenamiento directo en Windows Server 2019

Este tema describe cómo actualizar un clúster de espacios de almacenamiento directo en Windows Server 2019. Existen cuatro enfoques para actualizar un clúster de espacios de almacenamiento directo desde Windows Server 2016 para Windows Server 2019, mediante el [del proceso de actualización gradual de SO del clúster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) : dos que implican mantener las máquinas virtuales que ejecutan y dos que implican Detener todas las máquinas virtuales. Cada enfoque tiene distintas ventajas e inconvenientes, así que seleccione que uno que mejor se adapte a las necesidades de su organización:

- **Actualización local mientras se ejecutan las máquinas virtuales** en cada servidor del clúster, esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero deberá esperar los trabajos de almacenamiento (reparación de espejo) completar después de actualizar cada servidor.

- **Instalación de SO limpiar mientras se ejecutan las máquinas virtuales** en cada servidor del clúster: esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero deberá esperar los trabajos de almacenamiento (reparación de espejo) completar una vez que se actualice cada servidor, y debe establecer seguridad de cada servidor y todos sus aplicaciones y los roles de nuevo.

- **Actualización local mientras se detienen las máquinas virtuales** en cada servidor del clúster, esta opción incurre en tiempo de inactividad de máquina virtual, pero no es necesario esperar para trabajos de almacenamiento (reparación de espejo), por lo que es más rápido.

- **OS limpiar instalar mientras se detienen las máquinas virtuales** en cada servidor del clúster, esta opción incurre en tiempo de inactividad de máquina virtual, pero no es necesario esperar para almacenamiento trabajos (reparación de espejo), por lo que es más rápido.

## <a name="prerequisites-and-limitations"></a>Requisitos previos y limitaciones

Antes de continuar con una actualización:

- Compruebe que tiene copias de seguridad pueden usar en caso de que hay algún problema durante el proceso de actualización.

- Compruebe que el proveedor de hardware tenga un BIOS, firmware y controladores para los servidores que serán compatibles con Windows Server 2019.

Existen algunas limitaciones con el proceso de actualización para tener en cuenta:

- Para permitir que espacios de almacenamiento directo con Windows Server 2019 compilaciones anteriores a 176693.292, los clientes que deba ponerse en contacto con soporte técnico de Microsoft para las claves del registro que habilitan la funcionalidad de espacios de almacenamiento directo y redes definidas por Software. Para obtener más información, consulte Microsoft Knowledge Base [artículo 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking).

- Al actualizar un clúster con volúmenes de ReFS, hay algunas limitaciones:

- La actualización es totalmente compatible con volúmenes de ReFS, sin embargo, los volúmenes actualizados no beneficiarán de ReFS las mejoras en Windows Server 2019. Estas ventajas, como un mayor rendimiento acelerado reflejado paridad y requieren un volumen de Windows Server 2019 ReFS recién creado. En otras palabras, tendría que crear nuevos volúmenes mediante el `New-Volume` cmdlet o el administrador del servidor. Estas son algunas de las mejoras de ReFS obtendrían nuevos volúmenes:

    - **Omisión de registro de mapa**: una mejora del rendimiento en ReFS que solo se aplica a los sistemas en clúster (espacios de almacenamiento directo) y no se aplica a los grupos de almacenamiento independiente.

    - **Compactación** las mejoras de eficiencia en Windows Server 2019 resistentes varios volúmenes específicos.

- Antes de actualizar un servidor de clúster de espacios de almacenamiento de Windows Server 2016 directo, se recomienda colocar el servidor en modo de mantenimiento de almacenamiento. Para obtener más información, consulte la sección eventos 5120 de [solucionar problemas de espacios de almacenamiento directo](troubleshooting-storage-spaces.md). Aunque este problema se corrigió en Windows Server 2016, se recomienda colocar cada servidor de espacios de almacenamiento directo en modo de mantenimiento de almacenamiento durante la actualización como una práctica recomendada.

- Hay un problema conocido con los entornos de redes definidas por Software que usan el conjunto de modificadores. Este problema consiste en migraciones en vivo de máquinas virtuales de Hyper-V de Windows Server 2019 a Windows Server 2016 (migración en vivo a un sistema operativo anterior). Para garantizar la correcta migraciones en vivo, se recomienda cambiar una configuración de red de máquina virtual en máquinas virtuales que se migran en vivo de 2019 de Windows Server a Windows Server 2016. Este problema se ha corregido para 2019 de Windows Server en el paquete acumulativo de revisiones de 2019-01D, también conocido como compilación 17763.292. Para obtener más información, consulte Microsoft Knowledge Base [artículo 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976).

Debido a los problemas conocidos anteriores, algunos clientes puede crear un nuevo clúster de Windows Server 2019 y copiar datos desde el clúster anterior, en lugar de actualizar sus clústeres de Windows Server 2016 mediante uno de los cuatro procesos que se describe a continuación.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Realizar una actualización en contexto mientras se ejecutan las máquinas virtuales

Esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero deberá esperar los trabajos de almacenamiento (reparación de espejo) completar después de actualizar cada servidor. Aunque los servidores individuales se reiniciará de forma secuencial durante el proceso de actualización, el resto de servidores en el clúster, así como todas las máquinas virtuales, seguirán ejecutándose.

1. Compruebe que todos los servidores del clúster han instalado las actualizaciones más recientes de Windows. Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Como mínimo, instale Microsoft Knowledge Base [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de febrero de 2019). El número de compilación (vea `ver` comando) debe ser 14393.2828 o posterior.

2. Si usa redes definidas por Software con el conjunto de modificadores, abra una sesión de PowerShell con privilegios elevados y ejecute el siguiente comando para deshabilitar las comprobaciones de la migración en vivo de máquinas virtuales en todas las máquinas virtuales en el clúster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   1. Utilice migración en vivo de máquinas virtuales de Hyper-V para mover máquinas virtuales en ejecución fuera del servidor que va a actualizar.

   2. Pausar el servidor de clúster mediante el siguiente comando de PowerShell, tenga en cuenta que algunos grupos internos están ocultos. Se recomienda este paso ir con cuidado, si ya no vive migrar máquinas virtuales fuera del servidor de este cmdlet hará lo que, por lo que podría omitir el paso anterior si lo prefiere.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Colocar el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Ejecute el siguiente cmdlet para comprobar que la **OperationalStatus** valor es **en modo de mantenimiento**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Realizar una instalación de actualización de Windows Server 2019 en el servidor mediante la ejecución de **setup.exe** y utilizando la opción "Mantener las aplicaciones y archivos personales". Una vez completada la instalación, el servidor permanece en el clúster y el servicio se inicia automáticamente.

   6. Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019. Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). El número de compilación (vea `ver` comando) debe ser 17763.292 o posterior.

   7. Quitar el servidor de almacenamiento modo de mantenimiento mediante el siguiente comando de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Reanudar el servidor mediante el siguiente comando de PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Espere para trabajos de reparación de almacenamiento finalizar y para todos los discos volver a un estado correcto. Esto puede tardar bastante tiempo, dependiendo del número de máquinas virtuales en ejecución durante la actualización del servidor. Estos son los comandos para ejecutar:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Actualice el servidor siguiente en el clúster.

5. Después de actualizar todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tener hasta cuatro semanas para hacerlo.

6. Después de actualizar el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento. En este punto, nuevos cmdlets como `Get-ClusterPerf` esté totalmente operativo en cualquier servidor en el clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, actualizar los niveles de configuración de máquina virtual deteniendo cada máquina virtual, mediante el `Update-VMVersion` cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

8. Si está utilizando redes definidas por Software con el conjunto de conmutadores y deshabilitado las comprobaciones de migración en vivo de máquinas virtuales tal como se indica a anterior, use el siguiente cmdlet para volver a habilitar comprobaciones de la máquina virtual en vivo:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Compruebe que el clúster actualizado funciona según lo previsto. Roles deben conmutar por error correctamente y si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar correctamente en vivo.

10. Validar el clúster mediante la ejecución de validación de clúster (`Test-Cluster`) y examinar el informe de validación de clúster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Realizar una instalación limpia del sistema operativo mientras se ejecutan las máquinas virtuales

Esta opción no incurre en ningún tiempo de inactividad de la máquina virtual, pero deberá esperar los trabajos de almacenamiento (reparación de espejo) completar después de actualizar cada servidor. Aunque los servidores individuales se reiniciará de forma secuencial durante el proceso de actualización, el resto de servidores en el clúster, así como todas las máquinas virtuales, seguirán ejecutándose.

1. Compruebe que todos los servidores del clúster se ejecutan las actualizaciones más recientes. Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Como mínimo, instale Microsoft Knowledge Base [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de febrero de 2019). El número de compilación (vea `ver` comando) debe ser 14393.2828 o posterior.

2. Si usa redes definidas por Software con el conjunto de modificadores, abra una sesión de PowerShell con privilegios elevados y ejecute el siguiente comando para deshabilitar las comprobaciones de la migración en vivo de máquinas virtuales en todas las máquinas virtuales en el clúster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   1. Utilice migración en vivo de máquinas virtuales de Hyper-V para mover máquinas virtuales en ejecución fuera del servidor que va a actualizar.

   2. Pausar el servidor de clúster mediante el siguiente comando de PowerShell, tenga en cuenta que algunos grupos internos están ocultos. Se recomienda este paso ir con cuidado, si ya no vive migrar máquinas virtuales fuera del servidor de este cmdlet hará lo que, por lo que podría omitir el paso anterior si lo prefiere.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Colocar el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Ejecute el siguiente cmdlet para comprobar que la **OperationalStatus** valor es **en modo de mantenimiento**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Expulsar al servidor del clúster, ejecute el comando de PowerShell siguiente:  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Realizar una instalación limpia de Windows Server 2019 en el servidor: formato de la unidad del sistema, ejecute **setup.exe** y use "Nothing" opción. Tendrá que configurar la identidad del servidor, roles, características y las aplicaciones una vez completada la instalación y reiniciado el servidor.

   5. Instale el rol Hyper-V y la característica de agrupación en clústeres de conmutación por error en el servidor (puede usar el `Install-WindowsFeature` cmdlet).

   6. Instalar a los controladores de almacenamiento y redes más recientes para el hardware que están aprobados por el fabricante del servidor para su uso con espacios de almacenamiento directo.

   7. Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019. Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). El número de compilación (vea `ver` comando) debe ser 17763.292 o posterior.

   8. Volver a unir el servidor al clúster mediante el comando de PowerShell siguiente:

       ```PowerShell
       Add-ClusterNode
       ```

   9. Quitar el servidor de almacenamiento modo de mantenimiento mediante los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Espere para trabajos de reparación de almacenamiento finalizar y para todos los discos volver a un estado correcto. Esto puede tardar bastante tiempo, dependiendo del número de máquinas virtuales en ejecución durante la actualización del servidor. Estos son los comandos para ejecutar:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Actualice el servidor siguiente en el clúster.

5. Después de actualizar todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tener hasta cuatro semanas para hacerlo.

6. Después de actualizar el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento. En este punto, nuevos cmdlets como `Get-ClusterPerf` esté totalmente operativo en cualquier servidor en el clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, actualizar los niveles de configuración de máquina virtual deteniendo cada máquina virtual y el uso de la `Update-VMVersion` cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

8. Si está utilizando redes definidas por Software con el conjunto de conmutadores y deshabilitado las comprobaciones de migración en vivo de máquinas virtuales tal como se indica a anterior, use el siguiente cmdlet para volver a habilitar comprobaciones de la máquina virtual en vivo:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Compruebe que el clúster actualizado funciona según lo previsto. Roles deben conmutar por error correctamente y si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar correctamente en vivo.

10. Validar el clúster mediante la ejecución de validación de clúster (`Test-Cluster`) y examinar el informe de validación de clúster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Realiza una actualización en contexto mientras se detienen las máquinas virtuales

Esta opción incurre en tiempo de inactividad de máquina virtual, pero puede tardar menos tiempo que si mantiene las máquinas virtuales en ejecución durante la actualización porque no tiene que esperar para que trabajos de almacenamiento (reparación de espejo) completar después de actualizar cada servidor. Aunque los servidores individuales se reiniciará de forma secuencial durante el proceso de actualización, los demás servidores del clúster seguir ejecutándose.

1. Compruebe que todos los servidores del clúster se ejecutan las actualizaciones más recientes. Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Como mínimo, instale Microsoft Knowledge Base [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de febrero de 2019). El número de compilación (vea `ver` comando) debe ser 14393.2828 o posterior.

2. Detener las máquinas virtuales que se ejecutan en el clúster.

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   1. Pausar el servidor de clúster mediante el siguiente comando de PowerShell, tenga en cuenta que algunos grupos internos están ocultos. Se recomienda este paso ir con cuidado.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Colocar el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Ejecute el siguiente cmdlet para comprobar que la **OperationalStatus** valor es **en modo de mantenimiento**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Realizar una instalación de actualización de Windows Server 2019 en el servidor mediante la ejecución de **setup.exe** y utilizando la opción "Mantener las aplicaciones y archivos personales".  
   Una vez completada la instalación, el servidor permanece en el clúster y el servicio se inicia automáticamente.

   5.  Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019.  
   Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   El número de compilación (vea `ver` comando) debe ser 17763.292 o posterior.

   6.  Quitar el servidor de almacenamiento modo de mantenimiento mediante los siguientes comandos de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Reanudar el servidor mediante el siguiente comando de PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Espere para trabajos de reparación de almacenamiento finalizar y para todos los discos volver a un estado correcto.  
   Esto debería ser relativamente rápido, ya que no se están ejecutando las máquinas virtuales. Estos son los comandos para ejecutar:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Actualice el servidor siguiente en el clúster.
5. Después de actualizar todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tener hasta cuatro semanas para hacerlo.

6. Después de actualizar el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento.  
   En este punto, nuevos cmdlets como `Get-ClusterPerf` esté totalmente operativo en cualquier servidor en el clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Inicie las máquinas virtuales en el clúster y compruebe que está funcionando correctamente.

8. Opcionalmente, actualizar los niveles de configuración de máquina virtual deteniendo cada máquina virtual y el uso de la `Update-VMVersion` cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

9. Compruebe que el clúster actualizado funciona según lo previsto.  
   Roles deben conmutar por error correctamente y si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar correctamente en vivo.

10. Validar el clúster mediante la ejecución de validación de clúster (`Test-Cluster`) y examinar el informe de validación de clúster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Realizar una instalación limpia del sistema operativo mientras se detienen las máquinas virtuales

Esta opción incurre en tiempo de inactividad de máquina virtual, pero puede tardar menos tiempo que si mantiene las máquinas virtuales en ejecución durante la actualización porque no tiene que esperar para que trabajos de almacenamiento (reparación de espejo) completar después de actualizar cada servidor. Aunque los servidores individuales se reiniciará de forma secuencial durante el proceso de actualización, los demás servidores del clúster seguir ejecutándose.

1. Compruebe que todos los servidores del clúster se ejecutan las actualizaciones más recientes.  
   Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   Como mínimo, instale Microsoft Knowledge Base [artículo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de febrero de 2019). El número de compilación (vea `ver` comando) debe ser 14393.2828 o posterior.

2. Detener las máquinas virtuales que se ejecutan en el clúster.

3. Realice los pasos siguientes en un servidor de clúster a la vez:

   2. Pausar el servidor de clúster mediante el siguiente comando de PowerShell, tenga en cuenta que algunos grupos internos están ocultos.  
      Se recomienda este paso ir con cuidado.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Colocar el servidor en modo de mantenimiento de almacenamiento mediante la ejecución de los siguientes comandos de PowerShell:

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Ejecute el siguiente cmdlet para comprobar que la **OperationalStatus** valor es **en modo de mantenimiento**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Expulsar al servidor del clúster, ejecute el comando de PowerShell siguiente:  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Realizar una instalación limpia de Windows Server 2019 en el servidor: formato de la unidad del sistema, ejecute **setup.exe** y use "Nothing" opción.  
      Tendrá que configurar la identidad del servidor, roles, características y las aplicaciones una vez completada la instalación y reiniciado el servidor.

   7. Instale el rol Hyper-V y la característica de agrupación en clústeres de conmutación por error en el servidor (puede usar el `Install-WindowsFeature` cmdlet).

   8. Instalar a los controladores de almacenamiento y redes más recientes para el hardware que están aprobados por el fabricante del servidor para su uso con espacios de almacenamiento directo.

   9. Compruebe que el servidor recién actualizado tiene las actualizaciones más recientes de Windows Server 2019.  
      Para obtener más información, consulte [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      El número de compilación (vea `ver` comando) debe ser 17763.292 o posterior.

   10. Volver a unir el servidor al clúster mediante el comando de PowerShell siguiente:

      ```PowerShell
      Add-ClusterNode
      ```

   11. Quitar el servidor de almacenamiento modo de mantenimiento mediante el siguiente comando de PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Espere para trabajos de reparación de almacenamiento finalizar y para todos los discos volver a un estado correcto.  
       Esto puede tardar bastante tiempo, dependiendo del número de máquinas virtuales en ejecución durante la actualización del servidor. Estos son los comandos para ejecutar:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Actualice el servidor siguiente en el clúster.

5. Después de actualizar todos los servidores a Windows Server 2019, use el siguiente cmdlet de PowerShell para actualizar el nivel funcional del clúster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Se recomienda actualizar el nivel funcional del clúster tan pronto como sea posible, aunque técnicamente tener hasta cuatro semanas para hacerlo.

6. Después de actualizar el nivel funcional del clúster, use el siguiente cmdlet para actualizar el grupo de almacenamiento.  
   En este punto, nuevos cmdlets como `Get-ClusterPerf` esté totalmente operativo en cualquier servidor en el clúster.

   ```PowerShell
   Update-StoragePool
   ```

7. Inicie las máquinas virtuales en el clúster y compruebe que está funcionando correctamente.

8. Opcionalmente, actualizar los niveles de configuración de máquina virtual deteniendo cada máquina virtual y el uso de la `Update-VMVersion` cmdlet y, a continuación, vuelva a iniciar las máquinas virtuales.

9. Compruebe que el clúster actualizado funciona según lo previsto.  
   Roles deben conmutar por error correctamente y si se usa la migración en vivo de máquinas virtuales en el clúster, las máquinas virtuales deben migrar correctamente en vivo.

10. Validar el clúster mediante la ejecución de validación de clúster (`Test-Cluster`) y examinar el informe de validación de clúster.
