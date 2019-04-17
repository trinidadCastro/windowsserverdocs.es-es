---
title: "Actualización del sistema operativo de giro de clúster"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2017
ms.openlocfilehash: 8463c163294a4d2223a74b7cfeaea6ac5ae4fcfe
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Actualización del sistema operativo sucesiva de clúster

> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Actualización sucesiva de clúster OS permite a un administrador actualizar el sistema operativo de los nodos del clúster sin detener la Hyper-V o las cargas de trabajo de servidor de archivos de escalado. Con esta característica, pueden evitarse las sanciones contra los acuerdos de nivel de servicio (SLA) de tiempo de inactividad.

Actualización de giro del clúster sistema operativo ofrece las siguientes ventajas:

- Se pueden actualizar clústeres de conmutación por error que ejecuta máquinas virtuales de Hyper-V y las cargas de trabajo de servidor de archivos de escalado (SOFS) de Windows Server 2012 R2 (que se ejecuta en todos los nodos del clúster) a Windows Server 2016 (que se ejecuta en todos los nodos del clúster del clúster) sin tiempo de inactividad. Otras cargas de trabajo de clúster, como SQL Server, estarán disponibles durante el tiempo (normalmente menos de cinco minutos) que se tarda en la conmutación por error a Windows Server 2016.  
- No requiere hardware adicional. Aunque puedes agregar más nodos del clúster clústeres pequeño para mejorar la disponibilidad del clúster durante la actualización de giro de sistema operativo de clúster procesan temporalmente.  
- No necesita el clúster se detiene o se reinicia.  
- No es necesario un clúster nuevo. Se actualiza el clúster existente. Además, se usan los objetos existentes de clúster almacenados en Active Directory.  
- El proceso de actualización es reversible hasta que la selecciona la "punto-de-no-devolución", cuando todos los nodos del clúster de cliente que ejecutan Windows Server 2016, y cuando se ejecuta el cmdlet de PowerShell ClusterFunctionalLevel de actualización.  
- El clúster puede admitir operaciones de aplicación de revisión y mantenimiento mientras se ejecuta en el modo de varios sistemas operativos.  
- Admite automatización mediante PowerShell y WMI.  
- La propiedad pública de clúster **ClusterFunctionalLevel** propiedad indica el estado del clúster en Windows Server 2016 nodos del clúster. Esta propiedad puede consultarse mediante el cmdlet de PowerShell de un nodo del clúster de Windows Server 2016 que pertenece a un clúster de conmutación por error:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Un valor de **8** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2012 R2. Un valor de **9** indica que se está ejecutando en el nivel funcional de Windows Server 2016 al clúster.  

Esta guía describe las distintas etapas del proceso de actualización de giro del clúster sistema operativo, los pasos de instalación, las limitaciones de características y las preguntas más frecuentes (P+f) y se aplica a los siguientes escenarios de clúster de giro actualización del sistema operativo en Windows Server 2016:  
- Clústeres de Hyper-V  
- Clústeres de servidor de archivos de escalado  

El siguiente escenario no se admite en Windows Server 2016:  
-  Actualización de sistema operativo sucesiva de clústeres de invitado con el disco duro virtual (archivo .vhdx) como almacenamiento compartido de clúster  

