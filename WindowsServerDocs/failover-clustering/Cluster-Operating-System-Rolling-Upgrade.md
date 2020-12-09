---
title: Actualización gradual del sistema operativo del clúster
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
manager: lizross
ms.date: 03/27/2018
ms.openlocfilehash: a61025f972445f37aeeece764558aab853dc90df
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866404"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Actualización gradual del sistema operativo del clúster

> Se aplica a: Windows Server 2019, Windows Server 2016

La actualización gradual de SO del clúster permite a un administrador actualizar el sistema operativo de los nodos del clúster sin detener las cargas de trabajo de Hyper-V o del servidor de archivos Scale-Out. Con esta característica, se pueden evitar las penalizaciones de tiempo de inactividad en los acuerdos de nivel de servicio (SLA).

La actualización gradual de SO del clúster proporciona las siguientes ventajas:

- Los clústeres de conmutación por error que ejecutan las cargas de trabajo de máquina virtual de Hyper-V y servidor de archivos de escalabilidad horizontal (SOFS) se pueden actualizar desde Windows Server 2012 R2 (que se ejecuta en todos los nodos del clúster) a Windows Server 2016 (que se ejecuta en todos los nodos del clúster) sin tiempo de inactividad. Otras cargas de trabajo de clúster, como SQL Server, no estarán disponibles durante el tiempo (normalmente menos de cinco minutos) que se tarda en conmutar por error a Windows Server 2016.
- No requiere hardware adicional. Aunque puede agregar nodos de clúster adicionales temporalmente a los clústeres pequeños para mejorar la disponibilidad del clúster durante el proceso de actualización gradual del sistema operativo del clúster.
- No es necesario detener o reiniciar el clúster.
- No es necesario un nuevo clúster. El clúster existente se actualiza. Además, se usan los objetos de clúster existentes almacenados en Active Directory.
- El proceso de actualización es reversible hasta que el cliente elija "punto de no devolución", cuando todos los nodos del clúster ejecuten Windows Server 2016 y cuando se ejecute el cmdlet de PowerShell de Update-ClusterFunctionalLevel.
- El clúster puede admitir operaciones de revisión y mantenimiento mientras se ejecuta en el modo de sistema operativo mixto.
- Admite la automatización a través de PowerShell y WMI.
- La propiedad **ClusterFunctionalLevel** de la propiedad pública del clúster indica el estado del clúster en los nodos de clúster de Windows Server 2016. Esta propiedad se puede consultar mediante el cmdlet de PowerShell desde un nodo de clúster de Windows Server 2016 que pertenezca a un clúster de conmutación por error:
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel
    ```

    Un valor de **8** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2012 R2. Un valor de **9** indica que el clúster se está ejecutando en el nivel funcional de Windows Server 2016.

En esta guía se describen las distintas fases del proceso de actualización gradual del sistema operativo del clúster, los pasos de instalación, las limitaciones de las características y las preguntas más frecuentes (p + f), y es aplicable a los siguientes escenarios de actualización gradual del sistema operativo de clúster en Windows Server 2016:
- Clústeres de Hyper-V
- Clústeres de servidores de archivos Scale-Out

El siguiente escenario no se admite en Windows Server 2016:
-  Actualización gradual del sistema operativo de clúster de los clústeres invitados con el disco duro virtual (archivo. vhdx) como almacenamiento compartido

La actualización gradual del sistema operativo del clúster es totalmente compatible con System Center Virtual Machine Manager (SCVMM) 2016. Si usa SCVMM 2016, consulte [realizar una actualización gradual de un clúster de hosts de Hyper-V a Windows Server 2016 en VMM](/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) para obtener instrucciones sobre cómo actualizar los clústeres y automatizar los pasos que se describen en este documento.

## <a name="requirements"></a>Requisitos
Complete los requisitos siguientes antes de comenzar el proceso de actualización gradual del sistema operativo del clúster:

- Comience con un clúster de conmutación por error que ejecute Windows Server (canal semianual), Windows Server 2016 o Windows Server 2012 R2.
- No se admite la actualización de un clúster de Espacios de almacenamiento directo a Windows Server, versión 1709.
- Si la carga de trabajo del clúster son máquinas virtuales de Hyper-V o Scale-Out servidor de archivos, puede esperar una actualización sin tiempo de inactividad.
- Compruebe que los nodos de Hyper-V tienen CPU que admiten Second-Level tabla de direcciones (SLAT) mediante uno de los métodos siguientes:       -Revisar [¿es compatible con slat? ](/archive/blogs/devfish/are-you-slat-compatible-wp8-sdk-tip-01) Artículo de la sugerencia 01 del SDK de WP8 en el que se describen dos métodos para comprobar si una CPU admite SLATs-descargar la herramienta [Coreinfo v 3.31](/sysinternals/downloads/coreinfo) para determinar si una CPU es compatible con slat.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Estados de transición del clúster durante la actualización gradual del sistema operativo del clúster

En esta sección se describen los distintos Estados de transición del clúster de Windows Server 2012 R2 que se está actualizando a Windows Server 2016 mediante la actualización gradual del sistema operativo del clúster.

Para mantener las cargas de trabajo del clúster en ejecución durante el proceso de actualización gradual del sistema operativo del clúster, el traslado de una carga de trabajo de clúster desde un nodo de Windows Server 2012 R2 al nodo de Windows Server 2016 funciona como si ambos nodos estuvieran ejecutando el sistema operativo Windows Server 2012 R2. Cuando se agregan nodos de Windows Server 2016 al clúster, funcionan en modo de compatibilidad de Windows Server 2012 R2. Un nuevo modo de clúster conceptual, denominado "modo de sistema operativo mixto", permite que existan nodos de versiones diferentes en el mismo clúster (vea la ilustración 1).

![Ilustración que muestra las tres etapas de una actualización gradual del sistema operativo del clúster: todos los nodos Windows Server 2012 R2, modo mixto-SO y todos los nodos Windows Server 2016 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)
 **Ilustración 1: transiciones de estado del sistema operativo del clúster**

Un clúster de Windows Server 2012 R2 entra en modo mixto de sistema operativo cuando se agrega un nodo de Windows Server 2016 al clúster. El proceso es totalmente reversible: los nodos de Windows Server 2016 se pueden quitar del clúster y los nodos de Windows Server 2012 R2 se pueden agregar al clúster en este modo. El "punto de no retorno" se produce cuando se ejecuta el cmdlet de PowerShell de Update-ClusterFunctionalLevel en el clúster. Para que este cmdlet se ejecute correctamente, todos los nodos deben ser Windows Server 2016 y todos los nodos deben estar en línea.

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Estados de transición de un clúster de cuatro nodos mientras se realiza la actualización gradual del sistema operativo

En esta sección se muestran y describen las cuatro fases diferentes de un clúster con almacenamiento compartido cuyos nodos se actualizan de Windows Server 2012 R2 a Windows Server 2016.

"Fase 1" es el estado inicial: comenzamos con un clúster de Windows Server 2012 R2.

![Ilustración que muestra el estado inicial: todos los nodos Windows Server 2012 R2 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)
 **ilustración 2: estado inicial: clúster de conmutación por error de windows Server 2012 R2 (fase 1)**

En la "fase 2", dos nodos se han pausado, purgado, expulsado, reformateado e instalado con Windows Server 2016.

![Ilustración en la que se muestra el clúster en modo de sistema operativo mixto: fuera del clúster de 4 nodos de ejemplo, dos nodos ejecutan Windows Server 2016 y dos nodos ejecutan Windows Server 2012 R2 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)
 **figura 3: estado intermedio: modo mixto-SO: windows Server 2012 R2 y clúster de conmutación por error de windows Server 2016 (fase 2)**

En la "fase 3", todos los nodos del clúster se han actualizado a Windows Server 2016 y el clúster está listo para actualizarse con Update-ClusterFunctionalLevel cmdlet de PowerShell.

> [!NOTE]
> En esta fase, el proceso se puede invertir completamente y los nodos de Windows Server 2012 R2 se pueden agregar a este clúster.

![Ilustración en la que se muestra que el clúster se ha actualizado completamente a Windows Server 2016 y está listo para que el cmdlet Update-ClusterFunctionalLevel Conecte el nivel funcional del clúster a Windows Server 2016, ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)
 **figura 4: estado intermedio: todos los nodos actualizados a windows Server 2016, listos para Update-ClusterFunctionalLevel (fase 3)**

Una vez ejecutada la Update-ClusterFunctionalLevelcmdlet, el clúster entra en "fase 4", donde se pueden usar las nuevas características de clúster de Windows Server 2016.

![Ilustración en la que se muestra que la actualización del sistema operativo gradual del clúster se ha completado correctamente. todos los nodos se han actualizado a Windows Server 2016 y el clúster se está ejecutando en el nivel funcional del clúster de Windows Server 2016 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)
 **ilustración 5: estado final: clúster de conmutación por error de windows Server 2016 (fase 4)**

## <a name="cluster-os-rolling-upgrade-process"></a>Proceso de actualización gradual de SO del clúster

En esta sección se describe el flujo de trabajo para realizar la actualización gradual del sistema operativo del clúster.

![Ilustración que muestra el flujo de trabajo para actualizar un clúster ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)
 **figura 6: flujo de trabajo del proceso de actualización gradual de SO del clúster**

La actualización gradual del sistema operativo del clúster incluye los pasos siguientes:

1. Prepare el clúster para la actualización del sistema operativo de la siguiente manera:
    1. La actualización gradual del sistema operativo del clúster requiere la eliminación de un nodo a la vez del clúster. Compruebe si tiene suficiente capacidad en el clúster para mantener los SLA de alta disponibilidad cuando uno de los nodos del clúster se quita del clúster para una actualización del sistema operativo. En otras palabras, ¿necesita la capacidad de conmutar por error las cargas de trabajo a otro nodo cuando se quita un nodo del clúster durante el proceso de actualización gradual del sistema operativo del clúster? ¿El clúster tiene la capacidad de ejecutar las cargas de trabajo necesarias cuando se quita un nodo del clúster para la actualización gradual del sistema operativo del clúster?
    2. En el caso de las cargas de trabajo de Hyper-V, compruebe que todos los hosts de Hyper-V de Windows Server 2016 tienen compatibilidad de CPU Second-Level tabla de direcciones (SLAT). Solo los equipos compatibles con SLAT pueden usar el rol de Hyper-V en Windows Server 2016.
    3. Compruebe que las copias de seguridad de la carga de trabajo se han completado y considere la posibilidad de realizar una copia de seguridad del clúster. Detenga las operaciones de copia de seguridad al agregar nodos al clúster.
    4. Compruebe que todos los nodos del clúster están en línea/Running/up con el [`Get-ClusterNode`](/powershell/module/failoverclusters/Get-ClusterNode) cmdlet (consulte la figura 7).

        ![Captura que muestra los resultados de la ejecución del cmdlet Get-ClusterNode ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png) **figura 7: determinar el estado del nodo mediante Get-ClusterNode cmdlet**

    5. Si está ejecutando actualizaciones compatibles con clústeres (CAU), compruebe si la CAU se está ejecutando actualmente mediante la interfaz **de usuario de actualización compatible con clústeres** o el [`Get-CauRun`](/powershell/module/clusterawareupdating/Get-CauRun) cmdlet (consulte la figura 8). Detenga la CAU con el [`Disable-CauClusterRole`](/powershell/module/clusterawareupdating/Disable-CauClusterRole) cmdlet (consulte la figura 9) para evitar que la Cau deje de usar los nodos durante el proceso de actualización gradual del sistema operativo del clúster.

        ![Captura que muestra la salida del cmdlet Get-CauRun ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png) **figura 8: uso del [`Get-CauRun`](/powershell/module/clusterawareupdating/Get-CauRun) cmdlet para determinar si las actualizaciones compatibles con clústeres se están ejecutando en el clúster**

        ![Captura que muestra la salida del cmdlet Disable-CauClusterRole ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png) **figura 9: deshabilitación del rol de actualizaciones compatibles con clústeres mediante el [`Disable-CauClusterRole`](/powershell/module/clusterawareupdating/Disable-CauClusterRole) cmdlet**

2. Para cada nodo del clúster, realice lo siguiente:
    1. Mediante la interfaz de usuario del administrador de clústeres, seleccione un nodo y use la **pausa |** Opción de menú de purga para purgar el nodo (vea la figura 10) o usar el [`Suspend-ClusterNode`](/powershell/module/failoverclusters/Suspend-ClusterNode) cmdlet (consulte la figura 11).

        ![Captura que muestra cómo purgar roles con la interfaz de usuario del administrador de clústeres ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png) **figura 10: purgar roles de un nodo mediante administrador de clústeres de conmutación por error**

        ![Captura que muestra la salida del cmdlet Suspend-ClusterNode ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png) **figura 11: purgar roles de un nodo mediante el [`Suspend-ClusterNode`](/powershell/module/failoverclusters/Suspend-ClusterNode) cmdlet**

    2.  Mediante la interfaz de usuario del administrador de clústeres, **expulse** el nodo en pausa del clúster o use el [`Remove-ClusterNode`](/powershell/module/failoverclusters/Remove-ClusterNode) cmdlet.

        ![Captura que muestra la salida del cmdlet Remove-ClusterNode ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)
         **figura 12: quitar un nodo del clúster mediante el [`Remove-ClusterNode`](/powershell/module/failoverclusters/Remove-ClusterNode) cmdlet**

    3.  Vuelva a formatear la unidad del sistema y realice una "instalación limpia del sistema operativo" de Windows Server 2016 en el nodo mediante la opción **personalizada: instalar solo Windows (** vea la figura 13) en setup.exe. Evite seleccionar la opción **actualizar: instalar Windows y mantener archivos, opciones de configuración y aplicaciones** porque la actualización gradual del sistema operativo del clúster no fomenta la actualización en contexto.

        ![Captura del Asistente para la instalación de Windows Server 2016 que muestra la opción instalación personalizada seleccionada ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)
         **figura 13: opciones de instalación disponibles para Windows Server 2016**

    4.  Agregue el nodo al dominio de Active Directory adecuado.
    5.  Agregue los usuarios adecuados al grupo de administradores.
    6.  Con la interfaz de usuario de Administrador del servidor o Install-WindowsFeature cmdlet de PowerShell, instale los roles de servidor que necesite, como Hyper-V.

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V
        ```

    7.  Con la interfaz de usuario de Administrador del servidor o Install-WindowsFeature cmdlet de PowerShell, instale la característica de clústeres de conmutación por error.

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering
        ```

    8.  Instale las características adicionales que necesiten las cargas de trabajo del clúster.
    9. Compruebe la configuración de conectividad de red y almacenamiento mediante la interfaz de usuario de Administrador de clústeres de conmutación por error.
    10. Si se usa Firewall de Windows, compruebe que la configuración del firewall sea correcta para el clúster. Por ejemplo, los clústeres habilitados para la actualización compatible con clústeres (CAU) pueden requerir la configuración del firewall.
    11. En el caso de cargas de trabajo de Hyper-V, use la interfaz de usuario del administrador de Hyper-V para iniciar el cuadro de diálogo Administrador de conmutadores virtuales (vea la figura 14).

        Compruebe que el nombre de los conmutadores virtuales que se usan son idénticos para todos los nodos de host de Hyper-V del clúster.

        ![Captura Mostrar la ubicación del cuadro de diálogo Administrador de conmutadores virtuales de Hyper-V ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)
         **figura 14: administrador de conmutadores virtuales**

    12. En un nodo de Windows Server 2016 (no use un nodo de Windows Server 2012 R2), use el Administrador de clústeres de conmutación por error (vea la figura 15) para conectarse al clúster.

        ![Captura mostrar el cuadro de diálogo Seleccionar clúster ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)
         **figura 15: agregar un nodo al clúster mediante administrador de clústeres de conmutación por error**

    13. Use la interfaz de usuario de Administrador de clústeres de conmutación por error o el [`Add-ClusterNode`](/powershell/module/failoverclusters/Add-ClusterNode) cmdlet (consulte la figura 16) para agregar el nodo al clúster.

        ![Captura que muestra la salida del cmdlet Add-ClusterNode ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)
         **figura 16: agregar un nodo al clúster mediante el [`Add-ClusterNode`](/powershell/module/failoverclusters/Add-ClusterNode) cmdlet**

        > [!NOTE]
        > Cuando el primer nodo de Windows Server 2016 se une al clúster, el clúster entra en modo "Mixed-OS" y los recursos principales del clúster se mueven al nodo Windows Server 2016. Un clúster de modo "Mixed-OS" es un clúster totalmente funcional en el que los nuevos nodos se ejecutan en un modo de compatibilidad con los nodos antiguos. El modo "Mixed-OS" es un modo transitorio para el clúster. No pretende ser permanente y se espera que los clientes actualicen todos los nodos de su clúster en un plazo de cuatro semanas.

    14. Después de agregar correctamente el nodo de Windows Server 2016 al clúster, puede (opcionalmente) trasladar parte de la carga de trabajo del clúster al nodo recién agregado para volver a equilibrar la carga de trabajo en el clúster de la siguiente manera:

        ![Captura que muestra la salida del cmdlet Move-ClusterVirtualMachineRole ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)
         **figura 17: mover una carga de trabajo de clúster (rol de VM de clúster) mediante el [`Move-ClusterVirtualMachineRole`](/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole) cmdlet**

        1. Use **migración en vivo** del administrador de clústeres de conmutación por error para máquinas virtuales o el [`Move-ClusterVirtualMachineRole`](/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole) cmdlet (consulte la figura 17) para realizar una migración en vivo de las máquinas virtuales.

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3
            ```

        2. Use **Move** desde el administrador de clústeres de conmutación por error o el [`Move-ClusterGroup`](/powershell/module/failoverclusters/Move-ClusterGroup) cmdlet para otras cargas de trabajo de clúster.

