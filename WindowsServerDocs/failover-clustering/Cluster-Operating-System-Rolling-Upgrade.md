---
title: Actualización gradual del sistema operativo del clúster
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: fc1799db76f528a599ef70eec5093da0a76206a2
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948536"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Actualización gradual del sistema operativo del clúster

> Se aplica a: Windows Server 2019, Windows Server 2016

La actualización gradual de SO del clúster permite a un administrador actualizar el sistema operativo de los nodos del clúster sin detener las cargas de trabajo de Hyper-V o Servidor de archivos de escalabilidad horizontal. Con esta característica, se pueden evitar las penalizaciones de tiempo de inactividad en los acuerdos de nivel de servicio (SLA).

La actualización gradual de SO del clúster proporciona las siguientes ventajas:

- Los clústeres de conmutación por error que ejecutan las cargas de trabajo de máquina virtual de Hyper-V y servidor de archivos de escalabilidad horizontal (SOFS) se pueden actualizar desde Windows Server 2012 R2 (que se ejecuta en todos los nodos del clúster) a Windows Server 2016 (que se ejecuta en todos los nodos del clúster) sin tiempo de inactividad. Otras cargas de trabajo de clúster, como SQL Server, no estarán disponibles durante el tiempo (normalmente menos de cinco minutos) que se tarda en conmutar por error a Windows Server 2016.  
- No requiere hardware adicional. Aunque puede agregar nodos de clúster adicionales temporalmente a los clústeres pequeños para mejorar la disponibilidad del clúster durante el proceso de actualización gradual del sistema operativo del clúster.  
- No es necesario detener o reiniciar el clúster.  
- No es necesario un nuevo clúster. El clúster existente se actualiza. Además, se usan los objetos de clúster existentes almacenados en Active Directory.  
- El proceso de actualización es reversible hasta que el cliente elija "punto de no devolución", cuando todos los nodos del clúster ejecuten Windows Server 2016 y cuando se ejecute el cmdlet de PowerShell Update-ClusterFunctionalLevel.  
- El clúster puede admitir operaciones de revisión y mantenimiento mientras se ejecuta en el modo de sistema operativo mixto.  
- Admite la automatización a través de PowerShell y WMI.  
- La propiedad **ClusterFunctionalLevel** de la propiedad pública del clúster indica el estado del clúster en los nodos de clúster de Windows Server 2016. Esta propiedad se puede consultar mediante el cmdlet de PowerShell desde un nodo de clúster de Windows Server 2016 que pertenezca a un clúster de conmutación por error:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Un valor de **8** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2012 R2. Un valor de **9** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2016.  

En esta guía se describen las distintas fases del proceso de actualización gradual del sistema operativo del clúster, los pasos de instalación, las limitaciones de las características y las preguntas más frecuentes (p + f), y es aplicable a los siguientes escenarios de actualización gradual del sistema operativo de clúster en Windows Server 2016:  
- Clústeres de Hyper-V  
- Clústeres de Servidor de archivos de escalabilidad horizontal  

El siguiente escenario no se admite en Windows Server 2016:  
-  Actualización gradual del sistema operativo de clúster de los clústeres invitados con el disco duro virtual (archivo. vhdx) como almacenamiento compartido  

La actualización gradual del sistema operativo del clúster es totalmente compatible con System Center Virtual Machine Manager (SCVMM) 2016. Si usa SCVMM 2016, consulte [realizar una actualización gradual de un clúster de hosts de Hyper-V a Windows Server 2016 en VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) para obtener instrucciones sobre cómo actualizar los clústeres y automatizar los pasos que se describen en este documento.  

## <a name="requirements"></a>Requisitos  
Complete los requisitos siguientes antes de comenzar el proceso de actualización gradual del sistema operativo del clúster:

