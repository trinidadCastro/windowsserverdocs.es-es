---
title: Administración de máquinas virtuales con Windows Admin Center
description: Administrar hosts de Hyper-V y máquinas virtuales con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 374ee30dcbaf9af3caa60ee85ec59fd3c158206b
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296757"
---
# Administración de máquinas virtuales con Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

La herramienta de máquinas virtuales está disponible en las conexiones de [servidor](manage-servers.md), el [Clúster de conmutación por error](manage-failover-clusters.md) o el [Clúster hiperconvergido](manage-hyper-converged.md) si está habilitado el rol de Hyper-V en el servidor o clúster. Puedes usar la herramienta de máquinas virtuales para administrar hosts de Hyper-V que ejecutan Windows Server 2012 instalado o posterior, ya sea con experiencia de escritorio o en modo Server Core. Hyper-V Server 2012, 2016 y 2019 también son compatibles.

## Características clave

Aspectos destacados de la herramienta de máquinas virtuales en Windows Admin Center incluyen:

- **Alto nivel Hyper-V host supervisión de recursos.** Ver general uso de CPU y memoria, las métricas de rendimiento de E/S, alertas de estado de la máquina virtual y eventos para el servidor de host de Hyper-V o todo el clúster en un único panel de información.
- **Experiencia unificada reunir a las capacidades de administrador de Hyper-V y el Administrador de clústeres de conmutación por error.** Permite ver todas las máquinas virtuales en un clúster y profundizar en una sola máquina virtual para la administración avanzada y solución de problemas.
- **Flujos de trabajo simplificados pero eficaces para la administración de la máquina virtual.** Nuevas experiencias de la interfaz de usuario se adapta a escenarios de administración de TI para crear, administrar y replicar máquinas virtuales.

Estas son algunas de las tareas de Hyper-V que puedes hacer en Windows Admin Center:

