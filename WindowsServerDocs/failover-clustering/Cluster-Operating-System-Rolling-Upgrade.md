---
title: Actualización gradual del sistema operativo del clúster
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: f56c036768de7c1afcf3327135a7ff7d7a690a8b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440141"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Actualización gradual de sistema operativo del clúster

> Se aplica a: Windows Server 2019, Windows Server 2016

Actualización gradual de clúster del sistema operativo permite a un administrador actualizar el sistema operativo de los nodos del clúster sin detener la función Hyper-V o las cargas de trabajo de servidor de archivos de escalabilidad horizontal. Con esta característica, se pueden evitar las penalizaciones de tiempo de inactividad en los acuerdos de nivel de servicio (SLA).

Actualización gradual de clúster del sistema operativo proporciona las siguientes ventajas:

- Clústeres de conmutación por error que se ejecutan cargas de trabajo de servidor de archivos de escalabilidad horizontal (SOFS) y la máquina virtual de Hyper-V se pueden actualizar desde Windows Server 2012 R2 (que se ejecuta en todos los nodos del clúster) a Windows Server 2016 (que se ejecuta en todos los nodos del clúster del clúster) sin tiempo de inactividad. Otras cargas de trabajo de clúster, como SQL Server, no estará disponibles durante el tiempo (normalmente menos de cinco minutos) que se tarda en conmutar por error a Windows Server 2016.  
- No requiere ningún hardware adicional. Sin embargo, puede agregar más nodos del clúster temporalmente para clústeres pequeños para mejorar la disponibilidad del clúster durante la actualización de gradual de sistema operativo de clúster procesan.  
- El clúster no tiene que ser detenido o reiniciado.  
- No se requiere un nuevo clúster. Se actualiza el clúster existente. Además, se usan los objetos de clúster existentes almacenados en Active Directory.  
- El proceso de actualización es reversible hasta que la elige al cliente el "punto-de-no-return", cuando todos los nodos del clúster ejecutan Windows Server 2016, y cuando se ejecuta el cmdlet de PowerShell Update-ClusterFunctionalLevel.  
- El clúster puede admitir la aplicación de revisiones y operaciones de mantenimiento mientras se ejecuta en el modo mixto y sistema operativo.  
- Admite la automatización a través de PowerShell y WMI.  
- La propiedad pública del clúster **ClusterFunctionalLevel** propiedad indica el estado del clúster en los nodos de clúster de Windows Server 2016. Esta propiedad se puede consultar mediante el cmdlet de PowerShell desde un nodo de clúster de Windows Server 2016 que pertenezca a un clúster de conmutación por error:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Un valor de **8** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2012 R2. Un valor de **9** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2016.  

Esta guía describe las distintas fases del proceso de actualización gradual de clúster del sistema operativo, los pasos de instalación, las limitaciones de características y las preguntas más frecuentes (P+f) y es aplicable a los siguientes escenarios de actualización gradual de clúster del sistema operativo en Windows Server 2016:  
- Clústeres de Hyper-V  
- Clústeres de servidor de archivos de escalabilidad horizontal  

El siguiente escenario no se admite en Windows Server 2016:  
-  Actualización del SO gradual de clústeres de invitados con disco duro virtual (archivo .vhdx) como almacenamiento compartido del clúster  

Actualización gradual de clúster del sistema operativo es totalmente compatible por System Center Virtual Machine Manager (SCVMM) 2016. Si utilizas SCVMM 2016, consulte [realizar una actualización gradual de un clúster de hosts de Hyper-V a Windows Server 2016 en VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) para obtener instrucciones sobre cómo actualizar los clústeres y automatizar los pasos que se describen en este documento.  

## <a name="requirements"></a>Requisitos  
Complete los siguientes requisitos antes de comenzar el proceso de actualización gradual de clúster del sistema operativo:

- Comenzar con un clúster de conmutación por error que ejecuta Windows Server (canal semianual), Windows Server 2016 o Windows Server 2012 R2.
- Actualizar un clúster de espacios de almacenamiento directo en Windows Server, no se admite la versión 1709.
- Si la carga de trabajo de clúster es que las máquinas virtuales de Hyper-V o Scale-Out File Server, puede esperar la actualización sin tiempo de inactividad.
- Compruebe que los nodos de Hyper-V con la CPU que admiten la tabla de direcciones de segundo nivel (SLAT) mediante uno de los métodos siguientes:  
        ¿-Revise la [eres SLAT Compatible? WP8 SDK sugerencia 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) artículo que se describe dos métodos para comprobar si una CPU admite listones  
        -Descargue el [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) herramienta para determinar si una CPU admite SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Estados de transición del clúster durante la actualización gradual de clúster del sistema operativo