- Comience con un clúster de conmutación por error que ejecute Windows Server (canal semianual), Windows Server 2016 o Windows Server 2012 R2.
- No se admite la actualización de un clúster de Espacios de almacenamiento directo a Windows Server, versión 1709.
- Si la carga de trabajo del clúster son máquinas virtuales de Hyper-V, o Servidor de archivos de escalabilidad horizontal, puede esperar una actualización sin tiempo de inactividad.
- Compruebe que los nodos de Hyper-V tienen CPU que admiten la tabla de direcciones de segundo nivel (SLAT) mediante uno de los métodos siguientes:  
        -Revisar [¿es compatible con slat? ](https://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx)Artículo de la sugerencia 01 del SDK de WP8 que describe dos métodos para comprobar si una CPU admite SLATs  
        -Descargue la herramienta [Coreinfo v 3.31](https://technet.microsoft.com/sysinternals/cc835722) para determinar si una CPU es compatible con slat.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Estados de transición del clúster durante la actualización gradual del sistema operativo del clúster

En esta sección se describen los distintos Estados de transición del clúster de Windows Server 2012 R2 que se está actualizando a Windows Server 2016 mediante la actualización gradual del sistema operativo del clúster.  

Para mantener las cargas de trabajo del clúster en ejecución durante el proceso de actualización gradual del sistema operativo del clúster, el traslado de una carga de trabajo de clúster desde un nodo de Windows Server 2012 R2 al nodo de Windows Server 2016 funciona como si ambos nodos estuvieran ejecutando el sistema operativo Windows Server 2012 R2. Cuando se agregan nodos de Windows Server 2016 al clúster, funcionan en modo de compatibilidad de Windows Server 2012 R2. Un nuevo modo de clúster conceptual, denominado "modo de sistema operativo mixto", permite que existan nodos de versiones diferentes en el mismo clúster (vea la ilustración 1).  

![ilustración que muestra las tres etapas de una actualización gradual del sistema operativo del clúster: todos los nodos Windows Server 2012 R2, modo mixto-SO y todos los nodos Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: transiciones de estado del sistema operativo del clúster**  

Un clúster de Windows Server 2012 R2 entra en modo mixto de sistema operativo cuando se agrega un nodo de Windows Server 2016 al clúster. El proceso es totalmente reversible: los nodos de Windows Server 2016 se pueden quitar del clúster y los nodos de Windows Server 2012 R2 se pueden agregar al clúster en este modo. El "punto de no retorno" se produce cuando se ejecuta el cmdlet de PowerShell Update-ClusterFunctionalLevel en el clúster. Para que este cmdlet se ejecute correctamente, todos los nodos deben ser Windows Server 2016 y todos los nodos deben estar en línea.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Estados de transición de un clúster de cuatro nodos mientras se realiza la actualización gradual del sistema operativo

En esta sección se muestran y describen las cuatro fases diferentes de un clúster con almacenamiento compartido cuyos nodos se actualizan de Windows Server 2012 R2 a Windows Server 2016.  

"Fase 1" es el estado inicial: comenzamos con un clúster de Windows Server 2012 R2.  

![ilustración que muestra el estado inicial: todos los nodos Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: estado inicial: clúster de conmutación por error de Windows Server 2012 R2 (fase 1)**  

En la "fase 2", dos nodos se han pausado, purgado, expulsado, reformateado e instalado con Windows Server 2016.  

![ilustración que muestra el clúster en modo de sistema operativo mixto: fuera del clúster de 4 nodos de ejemplo, dos nodos ejecutan Windows Server 2016 y dos nodos ejecutan Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: estado intermedio: modo mixto-SO: Windows Server 2012 R2 y clúster de conmutación por error de Windows Server 2016 (fase 2)**  

En la "fase 3", todos los nodos del clúster se han actualizado a Windows Server 2016 y el clúster está listo para actualizarse con el cmdlet de PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> En esta fase, el proceso se puede invertir completamente y los nodos de Windows Server 2012 R2 se pueden agregar a este clúster.  

![ilustración que muestra que el clúster se ha actualizado completamente a Windows Server 2016 y está listo para que el cmdlet Update-ClusterFunctionalLevel ponga el nivel funcional del clúster a Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: estado intermedio: todos los nodos actualizados a Windows Server 2016, listos para Update-ClusterFunctionalLevel (fase 3)**  

Después de ejecutar Update-ClusterFunctionalLevelcmdlet, el clúster entra en "Stage 4", donde se pueden usar las nuevas características de clúster de Windows Server 2016.  

![ilustración que muestra que la actualización del sistema operativo gradual del clúster se ha completado correctamente; todos los nodos se han actualizado a Windows Server 2016 y el clúster se está ejecutando en el nivel funcional del clúster de Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: estado final: clúster de conmutación por error de Windows Server 2016 (fase 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Proceso de actualización gradual de SO del clúster

En esta sección se describe el flujo de trabajo para realizar la actualización gradual del sistema operativo del clúster.  

![ilustración que muestra el flujo de trabajo para actualizar un clúster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: flujo de trabajo del proceso de actualización gradual de SO de clúster**  

La actualización gradual del sistema operativo del clúster incluye los pasos siguientes:  

1. Prepare el clúster para la actualización del sistema operativo de la siguiente manera:  
    1. La actualización gradual del sistema operativo del clúster requiere la eliminación de un nodo a la vez del clúster. Compruebe si tiene suficiente capacidad en el clúster para mantener los SLA de alta disponibilidad cuando uno de los nodos del clúster se quita del clúster para una actualización del sistema operativo. En otras palabras, ¿necesita la capacidad de conmutar por error las cargas de trabajo a otro nodo cuando se quita un nodo del clúster durante el proceso de actualización gradual del sistema operativo del clúster? ¿El clúster tiene la capacidad de ejecutar las cargas de trabajo necesarias cuando se quita un nodo del clúster para la actualización gradual del sistema operativo del clúster?  
    2. En el caso de las cargas de trabajo de Hyper-V, compruebe que todos los hosts de Hyper-V de Windows Server 2016 tienen una tabla de direcciones de segundo nivel (SLAT) de compatibilidad con CPU. Solo los equipos compatibles con SLAT pueden usar el rol de Hyper-V en Windows Server 2016.  
    3. Compruebe que las copias de seguridad de la carga de trabajo se han completado y considere la posibilidad de realizar una copia de seguridad del clúster. Detenga las operaciones de copia de seguridad al agregar nodos al clúster.  
    4. Compruebe que todos los nodos del clúster están en línea/Running/up con el cmdlet [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) (consulte la figura 7).  

        ![captura que muestra los resultados de ejecutar el cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: determinación del estado del nodo mediante el cmdlet Get-ClusterNode**  

    5. Si está ejecutando actualizaciones compatibles con clústeres (CAU), compruebe si la CAU se está ejecutando actualmente mediante la interfaz **de usuario de actualización compatible con clústeres** o el cmdlet [`Get-CauRun`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) (consulte la figura 8). Detenga la CAU con el cmdlet [`Disable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) (vea la ilustración 9) para evitar que la Cau detenga los nodos durante el proceso de actualización gradual del sistema operativo del clúster.  

        ![captura que muestra la salida del cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: uso del cmdlet [`Get-CauRun`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) para determinar si las actualizaciones compatibles con clústeres se están ejecutando en el clúster**  

        ![captura que muestra la salida del cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: deshabilitación del rol de actualizaciones compatibles con clústeres mediante el cmdlet [`Disable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps)**  

2. Para cada nodo del clúster, realice lo siguiente:  
    1. Mediante la interfaz de usuario del administrador de clústeres, seleccione un nodo y use la **pausa |** Opción de menú de purga para purgar el nodo (vea la figura 10) o usar el cmdlet [`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) (consulte la figura 11).  

        ![captura que muestra cómo purgar roles con la interfaz de usuario del administrador de clústeres](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: purga de roles de un nodo mediante Administrador de clústeres de conmutación por error**  

        ![captura que muestra la salida del cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: purga de roles de un nodo mediante el cmdlet [`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps)**  

    2.  Mediante la interfaz de usuario del administrador de clústeres, **expulse** el nodo en pausa del clúster o use el cmdlet [`Remove-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) .  

        ![captura que muestra la salida del cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: eliminación de un nodo del clúster mediante el cmdlet [`Remove-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps)**  

    3.  Vuelva a dar formato a la unidad del sistema y realice una "instalación limpia del sistema operativo" de Windows Server 2016 en el nodo mediante la opción **personalizada: instalar solo Windows (** vea la figura 13) de Setup. exe. Evite seleccionar la opción **actualizar: instalar Windows y mantener archivos, opciones de configuración y aplicaciones** porque la actualización gradual del sistema operativo del clúster no fomenta la actualización en contexto.  

        ![captura del Asistente para la instalación de Windows Server 2016 que muestra la opción instalación personalizada seleccionada](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: opciones de instalación disponibles para Windows Server 2016**  

    4.  Agregue el nodo al dominio de Active Directory adecuado.  
    5.  Agregue los usuarios adecuados al grupo de administradores.  
    6.  Con la interfaz de usuario de Administrador del servidor o el cmdlet install-WindowsFeature de PowerShell, instale los roles de servidor que necesite, como Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Instale la característica de clústeres de conmutación por error mediante la interfaz de usuario de Administrador del servidor o el cmdlet install-WindowsFeature de PowerShell.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Instale las características adicionales que necesiten las cargas de trabajo del clúster.  
    9. Compruebe la configuración de conectividad de red y almacenamiento mediante la interfaz de usuario de Administrador de clústeres de conmutación por error.  
    10. Si se usa Firewall de Windows, compruebe que la configuración del firewall sea correcta para el clúster. Por ejemplo, los clústeres habilitados para la actualización compatible con clústeres (CAU) pueden requerir la configuración del firewall.  
    11. En el caso de cargas de trabajo de Hyper-V, use la interfaz de usuario del administrador de Hyper-V para iniciar el cuadro de diálogo Administrador de conmutadores virtuales (vea la figura 14).  

        Compruebe que el nombre de los conmutadores virtuales que se usan son idénticos para todos los nodos de host de Hyper-V del clúster.  

        ![captura que muestra la ubicación del cuadro de diálogo Administrador de conmutadores virtuales de Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: administrador de conmutadores virtuales**  

    12. En un nodo de Windows Server 2016 (no use un nodo de Windows Server 2012 R2), use el Administrador de clústeres de conmutación por error (vea la figura 15) para conectarse al clúster.  

        ![captura que muestra el cuadro de diálogo Seleccionar clúster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: adición de un nodo al clúster mediante Administrador de clústeres de conmutación por error**  

    13. Use la interfaz de usuario de Administrador de clústeres de conmutación por error o el cmdlet [`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) (vea la figura 16) para agregar el nodo al clúster.  

        ![captura que muestra la salida del cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: adición de un nodo al clúster mediante [`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > Cuando el primer nodo de Windows Server 2016 se une al clúster, el clúster entra en modo "Mixed-OS" y los recursos principales del clúster se mueven al nodo Windows Server 2016. Un clúster de modo "Mixed-OS" es un clúster totalmente funcional en el que los nuevos nodos se ejecutan en un modo de compatibilidad con los nodos antiguos. El modo "Mixed-OS" es un modo transitorio para el clúster. No pretende ser permanente y se espera que los clientes actualicen todos los nodos de su clúster en un plazo de cuatro semanas.  

    14. Después de agregar correctamente el nodo de Windows Server 2016 al clúster, puede (opcionalmente) trasladar parte de la carga de trabajo del clúster al nodo recién agregado para volver a equilibrar la carga de trabajo en el clúster de la siguiente manera:

        ![captura que muestra la salida del cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: mover una carga de trabajo de clúster (rol de VM de clúster) mediante [`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet**  

        1. Use **migración en vivo** del administrador de clústeres de conmutación por error de máquinas virtuales o el cmdlet de [`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) (consulte la figura 17) para realizar una migración en vivo de las máquinas virtuales.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Use **Move** desde el administrador de clústeres de conmutación por error o el cmdlet [`Move-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) para otras cargas de trabajo de clúster.  

3. Cuando todos los nodos se hayan actualizado a Windows Server 2016 y se hayan agregado de nuevo al clúster, o cuando se hayan expulsado los nodos de Windows Server 2012 R2 restantes, haga lo siguiente:  

    > [!IMPORTANT]  
    > -   Después de actualizar el nivel funcional del clúster, no puede volver al nivel funcional de Windows Server 2012 R2 y los nodos de Windows Server 2012 R2 no se pueden agregar al clúster.
    > -   Hasta que se ejecute el cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , el proceso es totalmente reversible y se pueden agregar nodos de windows Server 2012 R2 a este clúster y se pueden quitar los nodos de windows Server 2016.  
    > -   Una vez que se ejecuta el cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , estarán disponibles nuevas características.  

    1.  Con la interfaz de usuario de Administrador de clústeres de conmutación por error o el cmdlet [`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) , compruebe que todos los roles de clúster se ejecutan en el clúster según lo esperado. En el ejemplo siguiente, el almacenamiento disponible no se usa, sino que se usa CSV, por lo tanto, el almacenamiento disponible muestra un estado **sin conexión** (consulte la figura 18).  

        ![captura que muestra la salida del cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: comprobación de que todos los grupos de clústeres (roles de clúster) se están ejecutando con el cmdlet [`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps)**  

    2.  Compruebe que todos los nodos del clúster están en línea y se ejecutan con el cmdlet [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) .  
    3.  Ejecutar el cmdlet [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) : no se deben devolver errores (vea la figura 19).  

        ![captura que muestra la salida del cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: actualización del nivel funcional de un clúster mediante PowerShell**  

    4.  Una vez que se ejecuta el cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , están disponibles nuevas características.  

4. Windows Server 2016: reanudar las actualizaciones y copias de seguridad normales del clúster:  

    1. Si previamente estaba ejecutando CAU, reinícielo con la interfaz de usuario de CAU o use el cmdlet [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) (consulte la figura 20).  

        ![captura que muestra la salida de enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: habilitación del rol de actualizaciones compatibles con clústeres mediante el cmdlet [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps)**  

    2. Reanudar las operaciones de copia de seguridad.  

5. Habilitar y usar las características de Windows Server 2016 en Hyper-V Virtual Machines.  

    1. Una vez que el clúster se ha actualizado al nivel funcional de Windows Server 2016, muchas cargas de trabajo, como las máquinas virtuales de Hyper-V, tendrán nuevas funcionalidades. Para obtener una lista de las nuevas funcionalidades de Hyper-V. consulte [migración y actualización de máquinas virtuales](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. En cada nodo de host de Hyper-V del clúster, use el cmdlet [`Get-VMHostSupportedVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) para ver las versiones de configuración de la máquina virtual de Hyper-v admitidas por el host.  

        ![captura que muestra la salida del cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: visualización de las versiones de configuración de la máquina virtual de Hyper-V admitidas por el host**  

   3. En cada nodo de host de Hyper-V del clúster, las versiones de configuración de máquina virtual de Hyper-V se pueden actualizar mediante la programación de una breve ventana de mantenimiento con usuarios, la copia de seguridad, la desactivación de máquinas virtuales y la ejecución del cmdlet [`Update-VMVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) (consulte la figura 22). Esto actualizará la versión de la máquina virtual y habilitará las nuevas características de Hyper-V, lo que elimina la necesidad de futuras actualizaciones de componentes de integración de Hyper-V (CI). Este cmdlet se puede ejecutar desde el nodo de Hyper-V que hospeda la máquina virtual, o bien se puede usar el parámetro `-ComputerName` para actualizar la versión de la máquina virtual de forma remota. En este ejemplo, se actualiza la versión de configuración de VM1 de 5,0 a 7,0 para aprovechar muchas de las nuevas características de Hyper-V asociadas a esta versión de configuración de máquina virtual, como puntos de control de producción (copias de seguridad coherentes con la aplicación) y máquina virtual binaria. archivo de configuración.  

       ![captura que muestra el cmdlet Update-VMVersion en acción](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
       **Figura 22: actualización de una versión de máquina virtual mediante el cmdlet de PowerShell Update-VMVersion**  

6. Los grupos de almacenamiento se pueden actualizar mediante el cmdlet de PowerShell [Update-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) , que es una operación en línea.  

Aunque nos centramos en escenarios de nube privada, específicamente Hyper-V y clústeres de servidores de archivos de escalabilidad horizontal, que se pueden actualizar sin tiempo de inactividad, el proceso de actualización gradual del sistema operativo del clúster se puede usar para cualquier rol de clúster.  

## <a name="restrictions--limitations"></a>Restricciones y limitaciones  
- Esta característica solo funciona para Windows Server 2012 R2 a las versiones 2016 de Windows Server. Esta característica no puede actualizar versiones anteriores de Windows Server, como Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2016.  
- Solo se debe cambiar el formato o la nueva instalación de cada nodo de Windows Server 2016. No se recomienda el tipo de instalación "en contexto" o "actualizar".  
- Se debe usar un nodo de Windows Server 2016 para agregar nodos de Windows Server 2016 al clúster.  
- Al administrar un clúster de modo de sistema operativo mixto, realice siempre las tareas de administración desde un nodo de nivel superior que ejecute Windows Server 2016. Los nodos de nivel inferior de Windows Server 2012 R2 no pueden usar la interfaz de usuario o las herramientas de administración en Windows Server 2016.  
- Animamos a los clientes a pasar por el proceso de actualización del clúster rápidamente porque algunas características del clúster no están optimizadas para el modo mixto-OS.  
- Evite crear o cambiar el tamaño del almacenamiento en los nodos de Windows Server 2016 mientras el clúster se ejecuta en modo de sistema operativo mixto debido a posibles incompatibilidades en la conmutación por error de un nodo de Windows Server 2016 a nodos de nivel inferior de Windows Server 2012 R2.  

## <a name="frequently-asked-questions"></a>Preguntas frecuentes  
**¿Cuánto tiempo puede ejecutarse el clúster de conmutación por error en modo de sistema operativo mixto?**  
    Animamos a los clientes a completar la actualización en un plazo de cuatro semanas. Hay muchas optimizaciones en Windows Server 2016. Hemos actualizado correctamente los clústeres de servidores de archivos de escalabilidad horizontal y Hyper-V sin tiempo de inactividad en menos de cuatro horas en total.  

**¿Trasladará esta característica de nuevo a Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008?**  
    No tenemos ningún plan para volver a migrar esta característica a las versiones anteriores. La actualización gradual del sistema operativo de clúster es nuestra visión de actualizar clústeres de Windows Server 2012 R2 a Windows Server 2016 y versiones posteriores.  

**¿Es necesario que el clúster de Windows Server 2012 R2 tenga todas las actualizaciones de software instaladas antes de iniciar el proceso de actualización gradual del sistema operativo del clúster?**  
    Sí, antes de iniciar el proceso de actualización gradual del sistema operativo del clúster, compruebe que todos los nodos del clúster estén actualizados con las actualizaciones de software más recientes.  

**¿Puedo ejecutar el cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) mientras los nodos están desactivados o en pausa?**  
    No. Todos los nodos del clúster deben estar encendidos y en pertenencia activa para que el cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) funcione.  

**¿Funciona la actualización gradual del sistema operativo de clúster para cualquier carga de trabajo del clúster? ¿Funciona para SQL Server?**  
    Sí, la actualización gradual del sistema operativo del clúster funciona para cualquier carga de trabajo del clúster. Sin embargo, solo es cero el tiempo de inactividad de Hyper-V y los clústeres de servidores de archivos de escalabilidad horizontal. La mayoría de las demás cargas de trabajo incurren en tiempo de inactividad (normalmente, un par de minutos) cuando se producen la conmutación por error y se requiere la conmutación por error al menos una vez durante el proceso de actualización gradual del sistema operativo  

**¿Puedo automatizar este proceso con PowerShell?**  
    Sí, hemos diseñado la actualización gradual del sistema operativo de clúster para que se Automatice con PowerShell.  

**Para un clúster de gran tamaño que tenga una carga de trabajo adicional y una capacidad de conmutación por error, ¿puedo actualizar varios nodos simultáneamente?**  
    Sí. Cuando se quita un nodo del clúster para actualizar el sistema operativo, el clúster tendrá un nodo menos para la conmutación por error; por lo tanto, tendrá una capacidad de conmutación por error reducida. En el caso de clústeres grandes con suficiente carga de trabajo y capacidad de conmutación por error, se pueden actualizar varios nodos simultáneamente. Puede agregar temporalmente nodos de clúster al clúster para proporcionar una mayor capacidad de carga de trabajo y conmutación por error durante el proceso de actualización gradual del sistema operativo del clúster.  

**¿Qué ocurre si se detecta un problema en el clúster después de que [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) se haya ejecutado correctamente?**  
    Si ha realizado una copia de seguridad de la base de datos del clúster con una copia de seguridad del estado del sistema antes de ejecutar [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), debería poder realizar una restauración autoritativa en un nodo de clúster de Windows Server 2012 R2 y restaurar la base de datos y la configuración originales del clúster.  

**¿Puedo usar la actualización en contexto para cada nodo en lugar de usar la instalación limpia del sistema operativo Si vuelve a formatear la unidad del sistema?**  
    No se recomienda el uso de la actualización en contexto de Windows Server, pero somos conscientes de que funciona en algunos casos en los que se usan controladores predeterminados. Lea atentamente todos los mensajes de advertencia que se muestran durante la actualización en contexto de un nodo de clúster.  

**Si utilizo la replicación de Hyper-V para una máquina virtual de Hyper-v en un clúster de Hyper-V, ¿la replicación permanece intacta durante y después del proceso de actualización gradual del sistema operativo del clúster?**  
    Sí, la réplica de Hyper-V permanece intacta durante y después del proceso de actualización gradual del sistema operativo del clúster.  

**¿Puedo usar System Center 2016 Virtual Machine Manager (SCVMM) para automatizar el proceso de actualización gradual del sistema operativo del clúster?**  
    Sí, puede automatizar el proceso de actualización gradual del sistema operativo del clúster mediante VMM en System Center 2016.  

## <a name="see-also"></a>Consulta también  
-   [Notas de la versión: problemas importantes en Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Novedades en Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Novedades de los clústeres de conmutación por error en Windows Server](whats-new-in-failover-clustering.md)  