- [Supervisar los recursos de host de Hyper-V y rendimiento](#monitor-hyper-v-host-resources-and-performance)
- [Permite ver el inventario de máquina virtual](#view-virtual-machine-inventory)
- [Crear una nueva máquina virtual](#create-a-new-virtual-machine)
- [Cambiar la configuración de máquina virtual](#change-virtual-machine-settings)
- [Migración en vivo una máquina virtual a otro nodo de clúster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Administración avanzada y solución de problemas de una sola máquina virtual](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Administrar una máquina virtual a través del host de Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Cambiar la configuración de host de Hyper-V](#change-hyper-v-host-settings)
- [Ver los registros de eventos de Hyper-V](#view-hyper-v-event-logs)
- [Proteger las máquinas virtuales con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## Supervisar los recursos de host de Hyper-V y rendimiento

![Pantalla de resumen de las máquinas virtuales](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. Hay dos pestañas en la parte superior de la herramienta de **máquinas virtuales** , la pestaña de **Resumen** y la pestaña de **inventario** . La pestaña **Resumen** proporciona una visión global de recursos del host de Hyper-V y rendimiento para el servidor actual o todo clúster, incluido lo siguiente:
    - El número de máquinas virtuales agrupados por estado - ejecutando, apagado, pausa y guardado
    - Eventos de registro de eventos de Hyper-V (alertas solo están disponibles para clústeres hiperconvergidos que ejecutan Windows Server 2016 o posterior) o reciente alertas de estado
    - Uso de CPU y memoria con el desglose de invitado de host vs
    - Principales máquinas virtuales que consume más recursos de CPU y memoria
    - Gráficos de rendimiento IOPS y E/S de línea de datos históricos y dinámicos (gráficos de líneas de rendimiento de almacenamiento solo están disponibles para clústeres hiperconvergidos que ejecutan Windows Server 2016 o posterior. Datos históricos solo están disponibles para clústeres hiperconvergidos que ejecutan Windows Server 2019)

## Permite ver el inventario de máquina virtual

![Pantalla de inventario de máquinas virtuales](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. Hay dos pestañas en la parte superior de la herramienta de **máquinas virtuales** , la pestaña de **Resumen** y la pestaña de **inventario** . La pestaña de **inventario** enumera las máquinas virtuales disponibles en el servidor actual o todo el clúster y proporciona comandos para administrar máquinas virtuales individuales. Se puede hacer lo siguiente:
    - Ver una lista de las máquinas virtuales que se ejecutan en el clúster o servidor actual.
    - Ver el servidor de host y el estado de la máquina virtual si estás viendo las máquinas virtuales para un clúster. También permite ver el uso de CPU y memoria desde la perspectiva del host, incluida la presión de memoria, demanda de memoria y memoria asignada y la máquina virtual actividad, estado del latido y el estado de protección con Azure Site Recovery.
    - [Crear una nueva máquina virtual](#create-a-new-virtual-machine).
    - Eliminar, iniciar, desactivar, apagar, pausar, reanudar, restablecer o cambiar el nombre de una máquina virtual. Guardar la máquina virtual, eliminar un estado guardado o crear un punto de control.
    - [Cambiar la configuración de una máquina virtual](#change-virtual-machine-settings).
    - Conectarse a una consola de máquina virtual con VMConnect mediante el host de Hyper-V.
    - [Replicar una máquina virtual con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Para las operaciones que se pueden ejecutar en varias máquinas virtuales, por ejemplo, inicio, cerrar, guardar, pausar, eliminar, restablecer, puedes seleccionar varias máquinas virtuales y ejecutar la operación a la vez.

Nota: Si estás conectado a un clúster, la herramienta de máquina Virtual solo mostrará las máquinas virtuales en clúster. Tenemos previsto volver a mostrar también las máquinas virtuales no agrupados en el futuro.

## Crear una nueva máquina virtual

![Crear nuevo filtro de máquina virtual](../media/manage-virtual-machines/new-vm.png)

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **inventario** y después haz clic en **nuevo** para crear una nueva máquina virtual.
3. Escribe el nombre de máquina virtual y elegir entre máquinas virtuales de generación 1 y 2.
4. Si vas a crear una máquina virtual en un clúster, puedes elegir qué host para crear inicialmente la máquina virtual en. Si estás ejecutando Windows Server 2016 o posterior, la herramienta proporcionará una recomendación de host para TI.
5. Elegir una ruta de acceso para los archivos de la máquina virtual. Elegir un volumen en la lista desplegable o haga clic en **Examinar** para seleccionar una carpeta con el selector de carpetas. Los archivos de configuración de máquina virtual y el archivo de disco duro virtual se guardará en una sola carpeta en el `\Hyper-V\\[virtual machine name]` ruta de acceso de la ruta de acceso o el volumen seleccionado.

>[!Tip]
> En el selector de carpetas, se puede examinar a cualquier recurso compartido de SMB disponible en la red especificando la ruta de acceso en el campo de **nombre de carpeta** como ```\\server\share```. Usar un recurso compartido de red para el almacenamiento VM tardará [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Elegir el número de procesadores virtuales, si quieres que la virtualización anidada habilitada, configurar la configuración de la memoria, adaptadores de red, los discos duros virtuales y elegir si quieres instalar un sistema operativo desde un archivo de imagen .iso o desde la red.
7. Haz clic en **crear** para crear la máquina virtual.
8. Una vez que la máquina virtual se crea y aparece en la lista de máquinas virtuales, puede iniciar la máquina virtual.
9. Una vez que se inicia la máquina virtual, puede conectarse a la consola de la máquina virtual mediante VMConnect para instalar el sistema operativo. Seleccione la máquina virtual en la lista, haz clic en **más** > **Connect** para descargar el archivo RDP. Abre el archivo RDP en la aplicación de la conexión a Escritorio remoto. Dado que esto se conecta a la consola de la máquina virtual, tendrás que escribir las credenciales de administrador del host de Hyper-V.

## Cambiar la configuración de máquina virtual

![Pantalla de configuración de máquina virtual](../media/manage-virtual-machines/vm-settings.png)

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **inventario** elige una máquina virtual de la lista y haz clic en **más** > **configuración**.
3. Cambiar entre la pestaña **General**, **seguridad**, **memoria**, **procesadores**, **discos**, **redes**, **orden de arranque** y **puntos de control** , configura las opciones necesarias y luego haz clic en **Guardar** para guardar configuración de la pestaña actual. Las opciones disponibles varían según la generación de la máquina virtual. Además, no se puede cambiar algunas opciones de configuración para ejecutar máquinas virtuales y tendrás que detener la máquina virtual en primer lugar.

## Migración en vivo una máquina virtual a otro nodo de clúster

Si estás conectado a un clúster, dinámicos puede migrar una máquina virtual a otro nodo del clúster.

1. En un clúster de conmutación por error o conexión de clústeres hiperconvergidos, haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **inventario** elige una máquina virtual de la lista y haz clic en **más** > **mover**.
3. Elige un servidor de la lista de nodos de clúster que están disponibles y haga clic en **mover**.
4. Las notificaciones para el progreso de movimiento se mostrará en la esquina superior derecha del centro de administración de Windows. Si el movimiento se realiza correctamente, verás el nombre del servidor Host ha cambiado en la lista de máquinas virtuales.

## Administración avanzada y solución de problemas de una sola máquina virtual

![Pantalla de detalles de la misma máquina virtual](../media/manage-virtual-machines/vm-details.png)

Puedes ver información detallada y gráficos de rendimiento de una sola máquina virtual desde la página de la misma máquina virtual.

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **inventario** . Haz clic en el nombre de una máquina virtual en la lista de máquinas virtuales.
3. En la página de la misma máquina virtual, puedes:
    - Ver información detallada para la máquina virtual.
    - Ver Live y gráficos de línea de datos históricos de CPU, memoria, red, rendimiento IOPS y E/S (datos históricos solo está disponibles para clústeres hiperconvergidos que ejecutan Windows Server 2019)
    - Ver, crear, aplicar, cambiar el nombre y eliminar los puntos de control.
    - Ver los detalles de los archivos de disco duro virtual (VHD), los adaptadores de red y servidor host de la máquina virtual.
    - Eliminar, iniciar, desactivar, apagar, pausar, reanudar, restablecer o cambiar el nombre de la máquina virtual. Guardar la máquina virtual, eliminar un estado guardado o crear un punto de control.
    - [Cambiar la configuración de la máquina virtual](#change-virtual-machine-settings).
    - Conectarse a la consola de máquina virtual con VMConnect mediante el host de Hyper-V.
    - [Replicar la máquina virtual con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## Administrar una máquina virtual a través del host de Hyper-V (VMConnect)

![Máquina virtual se conectan a través de su navegador web](../media/manage-virtual-machines/vm-connect.png)

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **inventario** elige una máquina virtual de la lista y haz clic en **más** > **Connect** o **archivo RDP descargar**. **Conectar** te permitirá interactuar con la máquina virtual de invitado a través de la consola de web de escritorio remoto, integrada en Windows Admin Center. **Archivo RDP descargar** descargará un archivo RDP que se puede abrir con la aplicación de la conexión a Escritorio remoto (mstsc.exe). Ambas opciones usarán VMConnect para conectarse a la máquina virtual de invitado a través del host de Hyper-V y requieren escribir las credenciales de administrador para el servidor de host de Hyper-V.

## Cambiar la configuración de host de Hyper-V

![Pantalla de configuración de host de Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. En una conexión de servidores, clústeres hiperconvergidos o clúster de conmutación por error, haz clic en el menú de **configuración** en la parte inferior del panel de navegación izquierda.
2. En un clúster o servidor de host de Hyper-V, verás un grupo de **Configuración del Host de Hyper-V** con las siguientes secciones:
    - General: Cambio virtual discos duros y las máquinas virtuales ruta del archivo y el hipervisor tipo de programación (si se admite)
    - Modo de sesión mejorada
    - Que abarcan NUMA
    - Migración en vivo
    - Migración de almacenamiento
3. Si realizas cambios en la configuración de una conexión de clúster de conmutación por error o de clústeres hiperconvergidos cualquier host de Hyper-V, el cambio se aplicará a todos los nodos del clúster.

## Ver los registros de eventos de Hyper-V

Puedes ver los registros de eventos de Hyper-V directamente desde la herramienta de máquinas virtuales.

1. Haz clic en la herramienta de **máquinas virtuales** desde el panel de navegación izquierda.
2. En la parte superior de la herramienta de máquinas virtuales, elige la pestaña de **Resumen** . En la sección eventos superior derecha, haz clic en **Todos los eventos de vista**.
3. La herramienta Visor de eventos mostrará los canales de evento de Hyper-V en el panel izquierdo. Elegir un canal para ver los eventos en el panel derecho. Si va a administrar un clúster de conmutación por error o un clúster hiperconvergido, los registros de eventos muestra eventos para todos los nodos del clúster, el servidor host se muestra en la columna de la máquina.

## Proteger las máquinas virtuales con Azure Site Recovery

Puedes usar Windows Admin Center para configurar Azure Site Recovery y replicar las máquinas virtuales de local a Azure. [Más información](../azure/azure-site-recovery.md)

## Más próximas

Administración de máquinas virtuales en Windows Admin Center está activamente en desarrollo y se agregarán nuevas características en un futuro cercano. Puedes ver el estado y el voto de características en UserVoice:

|Solicitud de característica|
|-------|
|[Importar o exportar las máquinas virtuales](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[Ordenar las máquinas virtuales en carpetas](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[Configuración de máquina virtual adicional de soporte técnico](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[Soporte de réplica de Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[Delegar la propiedad de la máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[Clona la máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[Crear una plantilla desde una máquina virtual existente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[Ver las máquinas virtuales en hosts de Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[Configurar VLAN en el panel de la nueva máquina Virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**Ver todos o Proponer nueva característica**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
