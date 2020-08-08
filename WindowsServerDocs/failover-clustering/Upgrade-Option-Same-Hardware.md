---
title: Actualización de clústeres de conmutación por error con el mismo hardware
description: En este artículo se describe cómo actualizar un clúster de conmutación por error de 2 nodos con el mismo hardware
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 02/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 3e25660eda2d21658f01fe1d8a01ae86a4b42116
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990949"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>Actualización de clústeres de conmutación por error en el mismo hardware

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad de las aplicaciones y los servicios. Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno de los nodos del clúster, otro comienza a dar servicio (proceso que se denomina conmutación por error). Los usuarios experimentarán un número de interrupciones mínimo en el servicio.

En esta guía se describen los pasos para actualizar los nodos de clúster a Windows Server 2019 o Windows Server 2016 desde una versión anterior con el mismo hardware.

## <a name="overview"></a>Introducción

La actualización del sistema operativo en un clúster de conmutación por error existente solo se admite cuando se pasa de Windows Server 2016 a Windows 2019.  Si el clúster de conmutación por error ejecuta una versión anterior, como Windows Server 2012 R2 y versiones anteriores, la actualización de mientras se ejecutan los servicios de clúster no permitirá unir nodos juntos.  Si usa el mismo hardware, se pueden realizar pasos para obtener la versión más reciente.

Antes de cualquier actualización del clúster de conmutación por error, consulta el [contenido de actualización de Windows Server](../upgrade/upgrade-overview.md).  Cuando se actualiza un servidor de Windows local, se pasa de una versión de sistema operativo existente a una versión más reciente y se mantiene en el mismo hardware. Windows Server se puede actualizar en contexto como mínimo y, a veces, dos versiones hacia delante. Por ejemplo, Windows Server 2012 R2 y Windows Server 2016 se pueden actualizar en contexto a Windows Server 2019.  Tenga en cuenta también que se puede usar el [Asistente para migración de clústeres](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) , pero solo se admiten hasta dos versiones. En el gráfico siguiente se muestran las rutas de actualización para Windows Server. Las flechas que apuntan hacia abajo representan la ruta de actualización admitida que se traslada de versiones anteriores a Windows Server 2019.