3. Cuando todos los nodos se hayan actualizado a Windows Server 2016 y se hayan agregado de nuevo al clúster, o cuando se hayan expulsado los nodos de Windows Server 2012 R2 restantes, haga lo siguiente:

    > [!IMPORTANT]
    > -   Después de actualizar el nivel funcional del clúster, no puede volver al nivel funcional de Windows Server 2012 R2 y los nodos de Windows Server 2012 R2 no se pueden agregar al clúster.
    > -   Hasta que [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) se ejecute el cmdlet, el proceso es totalmente reversible y se pueden agregar nodos de Windows server 2012 R2 a este clúster y se pueden quitar los nodos de Windows server 2016.
    > -   Una vez [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) ejecutado el cmdlet, estarán disponibles nuevas características.

    1.  Con la interfaz de usuario de Administrador de clústeres de conmutación por error o el [`Get-ClusterGroup`](/powershell/module/failoverclusters/Get-ClusterGroup) cmdlet, compruebe que todos los roles de clúster se ejecutan en el clúster según lo esperado. En el ejemplo siguiente, el almacenamiento disponible no se usa, sino que se usa CSV, por lo tanto, el almacenamiento disponible muestra un estado **sin conexión** (consulte la figura 18).

        ![Captura que muestra la salida del cmdlet Get-ClusterGroup ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)
         **figura 18: comprobación de que todos los grupos de clústeres (roles de clúster) se están ejecutando con el [`Get-ClusterGroup`](/powershell/module/failoverclusters/Get-ClusterGroup) cmdlet**

    2.  Compruebe que todos los nodos del clúster están en línea y se ejecutan con el [`Get-ClusterNode`](/powershell/module/failoverclusters/Get-ClusterNode) cmdlet.
    3.  Ejecutar el [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet: no se deben devolver errores (vea la figura 19).

        ![Captura que muestra la salida del cmdlet Update-ClusterFunctionalLevel ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)
         **figura 19: actualizar el nivel funcional de un clúster mediante PowerShell**

    4.  Una vez que [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) se ejecuta el cmdlet, están disponibles nuevas características.

4. Windows Server 2016: reanudar las actualizaciones y copias de seguridad normales del clúster:

    1. Si previamente estaba ejecutando CAU, reinícielo con la interfaz de usuario de CAU o use el [`Enable-CauClusterRole`](/powershell/module/clusterawareupdating/Enable-CauClusterRole) cmdlet (consulte la figura 20).

        ![Captura que muestra la salida de la función enable-CauClusterRole de la ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png) **figura 20: habilitar las actualizaciones compatibles con clústeres mediante el [`Enable-CauClusterRole`](/powershell/module/clusterawareupdating/Enable-CauClusterRole) cmdlet**

    2. Reanudar las operaciones de copia de seguridad.

5. Habilitar y usar las características de Windows Server 2016 en Hyper-V Virtual Machines.

    1. Una vez que el clúster se ha actualizado al nivel funcional de Windows Server 2016, muchas cargas de trabajo, como las máquinas virtuales de Hyper-V, tendrán nuevas funcionalidades. Para obtener una lista de las nuevas funcionalidades de Hyper-V. consulte [migración y actualización de máquinas virtuales](../virtualization/hyper-v/deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)

    2. En cada nodo de host de Hyper-V del clúster, use el [`Get-VMHostSupportedVersion`](/powershell/module/hyper-v/Get-VMHostSupportedVersion) cmdlet para ver las versiones de configuración de la máquina virtual de Hyper-v admitidas por el host.

        ![Captura muestra la salida del cmdlet Get-VMHostSupportedVersion ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png) **figura 21: ver las versiones de configuración de la máquina virtual de Hyper-V admitidas por el host**

   3. En cada nodo de host de Hyper-V del clúster, las versiones de configuración de máquina virtual de Hyper-V se pueden actualizar mediante la programación de una breve ventana de mantenimiento con usuarios, la copia de seguridad, la desactivación de máquinas virtuales y la ejecución del [`Update-VMVersion`](/powershell/module/hyper-v/Update-VMVersion) cmdlet (consulte la figura 22). Esto actualizará la versión de la máquina virtual y habilitará las nuevas características de Hyper-V, lo que elimina la necesidad de futuras actualizaciones de componentes de integración de Hyper-V (CI). Este cmdlet se puede ejecutar desde el nodo de Hyper-V que hospeda la máquina virtual, o bien `-ComputerName` se puede usar el parámetro para actualizar la versión de la máquina virtual de forma remota. En este ejemplo, se actualiza la versión de configuración de VM1 de 5,0 a 7,0 para aprovechar muchas de las nuevas características de Hyper-V asociadas a esta versión de configuración de máquina virtual, como los puntos de control de producción (copias de seguridad coherentes con la aplicación) y el archivo de configuración de máquina virtual binaria.

       ![Captura que muestra el cmdlet Update-VMVersion en la acción ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png) **figura 22: actualización de una versión de máquina virtual mediante el cmdlet de PowerShell de Update-VMVersion**

6. Los grupos de almacenamiento se pueden actualizar mediante el cmdlet de PowerShell [Update-StoragePool](/powershell/module/storage/Update-StoragePool) , que es una operación en línea.

Aunque nos centramos en escenarios de nube privada, específicamente Hyper-V y clústeres de servidores de archivos de escalabilidad horizontal, que se pueden actualizar sin tiempo de inactividad, el proceso de actualización gradual del sistema operativo del clúster se puede usar para cualquier rol de clúster.

## <a name="restrictions--limitations"></a>Restricciones y limitaciones
- Esta característica solo funciona para Windows Server 2012 R2 a las versiones 2016 de Windows Server. Esta característica no puede actualizar versiones anteriores de Windows Server, como Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2016.
- Solo se debe cambiar el formato o la nueva instalación de cada nodo de Windows Server 2016. No se recomienda el tipo de instalación "en contexto" o "actualizar".
- Se debe usar un nodo de Windows Server 2016 para agregar nodos de Windows Server 2016 al clúster.
- Al administrar un clúster de modo de sistema operativo mixto, realice siempre las tareas de administración desde un nodo de nivel superior que ejecute Windows Server 2016. Los nodos de nivel inferior de Windows Server 2012 R2 no pueden usar la interfaz de usuario o las herramientas de administración en Windows Server 2016.
- Animamos a los clientes a pasar por el proceso de actualización del clúster rápidamente porque algunas características del clúster no están optimizadas para el modo mixto-OS.
- Evite crear o cambiar el tamaño del almacenamiento en los nodos de Windows Server 2016 mientras el clúster se ejecuta en modo de sistema operativo mixto debido a posibles incompatibilidades en la conmutación por error de un nodo de Windows Server 2016 a nodos de nivel inferior de Windows Server 2012 R2.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
**¿Cuánto tiempo puede ejecutarse el clúster de conmutación por error en modo de sistema operativo mixto?**
Animamos a los clientes a completar la actualización en un plazo de cuatro semanas. Hay muchas optimizaciones en Windows Server 2016. Hemos actualizado correctamente los clústeres de servidores de archivos de escalabilidad horizontal y Hyper-V sin tiempo de inactividad en menos de cuatro horas en total.

**¿Trasladará esta característica de nuevo a Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008?**
No tenemos ningún plan para volver a migrar esta característica a las versiones anteriores. La actualización gradual del sistema operativo de clúster es nuestra visión de actualizar clústeres de Windows Server 2012 R2 a Windows Server 2016 y versiones posteriores.

**¿Es necesario que el clúster de Windows Server 2012 R2 tenga todas las actualizaciones de software instaladas antes de iniciar el proceso de actualización gradual del sistema operativo del clúster?**
Sí, antes de iniciar el proceso de actualización gradual del sistema operativo del clúster, compruebe que todos los nodos del clúster estén actualizados con las actualizaciones de software más recientes.

**¿Puedo ejecutar el [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) cmdlet mientras los nodos están desactivados o pausados?**
No. Todos los nodos del clúster deben estar encendidos y en pertenencia activa para [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) que el cmdlet funcione.

**¿Funciona la actualización gradual del sistema operativo de clúster para cualquier carga de trabajo del clúster? ¿Funciona para SQL Server?**
Sí, la actualización gradual del sistema operativo del clúster funciona para cualquier carga de trabajo del clúster. Sin embargo, solo es cero el tiempo de inactividad de Hyper-V y los clústeres de servidores de archivos de escalabilidad horizontal. La mayoría de las demás cargas de trabajo incurren en tiempo de inactividad (normalmente, un par de minutos) cuando se producen la conmutación por error y se requiere la conmutación por error al menos una vez durante el proceso de actualización gradual del sistema operativo

**¿Puedo automatizar este proceso con PowerShell?**
Sí, hemos diseñado la actualización gradual del sistema operativo de clúster para que se Automatice con PowerShell.

**Para un clúster de gran tamaño que tenga una carga de trabajo adicional y una capacidad de conmutación por error, ¿puedo actualizar varios nodos simultáneamente?**
Sí. Cuando se quita un nodo del clúster para actualizar el sistema operativo, el clúster tendrá un nodo menos para la conmutación por error; por lo tanto, tendrá una capacidad de conmutación por error reducida. En el caso de clústeres grandes con suficiente carga de trabajo y capacidad de conmutación por error, se pueden actualizar varios nodos simultáneamente. Puede agregar temporalmente nodos de clúster al clúster para proporcionar una mayor capacidad de carga de trabajo y conmutación por error durante el proceso de actualización gradual del sistema operativo del clúster.

**¿Qué ocurre si se detecta un problema en el clúster después de que se haya [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) ejecutado correctamente?**
Si ha realizado una copia de seguridad de la base de datos del clúster con una copia de seguridad del estado del sistema antes de ejecutar [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) , debería poder realizar una restauración autoritativa en un nodo de clúster de Windows Server 2012 R2 y restaurar la base de datos y la configuración originales del clúster.

**¿Puedo usar la actualización en contexto para cada nodo en lugar de usar la instalación limpia del sistema operativo Si vuelve a formatear la unidad del sistema?**
No se recomienda el uso de la actualización en contexto de Windows Server, pero somos conscientes de que funciona en algunos casos en los que se usan controladores predeterminados. Lea atentamente todos los mensajes de advertencia que se muestran durante la actualización en contexto de un nodo de clúster.

**Si utilizo la replicación de Hyper-V para una máquina virtual de Hyper-v en un clúster de Hyper-V, ¿la replicación permanece intacta durante y después del proceso de actualización gradual del sistema operativo del clúster?**
Sí, la réplica de Hyper-V permanece intacta durante y después del proceso de actualización gradual del sistema operativo del clúster.

**¿Puedo usar System Center 2016 Virtual Machine Manager (SCVMM) para automatizar el proceso de actualización gradual del sistema operativo del clúster?**
Sí, puede automatizar el proceso de actualización gradual del sistema operativo del clúster mediante VMM en System Center 2016.

## <a name="additional-references"></a>Referencias adicionales
-   [Notas de la versión: Problemas importantes en Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
-   [Novedades en Windows Server 2016](../get-started/whats-new-in-windows-server-2016.md)
-   [Novedades de los clústeres de conmutación por error de Windows Server](whats-new-in-failover-clustering.md)
