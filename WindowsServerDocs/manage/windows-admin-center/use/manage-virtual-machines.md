---
title: Administrar Virtual Machines con el centro de administración de Windows
description: Administración de hosts de Hyper-V y máquinas virtuales con el centro de administración de Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 02a33383b466e8bade2db0bbddaff66f0196954c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356858"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Administrar Virtual Machines con el centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

La herramienta Virtual Machines está disponible en las conexiones de [servidor](manage-servers.md), [clúster de conmutación por error](manage-failover-clusters.md) o [clúster hiperconvergido](manage-hyper-converged.md) si el rol de Hyper-V está habilitado en el servidor o clúster. Puede usar la herramienta de Virtual Machines para administrar hosts de Hyper-V que ejecutan Windows Server 2012 o posterior, ya sea instalado con experiencia de escritorio o como Server Core. También se admiten Hyper-V Server 2012, 2016 y 2019.

## <a name="key-features"></a>Principales características

Los aspectos destacados de la herramienta Virtual Machines del centro de administración de Windows incluyen:

- **Supervisión de recursos de host de Hyper-V de alto nivel.** Vea el uso general de la CPU y la memoria, las métricas de rendimiento de e/s, las alertas de estado de la máquina virtual y los eventos para el servidor host de Hyper-V o el clúster completo en un solo panel.
- **Experiencia unificada que reúne las capacidades de administrador de Hyper-V y Administrador de clústeres de conmutación por error.** Vea todas las máquinas virtuales en un clúster y explore en profundidad una única máquina virtual para la administración y solución de problemas avanzadas.
- **Flujos de trabajo simplificados pero potentes para la administración de máquinas virtuales.** Nuevas experiencias de IU adaptadas a escenarios de administración de TI para crear, administrar y replicar máquinas virtuales.

Estas son algunas de las tareas de Hyper-V que puede realizar en el centro de administración de Windows:

- [Supervisar el rendimiento y los recursos del host de Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Ver inventario de máquinas virtuales](#view-virtual-machine-inventory)
- [Crear una nueva máquina virtual](#create-a-new-virtual-machine)
- [Cambiar la configuración de la máquina virtual](#change-virtual-machine-settings)
- [Migración en vivo de una máquina virtual a otro nodo de clúster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Administración avanzada y solución de problemas para una sola máquina virtual](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Administración de una máquina virtual a través del host de Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Cambiar la configuración del host de Hyper-V](#change-hyper-v-host-settings)
- [Ver registros de eventos de Hyper-V](#view-hyper-v-event-logs)
- [Protección de máquinas virtuales con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Supervisar el rendimiento y los recursos del host de Hyper-V

![Virtual Machines pantalla de Resumen](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. Hay dos pestañas en la parte superior de la herramienta **virtual machines** , la pestaña **Resumen** y la pestaña **inventario** . La pestaña **Resumen** proporciona una vista holística de los recursos del host de Hyper-V y el rendimiento del servidor actual o del clúster completo, incluidos los siguientes:
    - El número de máquinas virtuales agrupadas por estado: en ejecución, desactivada, en pausa y guardadas
    - Alertas de estado recientes o eventos del registro de eventos de Hyper-V (las alertas solo están disponibles para clústeres hiperconvergidos que ejecutan Windows Server 2016 o posterior)
    - Uso de la CPU y la memoria con el análisis de host vs huésped
    - Principales máquinas virtuales que consumen la mayoría de los recursos de CPU y memoria
    - Gráficos de líneas de datos en directo e históricos para el rendimiento de IOPS y e/s (los gráficos de líneas de rendimiento de almacenamiento solo están disponibles para los clústeres hiperconvergidos que ejecutan Windows Server 2016 o posterior. Los datos históricos solo están disponibles para los clústeres hiperconvergidos que ejecutan Windows Server 2019).

## <a name="view-virtual-machine-inventory"></a>Ver inventario de máquinas virtuales

![Virtual Machines pantalla de inventario](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. Hay dos pestañas en la parte superior de la herramienta **virtual machines** , la pestaña **Resumen** y la pestaña **inventario** . En la pestaña **inventario** se enumeran las máquinas virtuales disponibles en el servidor actual o en todo el clúster y se proporcionan comandos para administrar máquinas virtuales individuales. Se puede hacer lo siguiente:
    - Vea una lista de las máquinas virtuales que se ejecutan en el servidor o clúster actual.
    - Vea el estado y el servidor host de la máquina virtual si está viendo las máquinas virtuales de un clúster. Vea también el uso de la CPU y la memoria desde la perspectiva del host, incluida la presión de memoria, la demanda de memoria y la memoria asignada, y el tiempo de actividad de la máquina virtual, el estado de latido y el estado de protección mediante Azure Site Recovery.
    - [Cree una nueva máquina virtual](#create-a-new-virtual-machine).
    - Eliminar, iniciar, apagar, apagar, pausar, reanudar, restablecer o cambiar el nombre de una máquina virtual. Guarde también la máquina virtual, elimine un estado guardado o cree un punto de control.
    - [Cambiar la configuración de una máquina virtual](#change-virtual-machine-settings).
    - Conéctese a una consola de máquina virtual mediante VMConnect a través del host de Hyper-V.
    - [Replicar una máquina virtual mediante Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - En el caso de las operaciones que se pueden ejecutar en varias máquinas virtuales, como iniciar, apagar, guardar, pausar, eliminar, restablecer, puede seleccionar varias máquinas virtuales y ejecutar la operación a la vez.

Nota: Si está conectado a un clúster, la herramienta máquina virtual solo mostrará las máquinas virtuales en clúster. Tenemos previsto Mostrar también máquinas virtuales no en clúster en el futuro.

## <a name="create-a-new-virtual-machine"></a>Crear una máquina virtual nueva

![Pantalla crear nueva máquina virtual](../media/manage-virtual-machines/new-vm.png)

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **inventario** y, a continuación, haga clic en **nuevo** para crear una nueva máquina virtual.
3. Escriba el nombre de la máquina virtual y elija entre las máquinas virtuales de generación 1 y 2.
4. Si va a crear una máquina virtual en un clúster, puede elegir en qué host se va a crear inicialmente la máquina virtual. Si está ejecutando Windows Server 2016 o una versión posterior, la herramienta le proporcionará una recomendación de host.
5. Elija una ruta de acceso para los archivos de la máquina virtual. Elija un volumen en la lista desplegable o haga clic en **examinar** para elegir una carpeta con el selector de carpetas. Los archivos de configuración de la máquina virtual y el archivo de disco duro virtual se guardarán en `\Hyper-V\\[virtual machine name]` una sola carpeta en la ruta de acceso del volumen o la ruta de acceso seleccionados.

   >[!Tip]
   > En el selector de carpetas, puede ir a cualquier recurso compartido de SMB disponible en la red. para ello, escriba la ruta de acceso en el campo **nombre de carpeta** como ```\\server\share```. El uso de un recurso compartido de red para el almacenamiento de máquina virtual requerirá [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Elija el número de procesadores virtuales, si desea que la virtualización anidada esté habilitada, configure las opciones de memoria, los adaptadores de red, los discos duros virtuales y elija si desea instalar un sistema operativo desde un archivo de imagen. ISO o desde la red.
7. Haga clic en **Crear** para crear la máquina virtual.
8. Una vez creada la máquina virtual y aparece en la lista de máquinas virtuales, puede iniciar la máquina virtual.
9. Una vez que se inicia la máquina virtual, puede conectarse a la consola de la máquina virtual a través de VMConnect para instalar el sistema operativo. Seleccione la máquina virtual en la lista, haga clic en **más** > **conectar** para descargar el archivo. RDP. Abra el archivo. RDP en la aplicación Conexión a Escritorio remoto. Puesto que se está conectando a la consola de la máquina virtual, deberá escribir las credenciales de administrador del host de Hyper-V.

## <a name="change-virtual-machine-settings"></a>Cambiar la configuración de la máquina virtual

![Pantalla de configuración de la máquina virtual](../media/manage-virtual-machines/vm-settings.png)

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **inventario** . Elija una máquina virtual de la lista y haga clic en **más** > **configuración**.
3. Cambie entre las pestañas **General**, **seguridad**, **memoria**, **procesadores**, **discos**, **redes**, **orden de arranque** y **puntos de control** , configure las opciones necesarias y, a continuación, haga clic en **Guardar** para guardar configuración de la pestaña actual. La configuración disponible variará en función de la generación de la máquina virtual. Además, algunas opciones de configuración no se pueden cambiar para máquinas virtuales en ejecución y primero deberá detener la máquina virtual.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migración en vivo de una máquina virtual a otro nodo de clúster

Si está conectado a un clúster, puede migrar en vivo una máquina virtual a otro nodo de clúster.

1. En un clúster de conmutación por error o una conexión de clúster hiperconvergida, haga clic en la herramienta de **virtual machines** desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **inventario** . Elija una máquina virtual de la lista y haga clic en **más** > **Move**.
3. Elija un servidor de la lista de nodos de clúster disponibles y haga clic en **Move**.
4. Las notificaciones para el progreso de movimiento se mostrarán en la esquina superior derecha del centro de administración de Windows. Si el movimiento se realiza correctamente, verá el nombre del servidor host cambiado en la lista de máquinas virtuales.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Administración avanzada y solución de problemas para una sola máquina virtual

![Pantalla de detalles de la máquina virtual única](../media/manage-virtual-machines/vm-details.png)

Puede ver información detallada y gráficos de rendimiento para una única máquina virtual desde la página de una sola máquina virtual.

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **inventario** . Haga clic en el nombre de una máquina virtual en la lista de máquinas virtuales.
3. En la página de una sola máquina virtual, puede:
    - Vea información detallada de la máquina virtual.
    - Ver gráficos de líneas de datos en directo e históricos para la CPU, la memoria, la red, la IOPS y el rendimiento de e/s (los datos históricos solo están disponibles para los clústeres hiperconvergidos que ejecutan Windows Server 2019)
    - Ver, crear, aplicar, cambiar el nombre y eliminar puntos de control.
    - Vea los detalles de los archivos de disco duro virtual (. vhd) de la máquina virtual, los adaptadores de red y el servidor host.
    - Eliminar, iniciar, apagar, apagar, pausar, reanudar, restablecer o cambiar el nombre de la máquina virtual. Guarde también la máquina virtual, elimine un estado guardado o cree un punto de control.
    - [Cambiar la configuración de la máquina virtual](#change-virtual-machine-settings).
    - Conéctese a la consola de la máquina virtual mediante VMConnect a través del host de Hyper-V.
    - [Replique la máquina virtual mediante Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>Administración de una máquina virtual a través del host de Hyper-V (VMConnect)

![Conexión de máquina virtual a través del explorador Web](../media/manage-virtual-machines/vm-connect.png)

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **inventario** . Elija una máquina virtual de la lista y haga clic en **más** > **conectar** o **descargar el archivo RDP**. **Connect** le permitirá interactuar con la máquina virtual invitada a través de la consola web de escritorio remoto, integrada en el centro de administración de Windows. **Descargar el archivo RDP** descargará un archivo. RDP que se puede abrir con la aplicación conexión a escritorio remoto (mstsc. exe). Ambas opciones usarán VMConnect para conectarse a la máquina virtual invitada a través del host de Hyper-V y requerirán que escriba las credenciales de administrador para el servidor host de Hyper-V.

## <a name="change-hyper-v-host-settings"></a>Cambiar la configuración del host de Hyper-V

![Pantalla de configuración de host de Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. En un servidor, un clúster hiperconvergido o una conexión de clúster de conmutación por error, haga clic en el menú **configuración** en la parte inferior del panel de navegación del lado izquierdo.
2. En un clúster o servidor host de Hyper-V, verá un grupo de **configuración de host de Hyper-v** con las siguientes secciones:
    - General: Cambiar la ruta de acceso del archivo de los discos duros virtuales y las máquinas virtuales y el tipo de programación del hipervisor (si se admite)
    - Modo de sesión mejorada
    - Expansión de NUMA
    - Migración en vivo
    - Migración de almacenamiento
3. Si realiza cambios en la configuración de un host de Hyper-V en un clúster hiperconvergido o en una conexión de clúster de conmutación por error, el cambio se aplicará a todos los nodos del clúster.

## <a name="view-hyper-v-event-logs"></a>Ver registros de eventos de Hyper-V

Puede ver los registros de eventos de Hyper-V directamente desde la herramienta Virtual Machines.

1. Haga clic en la herramienta **virtual machines** del panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta Virtual Machines, elija la pestaña **Resumen** . En la sección eventos de la parte superior derecha, haga clic en **Ver todos los eventos**.
3. La herramienta Visor de eventos mostrará los canales de eventos de Hyper-V en el panel izquierdo. Elija un canal para ver los eventos en el panel derecho. Si está administrando un clúster de conmutación por error o un clúster hiperconvergido, los registros de eventos mostrarán los eventos de todos los nodos del clúster y mostrarán el servidor host en la columna equipo.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Protección de máquinas virtuales con Azure Site Recovery

Puede usar el centro de administración de Windows para configurar Azure Site Recovery y replicar las máquinas virtuales locales en Azure. [Más información](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>Más próximamente

La administración de máquinas virtuales en el centro de administración de Windows está activamente en desarrollo y las nuevas características se agregarán en un futuro próximo. Puede ver el estado y votar para las características de UserVoice:

- [Importar o exportar máquinas virtuales](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [Ordenar máquinas virtuales en carpetas](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [Compatibilidad con la configuración de máquina virtual adicional](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Compatibilidad con réplicas de Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [Delegar la propiedad de la máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [Clonar máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [Crear una plantilla a partir de una máquina virtual existente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [Ver máquinas virtuales en hosts de Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [Configurar VLAN en el panel nueva máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[Vea todas o proponer nuevas características](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D).
