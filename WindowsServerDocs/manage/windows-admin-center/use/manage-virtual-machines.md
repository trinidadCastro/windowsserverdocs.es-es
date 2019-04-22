---
title: Administración de máquinas virtuales con Windows Admin Center
description: Administración de hosts de Hyper-V y máquinas virtuales con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 41767b9e53c0106931e78f86f8675e413cca0d0a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816886"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Administración de máquinas virtuales con Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

La herramienta de máquinas virtuales está disponible en [Server](manage-servers.md), [clúster de conmutación por error](manage-failover-clusters.md) o [Hyper-Converged clúster](manage-hyper-converged.md) conexiones si el rol de Hyper-V está habilitado en el servidor o clúster. Puede usar la herramienta de máquinas virtuales para administrar hosts de Hyper-V que ejecutan Windows Server 2012 o versiones posteriores, ya sea instaladas con experiencia de escritorio o como Server Core. También se admiten Hyper-V Server 2012 y 2016.

## <a name="key-features"></a>Principales características

Aspectos destacados de la herramienta de máquinas virtuales en Windows Admin Center incluyen:

- **Hyper-V host recursos supervisión de alto nivel.** Ver uso de CPU y memoria global, las métricas de rendimiento de E/S, alertas de estado de la máquina virtual y eventos para el servidor host de Hyper-V o clúster completo en un solo panel.
- **Experiencia unificada reúne las funciones de administrador de Hyper-V y Administrador de clústeres de conmutación por error.** Ver todas las máquinas virtuales en un clúster y profundizar en una sola máquina virtual para la administración avanzada y solución de problemas.
- **Flujos de trabajo, pero potentes para administración de máquinas virtuales.** Nuevas experiencias de usuario se adaptan a escenarios de administración de TI para crear, administrar y replicar máquinas virtuales.

Estas son algunas de las tareas de Hyper-V que puede hacer en Windows Admin Center:

- [Supervisar los recursos de host de Hyper-V y el rendimiento](#monitor-hyper-v-host-resources-and-performance)
- [Ver el inventario de máquina virtual](#view-virtual-machine-inventory)
- [Crear una nueva máquina virtual](#create-a-new-virtual-machine)
- [Cambiar la configuración de máquina virtual](#change-virtual-machine-settings)
- [Migración en vivo de una máquina virtual a otro nodo del clúster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Administración avanzada y solución de problemas de una sola máquina virtual](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Ver registros de eventos de Hyper-V](#view-hyper-v-event-logs)
- [Proteger las máquinas virtuales con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Supervisar los recursos de host de Hyper-V y el rendimiento

![Pantalla de resumen de las máquinas virtuales](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. Hay dos fichas en la parte superior de la **máquinas virtuales** herramienta, el **resumen** ficha y el **inventario** ficha. El **resumen** ficha proporciona una vista holística de los recursos de host de Hyper-V y el rendimiento para el servidor actual o el clúster completo, incluido lo siguiente:
    - El número de máquinas virtuales agrupadas por estado: en ejecución, desactivado, en pausa y guardado
    - Alertas de estado reciente o eventos de registro de eventos de Hyper-V (las alertas solo están disponibles para los clústeres hiperconvergido que ejecuta Windows Server 2016 o posterior)
    - Uso de CPU y memoria con el desglose de invitado de host vs
    - Máquinas virtuales principales que consumen más recursos de CPU y memoria
    - Gráficos de rendimiento de e/s por segundo y la E/S de líneas de datos históricos y en vivo (gráficos de líneas de rendimiento de almacenamiento solo están disponibles para los clústeres hiperconvergido que ejecuta Windows Server 2016 o posterior. Los datos históricos solo están disponibles para clústeres hiperconvergido que ejecuta Windows Server 2019)

## <a name="view-virtual-machine-inventory"></a>Ver el inventario de máquina virtual

![Pantalla de inventario de las máquinas virtuales](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. Hay dos fichas en la parte superior de la **máquinas virtuales** herramienta, el **resumen** ficha y el **inventario** ficha. El **inventario** ficha enumera las máquinas virtuales disponibles en el servidor actual o todo el clúster y proporciona comandos para administrar máquinas virtuales individuales. Se puede hacer lo siguiente:
    - Ver una lista de las máquinas virtuales que se ejecutan en el servidor actual o el clúster.
    - Ver el servidor de host y el estado de la máquina virtual si está viendo las máquinas virtuales para un clúster. Ver también el uso de CPU y memoria desde la perspectiva del host, incluida la presión de memoria, la demanda de memoria y memoria asignada y la máquina virtual de tiempo de actividad, estado del latido y estado de protección con Azure Site Recovery.
    - [Crear una nueva máquina virtual](#create-a-new-virtual-machine).
    - Eliminar, iniciar, apagar, apagar, pausar, reanudar, restablecer o cambiar el nombre de una máquina virtual. También guardar la máquina virtual, elimine un estado guardado o crear un punto de control.
    - [Cambiar la configuración de una máquina virtual](#change-virtual-machine-settings).
    - Conectarse a una consola de máquina virtual con VMConnect mediante el host de Hyper-V.
    - [Replicar una máquina virtual mediante Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

Nota: Si está conectado a un clúster, la herramienta de la máquina Virtual mostrará solo máquinas virtuales en clúster. Tenemos previsto también se muestran en el futuro sin agrupar máquinas virtuales.

## <a name="create-a-new-virtual-machine"></a>Crear una máquina virtual nueva

![Crear nueva pantalla de la máquina virtual](../media/manage-virtual-machines/new-vm.png)

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta de máquinas virtuales, elija el **inventario** y, después, haga clic en **New** para crear una nueva máquina virtual.
3. Escriba el nombre de la máquina virtual y elija entre las máquinas virtuales de generación 1 y 2.
4. Si va a crear una máquina virtual en un clúster, puede elegir qué host para crear inicialmente la máquina virtual en. Si está ejecutando Windows Server 2016 o versiones posteriores, la herramienta proporcionará una recomendación de host para usted.
5. Elija una ruta de acceso para los archivos de máquina virtual. Elija un volumen en la lista desplegable o haga clic en **examinar** para elegir una carpeta con el selector de carpeta. Los archivos de configuración de máquina virtual y el archivo de disco duro virtual se guardarán en una sola carpeta en el `\Hyper-V\\[virtual machine name]` ruta de acceso de la ruta de acceso o el volumen seleccionado.
6. Elija el número de procesadores virtuales, si desea habilitar la virtualización anidada, configurar opciones de memoria, adaptadores de red, discos duros virtuales y elija si desea instalar un sistema operativo desde un archivo de imagen .iso o desde la red.
7. Haga clic en **Crear** para crear la máquina virtual.
8. Una vez que la máquina virtual se crea y aparece en la lista de máquinas virtuales, puede iniciar la máquina virtual.
9. Una vez que se inicia la máquina virtual, puede conectarse a la consola de la máquina virtual mediante VMConnect para instalar el sistema operativo. Seleccione la máquina virtual en la lista, haga clic en **más** > **Connect** para descargar el archivo RDP. Abra el archivo .rdp en la aplicación de conexión a Escritorio remoto. Puesto que este se conecta a la consola de la máquina virtual, deberá especificar las credenciales de administrador del host de Hyper-V.

## <a name="change-virtual-machine-settings"></a>Cambiar la configuración de máquina virtual

![Pantalla de configuración de máquina virtual](../media/manage-virtual-machines/vm-settings.png)

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta de máquinas virtuales, elija el **inventario** ficha. Elija una máquina virtual en la lista y haga clic en **más** > **configuración**.
3. Cambiar entre el **General**, **memoria**, **procesadores**, **discos**, **redes**, **Orden de arranque** y **puntos de control** pestaña, configure las opciones necesarias y luego haga clic en **guardar** para guardar la configuración de la ficha actual. Las opciones disponibles varían en función de generación de la máquina virtual. Además, no se puede cambiar algunos valores de configuración para ejecutar máquinas virtuales y tendrá que detener la máquina virtual en primer lugar.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migración en vivo de una máquina virtual a otro nodo del clúster

Si está conectado a un clúster, puede migrar en vivo una máquina virtual a otro nodo del clúster.

1. En un clúster de conmutación por error o la conexión del clúster hiperconvergido, haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta de máquinas virtuales, elija el **inventario** ficha. Elija una máquina virtual en la lista y haga clic en **más** > **mover**.
3. Elija un servidor de la lista de nodos de clúster disponibles y haga clic en **mover**.
4. Las notificaciones para ver el progreso de movimiento se mostrará en la esquina superior derecha de Windows Admin Center. Si el movimiento se realiza correctamente, verá el nombre del servidor Host puede cambiar en la lista de máquinas virtuales.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Administración avanzada y solución de problemas de una sola máquina virtual

![Pantalla de detalles de la máquina virtual única](../media/manage-virtual-machines/vm-details.png)

Puede ver información detallada y gráficos de rendimiento de una sola máquina virtual desde la página de la máquina virtual única.

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta de máquinas virtuales, elija el **inventario** ficha. Haga clic en el nombre de una máquina virtual desde la lista de máquinas virtuales.
3. En la página de la máquina virtual única, hacer lo siguiente:
    - Ver información detallada de la máquina virtual.
    - Ver Live y los gráficos de líneas de datos históricos de CPU, memoria, red y rendimiento IOPS y E/S (datos históricos solo están disponibles para clústeres hiperconvergido que ejecuta Windows Server 2019)
    - Ver, crear, aplicar, cambiar el nombre y eliminar puntos de control.
    - Ver los detalles de los archivos de disco duro virtual (.vhd), adaptadores de red y servidor host de la máquina virtual.
    - Eliminar, iniciar, apagar, apagar, pausar, reanudar, restablecer o cambiar el nombre de la máquina virtual. También guardar la máquina virtual, elimine un estado guardado o crear un punto de control.
    - [Cambiar la configuración de la máquina virtual](#change-virtual-machine-settings).
    - Conectarse a la consola de máquina virtual con VMConnect mediante el host de Hyper-V.
    - [Replicar la máquina virtual mediante Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="view-hyper-v-event-logs"></a>Ver registros de eventos de Hyper-V

Puede ver los registros de eventos de Hyper-V directamente desde la herramienta de máquinas virtuales.

1. Haga clic en el **máquinas virtuales** herramienta desde el panel de navegación del lado izquierdo.
2. En la parte superior de la herramienta de máquinas virtuales, elija el **resumen** ficha. En la sección eventos superior derecha, haga clic en **todos los eventos de vista**.
3. La herramienta Visor de eventos mostrará los canales de eventos de Hyper-V en el panel izquierdo. Elija un canal para ver los eventos en el panel derecho. Si está administrando un clúster de conmutación por error o un clúster hiperconvergido, los registros de eventos muestra eventos para todos los nodos del clúster, muestra el servidor de host en la columna de la máquina.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Proteger las máquinas virtuales con Azure Site Recovery

Puede usar Windows Admin Center para configurar Azure Site Recovery y replicar máquinas virtuales locales a Azure. [Más información](azure-services.md)

## <a name="more-coming"></a>Pero pronto habrá más

Administración de máquinas virtuales en Windows Admin Center activamente está en desarrollo y se agregarán nuevas características en un futuro próximo. Puede ver el estado y vote por características en UserVoice:

|Solicitud de característica|
|-------|
|[Importación y exportación de máquinas virtuales](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[Máquinas virtuales de ordenación en las carpetas](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[Configuración de máquina virtual adicional de soporte técnico](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[Compatibilidad con réplica de Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[Delegar la propiedad de la máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[Clonar máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[Crear una plantilla desde una máquina virtual existente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[Ver la máquinas virtuales entre hosts de Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[Configurar las VLAN en el panel de la nueva máquina Virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**Ver todos o Proponer nueva característica**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