Esta sección describen los distintos Estados de transición del clúster de Windows Server 2012 R2 que se está actualizando a Windows Server 2016 mediante la actualización gradual de clúster del sistema operativo.  

Para mantener las cargas de trabajo de clúster ejecutándose durante el proceso de actualización gradual de clúster del sistema operativo, mover una carga de trabajo de clúster desde un nodo de Windows Server 2012 R2 a Windows Server 2016 nodo funciona como si ambos nodos estaban ejecutando el sistema operativo Windows Server 2012 R2. Cuando se agregan nodos de Windows Server 2016 en el clúster, operan en un modo de compatibilidad de Windows Server 2012 R2. Un nuevo modo de clúster conceptual, denominado "Modo mixto y sistema operativo", permite que los nodos de diferentes versiones que exista en el mismo clúster (consulte la figura 1).  

![Ilustración que muestra las tres fases de una actualización gradual de SO del clúster: todos los nodos de Windows Server 2012 R2, el modo mixto y sistema operativo y todos los nodos de Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: Transiciones de estado del sistema operativo de clúster**  

Un clúster de Windows Server 2012 R2 entra en modo mixto y sistema operativo cuando se agrega un nodo de Windows Server 2016 en el clúster. El proceso es completamente reversible: se pueden quitar nodos de Windows Server 2016 desde el clúster y se pueden agregar nodos de Windows Server 2012 R2 en el clúster en este modo. El "punto sin retorno" se produce cuando se ejecuta el cmdlet de PowerShell Update-ClusterFunctionalLevel en el clúster. Para este cmdlet se realice correctamente, todos los nodos deben ser Windows Server 2016 y todos los nodos deben estar en línea.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Estados de transición de un clúster de cuatro nodos mientras se realiza la actualización gradual de SO

En esta sección se muestra y describe las cuatro fases distintas de un clúster con almacenamiento compartido que los nodos se actualizan desde Windows Server 2012 R2 a Windows Server 2016.  

"Fase 1" es el estado inicial, empezaremos con un clúster de Windows Server 2012 R2.  

![Ilustración que muestra el estado inicial: todos los nodos de Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: Estado inicial: Clúster de conmutación por error de Windows Server 2012 R2 (fase 1)**  

En "fase 2", dos nodos han ha en pausa, purgar, expulsados, volver a formatear e instalado con Windows Server 2016.  

![Ilustración que muestra el clúster en modo mixto y sistema operativo: fuera del clúster de 4 nodos de ejemplo, dos nodos ejecutan Windows Server 2016 y dos nodos ejecutan Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: Estado intermedio: Modo mixto y sistema operativo: Windows Server 2012 R2 y Windows Server 2016 Failover Cluster Server (fase 2)**  

En "fase 3", todos los nodos del clúster se han actualizado a Windows Server 2016 y el clúster esté listo para actualizarse con el cmdlet de PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> En esta fase, se puede invertir el proceso por completo y se pueden agregar nodos de Windows Server 2012 R2 a este clúster.  

![Ilustración que muestra que el clúster se ha actualizado completamente para Windows Server 2016 y está listo para el cmdlet Update-ClusterFunctionalLevel para elevar el nivel funcional del clúster hasta Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: Estado intermedio: Todos los nodos que se actualiza a Windows Server 2016, listo para Update-ClusterFunctionalLevel (fase 3)**  

Después de ejecuta la actualización-ClusterFunctionalLevelcmdlet, el clúster entra en "Fase 4", donde se pueden usar las nuevas características de clúster de Windows Server 2016.  

![Ilustración que muestra que la actualización de SO gradual de clúster se ha completado correctamente; todos los nodos se han actualizado a Windows Server 2016 y el clúster se está ejecutando en el nivel funcional del clúster de Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: Estado final: Clúster de conmutación por error de Windows Server 2016 (fase 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Proceso de actualización gradual de SO del clúster

En esta sección se describe el flujo de trabajo para realizar la actualización gradual de clúster del sistema operativo.  

![Ilustración que muestra el flujo de trabajo para actualizar un clúster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: Implementación de flujo de trabajo del proceso de actualización de SO del clúster**  

Actualización gradual de SO del clúster incluye los siguientes pasos:  

1. Preparar el clúster para la actualización del sistema operativo como sigue:  
    1. Actualización gradual de clúster del sistema operativo requiere quitar un nodo del clúster a la vez. Compruebe si tiene suficiente capacidad en el clúster para mantener la alta disponibilidad de SLA cuando uno de los nodos del clúster se quita del clúster para realizar una actualización de sistema operativo. ¿En otras palabras, necesita la capacidad de las cargas de trabajo de conmutación por error a otro nodo cuando se quita un nodo del clúster durante el proceso de actualización gradual de clúster del sistema operativo? ¿El clúster tiene la capacidad para ejecutar las cargas de trabajo necesarios cuando se quita un nodo del clúster para la actualización gradual de clúster del sistema operativo?  
    2. Para cargas de trabajo de Hyper-V, compruebe que todos los hosts de Windows Server 2016 Hyper-V tienen CPU compatible con la tabla de direcciones de segundo nivel (SLAT). Solo las máquinas compatibles con SLAT pueden usar el rol Hyper-V en Windows Server 2016.  
    3. Compruebe que las copias de seguridad de la carga de trabajo han completado y considere la posibilidad de realizar copias de seguridad del clúster. Detener las operaciones de copia de seguridad al agregar nodos al clúster.  
    4. Compruebe que todos los nodos del clúster estén en línea/ejecución/seguridad mediante la [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet (consulte la figura 7).  

        ![Captura de pantalla que muestra los resultados de ejecutar el cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: Determinar el estado del nodo mediante el cmdlet Get-ClusterNode**  

    5. Si está ejecutando las actualizaciones de compatible con clústeres (CAU), compruebe si se está ejecutando actualmente CAU utilizando el **actualizar conscientes del clúster** interfaz de usuario, o la [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet (consulte la figura 8). Dejar de usar la CAU la [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet (consulte la figura 9) para evitar que los nodos se pause y vacían CAU durante el proceso de actualización gradual de clúster del sistema operativo.  

        ![Captura de pantalla que muestra la salida del cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: Mediante el [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet para determinar si las actualizaciones compatibles con clúster se está ejecutando en el clúster**  

        ![Captura de pantalla que muestra la salida del cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: Deshabilitar la función de las actualizaciones de clúster compatibles con usando la [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet**  

2. Por cada nodo del clúster, realice lo siguiente:  
    1. UI del Administrador de clúster, seleccione un nodo y usar el **pausar | Purgar** opción de menú para purgar el nodo (consulte la figura 10) o usar el [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet (consulte la figura 11).  

        ![Captura de pantalla que muestra cómo se purgue los roles con la UI del Administrador de clústeres](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: Roles de purga de un nodo mediante el Administrador de clústeres de conmutación por error**  

        ![Captura de pantalla que muestra la salida del cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: Purgar roles desde un nodo mediante la [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet**  

    2.  Mediante el Administrador de clústeres de UI, **Evict** el nodo en pausa desde el clúster, o usar el [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet.  

        ![Captura de pantalla que muestra la salida del cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: Quitar un nodo de clúster con [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet**  

    3.  Volver a formatear la unidad del sistema y realizar una "instalación limpia del sistema operativo" de Windows Server 2016 en el nodo con el **personalizado: Instalar solo Windows (avanzado)** opción de instalación (consulte la figura 13) de setup.exe. No seleccione la **actualizar: Instalar Windows y mantener los archivos, configuraciones y aplicaciones** opción ya que la actualización gradual de clúster del sistema operativo no recomendamos la actualización en contexto.  

        ![Captura de pantalla del Asistente de instalación de Windows Server 2016 que muestra la opción de instalación personalizada seleccionada](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: Opciones de instalación disponibles para Windows Server 2016**  

    4.  Agregue el nodo para el dominio de Active Directory adecuado.  
    5.  Agregue los usuarios adecuados al grupo de administradores.  
    6.  Usar el cmdlet Install-WindowsFeature PowerShell o de UI del Administrador de servidor, instale los roles de servidor que necesita, como Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Usar el cmdlet Install-WindowsFeature PowerShell o de UI del Administrador de servidor, instale la característica clúster de conmutación por error.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Instale las características adicionales necesarias para las cargas de trabajo de clúster.  
    9. Compruebe la configuración de conectividad de red y de almacenamiento mediante la UI del Administrador de clústeres de conmutación por error.  
    10. Si se usa Firewall de Windows, compruebe que la configuración de Firewall es correcta para el clúster. Por ejemplo, clústeres de clúster compatible con la actualización (CAU) habilitada pueden requerir configuración de Firewall.  
    11. Para cargas de trabajo de Hyper-V, use la interfaz de usuario del Administrador de Hyper-V para iniciar el cuadro de diálogo Administrador de conmutadores virtuales (consulte la figura 14).  

        Compruebe que el nombre de la Virtual switches usa son idénticas para todos los nodos de host de Hyper-V en el clúster.  

        ![Captura de pantalla que muestra la ubicación del cuadro de diálogo Administrador de conmutadores virtuales de Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: Administrador de conmutadores virtuales**  

    12. En un nodo de Windows Server 2016 (no utilice un nodo de Windows Server 2012 R2), use el Administrador de clústeres de conmutación por error (consulte la figura 15) para conectarse al clúster.  

        ![Captura de pantalla que muestra el cuadro de diálogo Seleccionar clúster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: Agregar un nodo al clúster mediante el Administrador de clústeres de conmutación por error**  

    13. Uso de cualquier la UI del Administrador de clústeres de conmutación por error o la [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet (consulte la figura 16) para agregar el nodo al clúster.  

        ![Captura de pantalla que muestra la salida del cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: Agregar un nodo al clúster con [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > Cuando el primer nodo de Windows Server 2016 une al clúster, el clúster entra en modo "mixto SO", y se mueven los recursos principales de clúster en el nodo de Windows Server 2016. Un clúster en modo "mixto SO" es un clúster de completamente funcional donde los nuevos nodos se ejecutan en un modo de compatibilidad con los nodos antiguos. Modo de "SO mixto" es un modo transitorio para el clúster. No pretende ser permanente y se esperan que los clientes actualizar todos los nodos del clúster de su plazo de cuatro semanas.  

    14. Después de Windows Server 2016 está correctamente agregar nodo al clúster, puede (opcionalmente) mover algunas de las cargas de trabajo de clúster para el nodo recién agregado con el fin de equilibrar la carga de trabajo en el clúster como sigue:

        ![Captura de pantalla que muestra la salida del cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: Mover un clúster de carga de trabajo (rol de máquina virtual de clúster) mediante [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet**  

        1. Use **migración en vivo** desde el Administrador de clústeres de conmutación por error para las máquinas virtuales o el [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet (consulte la figura 17) para realizar una migración en vivo de las máquinas virtuales.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Use **mover** desde el Administrador de clústeres de conmutación por error o la [ `Move-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) cmdlet para otras cargas de trabajo de clúster.  

3. Cuando cada nodo ha actualizado a Windows Server 2016 y se vuelve a añadir al clúster, o cuando los nodos de Windows Server 2012 R2 restantes se expulsaron, realice lo siguiente:  

    > [!IMPORTANT]  
    > -   Después de actualizar el nivel funcional del clúster, no podrá volver al nivel funcional de Windows Server 2012 R2 y nodos de Windows Server 2012 R2 no se puede agregar al clúster.
    > -   Hasta que el [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet se ejecuta, el proceso es completamente reversible y nodos de Windows Server 2012 R2 se pueden agregar a este clúster y se pueden quitar los nodos de Windows Server 2016.  
    > -   Después de la [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet se ejecuta, las nuevas características estarán disponibles.  

    1.  Mediante la UI del Administrador de clústeres de conmutación por error o la [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet, compruebe que todos los roles de clúster se ejecutan en el clúster según lo previsto. En el ejemplo siguiente, no se utiliza almacenamiento disponible, en su lugar se utiliza CSV, por lo tanto, muestra el almacenamiento disponible un **Offline** estado (consulte la figura 18).  

        ![Captura de pantalla que muestra la salida del cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: Comprobar que todos los clúster grupos (roles de clúster) se ejecutan utilizando el [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet**  

    2.  Compruebe que todos los nodos del clúster estén en línea y se está ejecutando mediante el [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet.  
    3.  Ejecute el [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet - no debe devolver errores (vea la figura 19).  

        ![Captura de pantalla que muestra la salida del cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: Actualizar el nivel funcional de un clúster con PowerShell**  

    4.  Después de la [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet se ejecuta, las nuevas características están disponibles.  

4. Windows Server 2016: reanudar copias de seguridad y actualizaciones del clúster normal:  

    1. Si se estaba ejecutando anteriormente CAU, reiniciarlo mediante la UI de CAU o usar el [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet (Véase la figura 20).  

        ![Captura de pantalla que muestra la salida de la Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: Habilitar las actualizaciones de clúster compatibles con rol mediante la [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet**  

    2. Reanudar las operaciones de copia de seguridad.  

5. Habilitar y usar las características de Windows Server 2016 en Hyper-V Virtual Machines.  

    1. Después de que el clúster se ha actualizado al nivel funcional de Windows Server 2016, muchas cargas de trabajo como máquinas virtuales de Hyper-V tendrán funcionalidades de nuevo. Para obtener una lista de nuevas capacidades de Hyper-V. consulte [migración y actualización de las máquinas virtuales](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. En cada nodo de host de Hyper-V en el clúster, use la [ `Get-VMHostSupportedVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) cmdlet para ver las versiones de configuración de máquina virtual de Hyper-V que son compatibles con el host.  

        ![Captura de pantalla que muestra la salida del cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: Ver las versiones de configuración de máquina virtual de Hyper-V admitidas por el host**  

   3. En cada nodo de host de Hyper-V en el clúster, se pueden actualizar las versiones de configuración de máquina virtual de Hyper-V mediante la programación de una ventana de mantenimiento breve con usuarios, realizar copias de seguridad, al desactivar las máquinas virtuales y ejecutar el [ `Update-VMVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) cmdlet (consulte la Figura 22). Esto actualizará la versión de la máquina virtual y habilitar nuevas características de Hyper-V, lo que elimina la necesidad de futuras actualizaciones de componentes de integración de Hyper-V (IC). Este cmdlet se puede ejecutar desde el nodo de Hyper-V que hospeda la máquina virtual, o el `-ComputerName` parámetro puede usarse para actualizar la versión de la máquina virtual de forma remota. En este ejemplo, aquí se actualiza la versión de configuración de VM1 de 5.0 a 7.0 pueda beneficiarse de muchas características nuevas de Hyper-V asociadas con esta versión de configuración de máquina virtual como puntos de control de producción (copias de seguridad coherentes de aplicación) y VM binario archivo de configuración.  

       ![Captura de pantalla que muestra el cmdlet Update-VMVersion en acción](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
       **Figura 22: Actualización de una versión de la máquina virtual mediante el cmdlet de PowerShell Update-VMVersion**  

6. Los grupos de almacenamiento se pueden actualizar mediante el [Update-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) cmdlet de PowerShell: se trata de una operación en línea.  

Aunque se usa como destino escenarios de nube privada, específicamente en Hyper-V y clústeres de servidor de archivos de escalabilidad horizontal, que pueden actualizarse sin tiempo de inactividad, el proceso de actualización gradual de clúster del sistema operativo pueden usarse para cualquier rol de clúster.  

## <a name="restrictions--limitations"></a>Restricciones y limitaciones  
- Esta característica solo funciona para Windows Server 2012 R2 a solo las versiones de Windows Server 2016. Esta característica no puede actualizar versiones anteriores de Windows Server como Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2016.  
- Cada nodo de Windows Server 2016 debe volver a formatear nueva instalación solo. "En contexto" o "Actualizar" no se recomienda el tipo de instalación.  
- Un nodo de Windows Server 2016 debe usarse para agregar nodos de Windows Server 2016 en el clúster.  
- Al administrar un clúster en modo mixto y sistema operativo, siempre que realice las tareas de administración desde un nodo de nivel superior que se está ejecutando Windows Server 2016. Los nodos de nivel inferior de Windows Server 2012 R2 no pueden usar herramientas de administración o de interfaz de usuario en Windows Server 2016.  
- Le animamos a los clientes mover rápidamente a través del proceso de actualización de clúster porque algunas características de clúster no están optimizados para el modo mixto y sistema operativo.  
- Evite crear o cambiar el tamaño de almacenamiento en nodos de Windows Server 2016 mientras el clúster se ejecuta en modo mixto y sistema operativo debido a posibles incompatibilidades en conmutación por error desde un nodo de Windows Server 2016 a nodos de Windows Server 2012 R2 de nivel inferior.  

## <a name="frequently-asked-questions"></a>Preguntas frecuentes  
**¿Cuánto tiempo puede ejecutar el clúster de conmutación por error en modo mixto y sistema operativo?**  
    Recomendamos a los clientes para completar la actualización en cuatro semanas. Hay muchas optimizaciones en Windows Server 2016. Actualizamos correctamente Hyper-V y clústeres de servidor de archivos de escalabilidad horizontal con tiempo de inactividad cero en menos de cuatro horas total.  

**¿Se portar esta característica a Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008?**  
    No tenemos planes para portar esta característica a las versiones anteriores. Actualización gradual de clúster del sistema operativo es nuestra visión de la actualización de clústeres de Windows Server 2012 R2 a Windows Server 2016 y mucho más.  

**¿Necesita el clúster de Windows Server 2012 R2 tener todas las actualizaciones de software instaladas antes de iniciar el proceso de actualización gradual de clúster del sistema operativo?**  
    Sí, antes de comenzar el proceso de actualización gradual de clúster del sistema operativo, compruebe que todos los nodos del clúster se actualizan con las últimas actualizaciones de software.  

**¿Puedo ejecutar el [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet mientras los nodos están desactivadas o en pausa?**  
    No. Todos los nodos del clúster deben encontrarse en y en la pertenencia a active el [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet funcione.  

**¿Actualización gradual de clúster del sistema operativo funciona para cualquier carga de trabajo de clúster? ¿Funciona para SQL Server?**  
    Sí, la actualización gradual de clúster del sistema operativo funciona para cualquier carga de trabajo de clúster. Sin embargo, es única sin tiempo de inactividad para Hyper-V y clústeres de servidor de archivos de escalabilidad horizontal. La mayoría de otras cargas de trabajo incurrir en tiempo de inactividad (normalmente un par de minutos) cuando se conmutación por error y conmutación por error no se requiere al menos una vez durante el proceso de actualización gradual de clúster del sistema operativo.  

**¿Puedo automatizar este proceso con PowerShell?**  
    Sí, hemos diseñado OS actualización gradual de clúster que se automatice mediante PowerShell.  

**Para un clúster grande que tiene la carga de trabajo adicional y la capacidad de conmutación por error, ¿se puede actualizar varios nodos simultáneamente?**  
    Sí. Cuando se quita un nodo del clúster para actualizar el sistema operativo, el clúster tendrá un nodo menos para la conmutación por error, por lo tanto, tendrán una capacidad reducida de conmutación por error. Clústeres grandes con suficiente capacidad de conmutación por error y la carga de trabajo, se pueden actualizar varios nodos simultáneamente. Puede agregar provisionalmente los nodos del clúster para el clúster para proporcionar una carga de trabajo mejorado y la capacidad de conmutación por error durante el proceso de actualización gradual de clúster del sistema operativo.  

**¿Qué ocurre si detecta un problema en mi clúster después de [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) se ha ejecutado correctamente?**  
    Si se dispone de copia de seguridad la base de datos de clúster con una copia de seguridad del estado del sistema antes de ejecutar [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), debe llevar a cabo una autorización restaure en un nodo de clúster de Windows Server 2012 R2 y el clúster original base de datos y la configuración.  

**¿Puedo usar actualización en contexto para cada nodo en lugar de usar la instalación del sistema operativo limpio volviendo a formatear la unidad del sistema?**  
    No se recomienda el uso de la actualización en contexto de Windows Server, pero somos conscientes de que funciona en algunos casos donde se usan controladores predeterminados. Lea detenidamente muestran todos los mensajes de advertencia durante la actualización en contexto de un nodo de clúster.  

**¿Si estoy usando replicación de Hyper-V para una máquina virtual de Hyper-V en mi clúster de Hyper-V, replicación permanecerán intacta durante y después del proceso de actualización gradual de clúster del sistema operativo?**  
    Sí, la réplica de Hyper-V permanece intacta durante y después del proceso de actualización gradual de clúster del sistema operativo.  

**¿Puedo usar System Center 2016 Virtual Machine Manager (SCVMM) para automatizar el proceso de actualización gradual de clúster del sistema operativo?**  
    Sí, puede automatizar el proceso de actualización gradual de clúster del sistema operativo con VMM en System Center 2016.  

## <a name="see-also"></a>Vea también  
-   [Notas de la versión: Problemas importantes en Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Novedades en Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Novedades de la conmutación por error en Windows Server](whats-new-in-failover-clustering.md)  