Actualización de giro del clúster sistema operativo es totalmente compatible mediante System Center Virtual Machine Manager (SCVMM) 2016. Si estás usando SCVMM 2016, consulta [clústeres de actualización de Windows Server 2012 R2 a Windows Server 2016 en VMM](https://technet.microsoft.com/library/mt445417.aspx) para obtener instrucciones sobre actualizar los clústeres y automatizar los pasos que se describen en este documento.  

## <a name="requirements"></a>Requisitos  
Antes de comenzar el proceso de actualización sucesiva de clúster OS, completa los siguientes requisitos:

- Empieza con un clúster de conmutación por error que ejecutan Windows Server (Channel anual comas), Windows Server 2016 o Windows Server 2012 R2.
- Actualizar un clúster de espacios de almacenamiento directa a Windows Server, versión 1709 no se admite.
- Si la carga de trabajo de clúster es máquinas virtuales de Hyper-V o servidor de archivos de escalado, puedes esperar actualización sin interrupción.
- Comprueba que los nodos de Hyper-V tienen CPU que admiten la tabla de direcciones de segundo nivel (SLAT) mediante uno de los métodos siguientes;  
        ¿: Revisa el [estás SLAT Compatible? WP8 SDK sugerencia 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) artículo que describe los dos métodos para comprobar si una CPU admite listones  
        -Descargar el [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) herramienta para determinar si una CPU admite SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Estados de transición de clúster durante la actualización sucesiva de clúster de sistema operativo

En esta sección se describe los diferentes Estados de transición del clúster de Windows Server 2012 R2 que se está actualizando a Windows Server 2016 con la actualización sucesiva de clúster de sistema operativo.  

Para mantener las cargas de trabajo de clúster que se ejecutan durante el proceso de actualización sucesiva de clúster OS, cambio de una carga de trabajo de clúster de un nodo de Windows Server 2012 R2 a Windows Server 2016 nodo actúa como si ambos nodos estaban ejecutando el sistema operativo de Windows Server 2012 R2. Cuando se agregan los nodos de Windows Server 2016 al clúster, trabajan en un modo de compatibilidad de Windows Server 2012 R2. Un nuevo modo de clúster conceptual, denominado "Modo de varios sistemas operativos," permite nodos de diferentes versiones de existir en el mismo clúster (consulta la figura 1).  

![Ilustración que muestra las tres fases de una actualización sucesiva de clúster SO: todos los nodos de Windows Server 2012 R2, el modo de varios sistemas operativos y todos los nodos de Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: Las transiciones de estado del sistema operativo de clúster**  

Un clúster de Windows Server 2012 R2 entra en modo mixto-OS cuando se agrega un nodo de Windows Server 2016 al clúster. El proceso es totalmente reversible: se pueden quitar Windows Server 2016 nodos del clúster y nodos de Windows Server 2012 R2 pueden agregarse al clúster en este modo. El "punto sin retorno" se produce cuando se ejecuta el cmdlet de PowerShell de actualización ClusterFunctionalLevel en el clúster. Para este cmdlet correctamente, todos los nodos deben ser Windows Server 2016, y todos los nodos deben estar en línea.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Estados de transición de un clúster de cuatro nodos al realizar la actualización del sistema operativo de giro

Esta sección se muestra y describe las cuatro diferentes fases de un clúster con almacenamiento compartido cuyos nodos actualizados de Windows Server 2012 R2 a Windows Server 2016.  

"Escenario 1" es el estado inicial, empezamos con un clúster de Windows Server 2012 R2.  

![Ilustración que muestra el estado inicial: todos los nodos de Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: Inicial estado: clúster de conmutación por error de Windows Server 2012 R2 (fase 1)**  

En "fase 2", dos nodos tienen ha pausado, descargados, expulsar, vuelve a formatear e instalado con Windows Server 2016.  

![Ilustración que muestra el clúster en modo de varios sistemas operativos: del clúster de 4 nodos de ejemplo, dos nodos ejecutan Windows Server 2016 y dos nodos ejecutan Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: Intermedio de estado: modo de varios sistemas operativos: Windows Server 2012 R2 y Windows Server 2016 conmutación por error de clúster (fase 2)**  

En "la primera fase 3", todos los nodos del clúster se han actualizado a Windows Server 2016 y está listo para actualizarse con el cmdlet de PowerShell de actualización ClusterFunctionalLevel el clúster.  

> [!NOTE]  
> En esta etapa, es posible completamente el proceso y nodos de Windows Server 2012 R2 pueden agregarse a este clúster.  

![Ilustración que muestra que se ha actualizado completamente a Windows Server 2016 el clúster y está listo para el cmdlet Update-ClusterFunctionalLevel para que aparezca el nivel funcional del clúster hasta Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: Intermedio de estado: todos los nodos de actualizar a Windows Server 2016, listo para la actualización-ClusterFunctionalLevel (fase 3)**  

Después de ejecuta el ClusterFunctionalLevelcmdlet de actualización, el clúster entra en "Escenario 4", donde se puedan usar las nuevas características de clúster de Windows Server 2016.  

![Ilustración que muestra que se ha completado correctamente el clúster sucesivas actualizaciones del sistema operativo; todos los nodos se han actualizado a Windows Server 2016 y el clúster se está ejecutando en el nivel funcional del clúster de Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: Estado de Final de: clúster de conmutación por error de Windows Server 2016 (fase de 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Proceso de actualización de giro de SO de clúster

Esta sección describe el flujo de trabajo para realizar la actualización sucesiva de clúster de sistema operativo.  

![Ilustración que muestra el flujo de trabajo para actualizar un clúster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: Sistema operativo de clúster flujo de trabajo del proceso de actualización de giro**  

Actualización de sistema operativo sucesiva de clúster incluye los siguientes pasos:  

1. Preparar el clúster para la actualización del sistema operativo como sigue:  
    1. Actualización sucesiva de clúster OS requiere quitar un nodo del clúster a la vez. Comprueba si tiene capacidad suficiente en el clúster para mantener HA SLA cuando se quita uno de los nodos del clúster del clúster para una actualización del sistema operativo. ¿En otras palabras, necesita la funcionalidad de las cargas de trabajo de conmutación por error a otro nodo cuando se quita un nodo del clúster durante el proceso de actualización sucesiva de clúster de sistema operativo? ¿El clúster tiene la capacidad de ejecutarse las cargas de trabajo necesarios cuando se quita un nodo del clúster de clúster de giro actualización del sistema operativo?  
    2. Para cargas de trabajo de Hyper-V, comprueba que todos los hosts de Windows Server 2016 Hyper-V tienen CPU admiten la tabla de direcciones de segundo nivel (SLAT). Solo los equipos compatibles con SLAT pueden utilizar el rol de Hyper-V en Windows Server 2016.  
    3. Comprueba que las copias de seguridad de la carga de trabajo completado y considera la posibilidad de copia de seguridad del clúster. Detener las operaciones de copia de seguridad al agregar nodos al clúster.  
    4. Comprueba que todos los nodos del clúster estén en línea o ejecutar o arriba con el [ `Get-ClusterNode` ](https://technet.microsoft.com/library/ee460990.aspx) cmdlet (consulta la figura 7).  

        ![ScreenCap que muestra los resultados de ejecutar el cmdlet Get-nodoClúster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: Determinar el estado del nodo con el cmdlet Get-nodoClúster**  

    5. Si estás ejecutando las actualizaciones de reconocimiento de clúster (PRU), comprueba si se está ejecutando actualmente PRU mediante el uso de la **conscientes de clúster actualizar** interfaz de usuario, o la [ `Get-CauRun` ](https://technet.microsoft.com/library/hh847230.aspx) cmdlet (consulta la figura 8). Dejar de usar PRU la [ `Disable-CauClusterRole` ](https://technet.microsoft.com/library/hh847219.aspx) cmdlet (consulta la figura 9) para evitar que los nodos pausas y agotado por PRU durante el proceso de actualización de giro del clúster sistema operativo.  

        ![ScreenCap que muestra el resultado del cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: Mediante la [ `Get-CauRun` ](https://technet.microsoft.com/library/hh847230.aspx) cmdlet para determinar si las actualizaciones de reconocimiento de clúster se ejecuta en el clúster**  

        ![ScreenCap que muestra la salida del cmdlet deshabilitar CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: Deshabilitar el rol de actualizaciones de reconocimiento de clúster usando la [ `Disable-CauClusterRole` ](https://technet.microsoft.com/library/hh847219.aspx) cmdlet**  

2. Para cada nodo del clúster, completa las siguientes acciones:  
    1. Con el Administrador de clústeres de UI, seleccione un nodo y usar el **pausa | Consumir** opción de menú para consumir el nodo (consulta la figura 10) o usar la [ `Suspend-ClusterNode` ](https://technet.microsoft.com/library/ee461051.aspx) cmdlet (consulta la figura 11).  

        ![ScreenCap que muestra cómo consumir funciones con el Administrador de clústeres de UI](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: Agotamiento funciones desde un nodo mediante el Administrador de clúster de conmutación por error**  

        ![ScreenCap que muestra la salida del cmdlet nodoClúster de suspensión](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: Purga roles desde un nodo mediante la [ `Suspend-ClusterNode` ](https://technet.microsoft.com/library/ee461051.aspx) cmdlet**  

    2.  Con el Administrador de clústeres de UI, **expulsar** del nodo del clúster o uso pausado la [ `Remove-ClusterNode` ](https://technet.microsoft.com/library/ee461001.aspx) cmdlet.  

        ![ScreenCap que muestra la salida del cmdlet Remove-nodoClúster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: Quitar un nodo del clúster mediante [ `Remove-ClusterNode` ](https://technet.microsoft.com/library/ee461001.aspx) cmdlet**  

    3.  Volver a formatear la unidad del sistema y realizar una instalación de limpia del sistema operativo de"" de Windows Server 2016 en el nodo mediante la **personalizada: instalar Windows solo (avanzado)** opción de instalación (consulta la figura 13) en setup.exe. Evita seleccionar la **actualizar: instalar Windows y mantener archivos, configuración y aplicaciones** opción dado que la actualización sucesiva de clúster de sistema operativo no fomentar la actualización en contexto.  

        ![ScreenCap del Asistente de instalación de Windows Server 2016 que muestra la opción de instalación personalizada seleccionada](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: Opciones de instalación disponibles para Windows Server 2016**  

    4.  Agregar el nodo al dominio de Active Directory apropiado.  
    5.  Agregar los usuarios adecuados al grupo de administradores.  
    6.  Utiliza el cmdlet WindowsFeature instalar PowerShell o la UI de administrador del servidor, instala todos los roles de servidor que necesitas, como Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Mediante el cmdlet WindowsFeature instalar PowerShell o la UI de administrador del servidor, instala la característica de clústeres de conmutación por error.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Instala las características adicionales necesarias para las cargas de trabajo de clúster.  
    9. Comprueba la configuración de conectividad de red y almacenamiento usando la UI del Administrador de clúster de conmutación por error.  
    10. Si se usa Firewall de Windows, comprueba que la configuración del Firewall es correcta para el clúster. Por ejemplo, los clústeres de clúster con reconocimiento de actualización (PRU) habilitado pueden requerir una configuración de Firewall.  
    11. Para las cargas de trabajo de Hyper-V, usar la interfaz de usuario del Administrador de Hyper-V para iniciar el cuadro de diálogo Administrador de interruptor Virtual (consulta la figura 14).  

        Comprueba que el nombre de los switches Virtual usa son idénticas para todos los nodos de host de Hyper-V del clúster.  

        ![ScreenCap que muestra la ubicación del cuadro de diálogo Administrador de Hyper-V Virtual conmutador](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: Virtual Switch Manager**  

    12. En un nodo de Windows Server 2016 (no uses un nodo de Windows Server 2012 R2), usa el Administrador de clúster de conmutación por error (consulta la figura 15) para conectarse al clúster.  

        ![ScreenCap que muestra el cuadro de diálogo de selección de clúster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: Agregar un nodo al clúster mediante el Administrador de clúster de conmutación por error**  

    13. Use la UI de administrador de clúster de conmutación por error o la [ `Add-ClusterNode` ](https://technet.microsoft.com/library/ee461047.aspx) cmdlet (consulta la figura 16) para agregar el nodo al clúster.  

        ![ScreenCap que muestra la salida del cmdlet Add-nodoClúster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: Agregar un nodo al clúster mediante [ `Add-ClusterNode` ](https://technet.microsoft.com/library/ee461047.aspx) cmdlet**  

        > [!NOTE]  
        > Cuando el primer nodo de Windows Server 2016 une al clúster, el clúster entra en modo "mixto SO" y los recursos principales de clúster se mueven al nodo de Windows Server 2016. Un clúster de modo "mixto SO" es un totalmente funcional donde los nuevos nodos se ejecutan en un modo de compatibilidad con los nodos antiguos. Modo de "Varios sistemas operativos" es un modo transitorio para el clúster. No pretende ser permanente y se espera que los clientes a actualizar todos los nodos del clúster de su en cuatro semanas.  

    14. Después de la Windows Server 2016 se correctamente agrega nodo del clúster, puedes (opcionalmente) mover parte de la carga de clúster al nodo recién agregado para equilibrar la carga de trabajo en el clúster como sigue:

        ![ScreenCap que muestra la salida del cmdlet ClusterVirtualMachineRole de movimiento](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: Mover una carga de trabajo (rol de máquina virtual de clúster) de clúster utilizando [ `Move-ClusterVirtualMachineRole` ](https://technet.microsoft.com/library/ee461041.aspx) cmdlet**  

        1. Usa **migración en vivo** desde el Administrador de clúster de conmutación por error para las máquinas virtuales o [ `Move-ClusterVirtualMachineRole` ](https://technet.microsoft.com/library/ee461041.aspx) cmdlet (consulta la figura 17) para llevar a cabo una migración en vivo de las máquinas virtuales.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Usa **mover** desde el Administrador de clúster de conmutación por error o la [ `Move-ClusterGroup` ](https://technet.microsoft.com/library/ee461002.aspx) cmdlet para otras cargas de trabajo de clúster.  

3. Cuando se ha actualizado a Windows Server 2016 todos los nodos y se ha agregado al clúster, o cuando se ha expulsa los nodos de Windows Server 2012 R2 restantes, haz lo siguiente:  

    > [!IMPORTANT]  
    > -   Después de actualizar el nivel funcional del clúster, no se puede volver al nivel funcional de Windows Server 2012 R2 y Windows Server 2012 R2 nodos no se puede agregar al clúster.
    > -   Hasta que la [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet se ejecuta, el proceso es completamente reversible y nodos de Windows Server 2012 R2 pueden agregarse a este clúster y pueden eliminarse nodos de Windows Server 2016.  
    > -   Después de la [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet se ejecuta, nuevas características estarán disponibles.  

    1.  Usando la UI del Administrador de clúster de conmutación por error o la [ `Get-ClusterGroup` ](https://technet.microsoft.com/library/ee461017.aspx) cmdlet, comprueba que todos los roles de clúster se ejecutan en el clúster según lo esperado. En el siguiente ejemplo, no se utiliza almacenamiento disponible, en su lugar se usa CSV, por lo tanto, almacenamiento disponible muestra una **sin conexión** estado (consulta la figura 18).  

        ![ScreenCap que muestra el resultado del cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: Comprobar que todos los grupos (clúster funciones) del clúster están ejecutando mediante el [ `Get-ClusterGroup` ](https://technet.microsoft.com/library/ee461017.aspx) cmdlet**  

    2.  Comprueba que todos los nodos del clúster estén en línea y se ejecuta mediante el [ `Get-ClusterNode` ](https://technet.microsoft.com/library/ee460990.aspx) cmdlet.  
    3.  Ejecutar el [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet - errores no se deben devolver (consulta la figura 19).  

        ![ScreenCap que muestra la salida del cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: Actualizar el nivel funcional de un clúster mediante PowerShell**  

    4.  Después de la [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet se ejecuta, nuevas características están disponibles.  

4. Windows Server 2016 - reanudar normal del clúster actualizaciones y las copias de seguridad:  

    1. Si tenías instalado previamente PRU, reinícielo por medio de la UI CAU o usar la [ `Enable-CauClusterRole` ](https://technet.microsoft.com/library/hh847229.aspx) cmdlet (consulta la figura 20).  

        ![ScreenCap que muestra el resultado de la CauClusterRole habilitar](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: Activar las actualizaciones con reconocimiento de clúster rol usando la [ `Enable-CauClusterRole` ](https://technet.microsoft.com/library/hh847229.aspx) cmdlet**  

    2. Reanudar las operaciones de copia de seguridad.  

5. Habilitar y usar las características de Windows Server 2016 en máquinas virtuales de Hyper-V.  

    1. Después de que se ha actualizado el clúster a nivel funcional de Windows Server 2016, muchas cargas de trabajo como máquinas virtuales de Hyper-V tiene nuevas funcionalidades. Para obtener una lista de nuevas capacidades de Hyper-V. consulta [de migración y actualización máquinas virtuales](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. En cada nodo de host Hyper-V del clúster, utilice el [ `Get-VMHostSupportedVersion` ](https://technet.microsoft.com/library/mt653838.aspx) cmdlet para ver las versiones de la configuración de máquina virtual de Hyper-V que son compatibles con el host.  

        ![ScreenCap que muestra el resultado del cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: Ver las versiones de configuración de la máquina virtual de Hyper-V admitidas por el host**  

   3.  En el nodo del clúster de cada host Hyper-V, versiones de la configuración de máquina virtual de Hyper-V pueden actualizarse mediante programación una ventana de mantenimiento breve con los usuarios, realizar copias de seguridad, desactivar máquinas virtuales y ejecutar la [ `Update-VMVersion` ](https://technet.microsoft.com/library/mt484146.aspx) cmdlet (consulta la figura 22). Esto actualizará la versión de la máquina virtual y habilitar nuevas características de Hyper-V, eliminando la necesidad de futuras actualizaciones de componentes de integración de Hyper-V (CI). Este cmdlet se puede ejecutar desde el nodo de Hyper-V que hospeda la máquina virtual, o la `-ComputerName` parámetro se puede usar para actualizar la versión de la máquina virtual de forma remota. En este ejemplo, aquí te actualizar la versión de la configuración de VM1 de 5.0 a 7.0 aprovechar muchas características nuevas de Hyper-V asociadas con esta versión de la configuración de máquina virtual como puntos de control de producción (copias de seguridad de aplicación coherente) y archivos binarios de configuración de máquina virtual.  

        ![ScreenCap que muestra el cmdlet Update-VMVersion en acción](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **Figura 22: Actualización de una versión de máquina virtual con el cmdlet de PowerShell de actualización VMVersion**  

4.  Grupos de almacenamiento se pueden actualizar mediante la [actualización StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/update-storagepool) cmdlet de PowerShell: se trata de una operación en línea.  

Aunque nos estamos destinadas a escenarios de nube privada, específicamente Hyper-V y los clústeres de servidor de archivos de escalado, que pueden actualizarse sin tiempo de inactividad, el proceso de actualización de giro del clúster sistema operativo pueden usarse para cualquier rol de clúster.  

## <a name="restrictions--limitations"></a>Restricciones y limitaciones  
- Esta característica funciona únicamente para Windows Server 2012 R2 a las versiones Windows Server 2016. Esta característica no puede actualizar las versiones anteriores de Windows Server, como Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 para Windows Server 2016.  
- Cada nodo de Windows Server 2016 deben volver a formatear nueva instalación solo. "En el lugar" o "Actualizar" no se recomienda el tipo de instalación.  
- Un nodo de Windows Server 2016 debe usarse para agregar nodos de Windows Server 2016 al clúster.  
- Al administrar un clúster en modo de varios sistemas operativos, siempre realizar las tareas de administración desde un nodo de nivel superior que se ejecuta Windows Server 2016. Los nodos de nivel inferior Windows Server 2012 R2, no pueden usar herramientas de administración o de interfaz de usuario en Windows Server 2016.  
- Recomendamos a los clientes para desplazarse rápidamente por el proceso de actualización de clúster ya que algunas características de clúster no están optimizadas para el modo de varios sistemas operativos.  
- Evita crear o cambiar el tamaño de almacenamiento en Windows Server 2016 nodos del clúster mientras se está ejecutando en modo de varios sistemas operativos debido a incompatibilidades posibles en la conmutación por error desde un nodo de Windows Server 2016 a los nodos de Windows Server 2012 R2 de nivel inferior.  

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes  
**¿Cuánto puede ejecutar el clúster de conmutación por error en el modo de varios sistemas operativos?**  
    Recomendamos a los clientes para completar la actualización en cuatro semanas. Hay muchas optimizaciones en Windows Server 2016. Hemos actualizado correctamente Hyper-V y clústeres de servidor de archivos de escalado sin interrupción en menos de cuatro horas totales.  

**¿Se migrará esta característica volver a Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008?**  
    No hay planes de esta característica volver a versiones anteriores de puerto. Actualización sucesiva de clúster OS es nuestra visión de actualización de clústeres de Windows Server 2012 R2 para Windows Server 2016 y más allá.  

**¿El clúster de Windows Server 2012 R2 debe tener todas las actualizaciones de software instaladas antes de iniciar el proceso de actualización de giro del clúster sistema operativo?**  
    Sí, antes de iniciar el proceso de actualización sucesiva de clúster OS, comprueba que todos los nodos del clúster se actualiza con las últimas actualizaciones de software.  

**¿Puedo ejecutar el [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet mientras nodos están desactivados o pausan?**  
    No. Todos los nodos del clúster deben estar en y en una suscripción activa para la [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet para trabajar.  

**¿Actualización sucesiva de clúster OS funciona para cualquier carga de trabajo de clúster? ¿Funciona para SQL Server?**  
    Sí, actualización sucesiva de clúster OS funciona para cualquier carga de trabajo de clúster. Sin embargo, es el único sin interrupción para Hyper-V y clústeres de servidor de archivos de escalado. La mayoría de otras cargas de trabajo incurrir en el tiempo de inactividad (normalmente un par de minutos) cuando se conmutación por error y conmutación por error, se requiere al menos una vez durante el proceso de actualización de giro del clúster sistema operativo.  

**¿Automatizar este proceso con PowerShell?**  
    Sí, hemos diseñado clúster OS actualización sucesiva para automatizarse mediante PowerShell.  

**Para un clúster grande que tiene la carga de trabajo adicional y la capacidad de conmutación por error, ¿puedo actualizar varios nodos simultáneamente?**  
    Sí. Cuando se quita un nodo del clúster para actualizar el sistema operativo, el clúster tendrá un nodo de menos de conmutación por error, por lo tanto, tendrá una capacidad reducida de conmutación por error. Clústeres de gran tamaño con suficiente capacidad de conmutación por error y la carga de trabajo, se pueden actualizar simultáneamente varios nodos. Para agregar nodos del clúster temporalmente al clúster para proporcionar mejorada de la carga de trabajo y la capacidad de conmutación por error durante el proceso de actualización de giro del clúster sistema operativo.  

**¿Qué ocurre si se descubre un problema en el clúster después [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) se ha ejecutado correctamente?**  
    Si se ha copia la base de datos del clúster con una copia de seguridad de estado del sistema antes de ejecutar [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx), debe ser capaz de realizar una autorización restaurar en un nodo del clúster de Windows Server 2012 R2 y restaurar la configuración y la base de datos de clúster original.  

**¿Puedo usar la actualización en contexto para cada nodo en lugar de usar la instalación del SO limpiar volviendo a formatear la unidad del sistema?**  
    No recomendamos el uso de la actualización en contexto de Windows Server, pero somos conscientes de que funciona en algunos casos donde se usan los controladores predeterminados. Lee detenidamente todos los mensajes de advertencia que se muestran durante la actualización en contexto de un nodo del clúster.  

**¿Si estoy utilizando replicación de Hyper-V para una máquina virtual de Hyper-V en mi clúster de Hyper-V, replicación permanecerá intacta durante y después del proceso de actualización de giro del clúster sistema operativo?**  
    Sí, réplica de Hyper-V permanece intacta durante y después del proceso de actualización de giro del clúster sistema operativo.  

**¿Puedo usar System Center 2016 Virtual Machine Manager (SCVMM) para automatizar el proceso de actualización de giro del clúster sistema operativo?**  
    Sí, puedes automatizar el proceso de actualización sucesiva de clúster OS con VMM en System Center 2016.  

## <a name="see-also"></a>Consulta también  
-   [Notas: Problemas importantes en Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Novedades en Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Novedades de conmutación por error en Windows Server](whats-new-in-failover-clustering.md)  