![Diagrama de actualización local](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

Los pasos siguientes son un ejemplo de cómo pasar de un servidor de clúster de conmutación por error de Windows Server 2012 a Windows Server 2019 con el mismo hardware.

Antes de iniciar cualquier actualización, asegúrese de que se ha realizado una copia de seguridad actual, incluido el estado del sistema.  Asegúrese también de que todos los controladores y firmware se han actualizado a los niveles certificados del sistema operativo que va a usar.  Estas dos notas no se tratarán aquí.

En el ejemplo siguiente, el nombre del clúster de conmutación por error es CLUSTER y los nombres de nodo son NODO1 y NODO2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Paso 1: expulsar el primer nodo y actualizar a Windows Server 2016

1. En Administrador de clústeres de conmutación por error, vacíe todos los recursos del NODO1 al NODO2 haciendo clic con el botón secundario del mouse en el nodo y seleccionando roles de **pausa** y **purga**.  Como alternativa, puede usar el comando de PowerShell [Suspend-CLUSTERNODE](/powershell/module/failoverclusters/suspend-clusternode).

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Expulse el NODO1 del clúster haciendo clic con el botón secundario en el nodo y seleccionando **más acciones** y **expulsar**.  Como alternativa, puede usar el comando de PowerShell [Remove-CLUSTERNODE](/powershell/module/failoverclusters/remove-clusternode).

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Como medida de precaución, desconecte el NODO1 del almacenamiento que está usando.  En algunos casos, basta con desconectar los cables de almacenamiento del equipo.  Consulte con el proveedor de almacenamiento los pasos adecuados de desasociación si es necesario.  En función del almacenamiento, es posible que no sea necesario.

4. Vuelva a generar el NODO1 con Windows Server 2016.  Asegúrese de que ha agregado todos los roles, las características, los controladores y las actualizaciones de seguridad necesarios.

5. Cree un nuevo clúster llamado CLUSTER1 con el NODO1.  Abra Administrador de clústeres de conmutación por error y, en el panel **Administración** , elija **crear clúster** y siga las instrucciones del asistente.

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Una vez creado el clúster, los roles deberán migrarse desde el clúster original a este nuevo clúster.  En el nuevo clúster, haga clic con el botón derecho en el nombre del clúster (CLUSTER1) y seleccione **más acciones** y **copiar roles de clúster**.  Siga el Asistente para migrar los roles.

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Una vez que se han migrado todos los recursos, apague el NODO2 (clúster original) y desconecte el almacenamiento para que no se produzca ninguna interferencia.  Conecte el almacenamiento al NODO1.  Una vez que todo está conectado, ponga todos los recursos en línea y asegúrese de que funcionan como debiera.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Paso 2: reconstruir el segundo nodo en Windows Server 2019

Una vez que haya comprobado que todo funciona como debería, NODO2 se puede volver a generar en Windows Server 2019 y unirse al clúster.

1. Realice una instalación limpia de Windows Server 2019 en el NODO2. Asegúrese de que ha agregado todos los roles, las características, los controladores y las actualizaciones de seguridad necesarios.

2. Ahora que el clúster original (clúster) ha desaparecido, puede dejar el nombre del nuevo clúster como CLUSTER1 o volver al nombre original.  Si desea volver al nombre original, siga estos pasos:

   a. En el NODO1, en Administrador de clústeres de conmutación por error botón secundario del mouse, haga clic en el nombre del clúster (CLUSTER1) y elija **propiedades**.

   b. En la pestaña **General** , cambie el nombre del clúster a clúster.

   c. Al elegir aceptar o aplicar, verá el cuadro de diálogo emergente siguiente.

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. El servicio de clúster se detendrá y será necesario volver a iniciarlo para que se complete el cambio de nombre.

3. En el NODO1, abra Administrador de clústeres de conmutación por error.  Haga clic con el botón derecho en los **nodos** y seleccione **agregar nodo**.  Siga el asistente agregando NODO2 al clúster.

4. Conecte el almacenamiento al NODO2. Esto podría incluir la reconexión de los cables de almacenamiento.

5. Vacíe todos los recursos del NODO1 al NODO2 haciendo clic con el botón secundario del mouse en el nodo y seleccionando los roles de **pausa** y **purga**.  Como alternativa, puede usar el comando de PowerShell [Suspend-CLUSTERNODE](/powershell/module/failoverclusters/suspend-clusternode).  Asegúrese de que todos los recursos están en línea y funcionan como deberían.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Paso 3: reconstruir el primer nodo en Windows Server 2019

1. Expulse el NODO1 del clúster y desconecte el almacenamiento del nodo en la forma en que se encontraba anteriormente.

2. Vuelva a generar o actualice el NODO1 a Windows Server 2019.  Asegúrese de que ha agregado todos los roles, las características, los controladores y las actualizaciones de seguridad necesarios.

3. Vuelva a adjuntar el almacenamiento y vuelva a agregar el NODO1 al clúster.

4. Mueva todos los recursos al NODO1 y asegúrese de que se conectan y funcionan según sea necesario.

5. El nivel funcional del clúster actual permanece en Windows 2016.  Actualice el nivel funcional a Windows 2019 con el comando [de PowerShell Update-CLUSTERFUNCTIONALLEVEL](/powershell/module/failoverclusters/update-clusterfunctionallevel).

Ahora se está ejecutando con un clúster de conmutación por error de Windows Server 2019 totalmente funcional.

## <a name="additional-notes"></a>Notas adicionales

- Como se explicó anteriormente, la desconexión del almacenamiento puede ser necesaria o no.  En nuestra documentación, queremos tener la precaución.  Póngase en contacto con su proveedor de almacenamiento.
- Si el punto de partida es un clúster de Windows Server 2008 o 2008 R2, puede que sea necesario realizar una serie de pasos adicionales.
- Si el clúster ejecuta máquinas virtuales, asegúrese de actualizar el nivel de máquina virtual una vez que se haya realizado el nivel funcional del clúster con el comando [de PowerShell Update-VMVERSION](/powershell/module/hyper-v/update-vmversion).
- Tenga en cuenta que si está ejecutando una aplicación como SQL Server, Exchange Server, etc., la aplicación no se migrará con el Asistente para copiar roles de clúster.  Debe consultar al proveedor de la aplicación para ver los pasos de migración adecuados de la aplicación.