---
title: Actualización de clústeres de conmutación por error con el mismo Hardware
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Este artículo describe la actualización de un clúster de conmutación por error de 2 nodos con el mismo hardware
ms.localizationpriority: medium
ms.openlocfilehash: 77cde9e64fda385facd91d86483f4d7f749f30a1
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453052"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>Actualización de clústeres de conmutación por error en el mismo hardware

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad de las aplicaciones y los servicios. Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno de los nodos del clúster, otro comienza a dar servicio (proceso que se denomina conmutación por error). Los usuarios experimentarán un número de interrupciones mínimo en el servicio.

Esta guía describe los pasos para actualizar los nodos del clúster a Windows Server 2019 o Windows Server 2016 desde una versión anterior con el mismo hardware.

## <a name="overview"></a>Información general

Actualizar el sistema operativo de una conmutación por error existente clúster solo se admite al pasar de Windows Server 2016 para Windows de 2019.  Si el clúster de conmutación por error se está ejecutando una versión anterior, tal como Windows Server 2012 R2 y versiones anteriores, la actualización mientras se ejecutan los servicios de cluster Server no permite unir los nodos.  Si usa el mismo hardware, se pueden realizar pasos para dirigirlo a la versión más reciente.  

Antes de realizar cualquier actualización del clúster de conmutación por error, consulte el [centro de actualización de Windows](https://www.microsoft.com/upgradecenter).  Al actualizar un servidor de Windows local, se mueve de una versión de sistema operativo existente a una versión más reciente en el mismo hardware. Windows Server puede ser actualizado en contexto al menos uno y a veces dos versiones al día. Por ejemplo, se pueden actualizar Windows Server 2012 R2 y Windows Server 2016 en contexto a Windows Server 2019.  También tenga en cuenta que el [Asistente para migración de clúster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) pueden utilizarse, pero solo se admite hasta dos versiones de vuelta. El gráfico siguiente muestra las rutas de actualización para Windows Server. Flechas hacia abajo de señaladores representan la ruta de actualización admitida mover desde versiones anteriores hasta Windows Server 2019.

![Diagrama de actualización en contexto](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

Los pasos siguientes son un ejemplo de pasar de un servidor de clúster de conmutación por error de Windows Server 2012 para Windows Server 2019 usa el mismo hardware.  

Antes de iniciar cualquier actualización, asegúrese de se ha realizado una copia de seguridad actual, incluido el estado del sistema.  Asegúrese también de todos los controladores y firmware se han actualizado a los niveles de certificados para el sistema operativo que se va a usar.  Estas dos notas no se tratará aquí.

En el ejemplo siguiente, el nombre del clúster de conmutación por error es el clúster y los nombres de nodo son Nodo1 y Nodo2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Paso 1: Expulsar el primer nodo y actualizar a Windows Server 2016

1. En el Administrador de clústeres de conmutación por error, purgar todos los recursos del Nodo1 al Nodo2 por secundario del mouse al hacer clic en el nodo y seleccione **pausar** y **purgar Roles**.  Como alternativa, puede usar el comando de PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Expulse el Nodo1 del clúster por secundario del mouse al hacer clic en el nodo y seleccione **más acciones** y **expulsar**.  Como alternativa, puede usar el comando de PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Como medida de precaución, desasociar el almacenamiento que se usen en NODE1.  En algunos casos, será suficiente la desconexión de los cables de almacenamiento de la máquina.  Póngase en contacto con su proveedor de almacenamiento para los pasos de desasociación adecuado si es necesario.  Dependiendo de su almacenamiento, esto puede no ser necesario.

4. Vuelva a generar NODE1 con Windows Server 2016.  Asegúrese de que ha agregado todos los roles necesarios, características, controladores y las actualizaciones de seguridad.

5. Crear un nuevo clúster denominado clúster1 con NODE1.  Abra el Administrador de clústeres de conmutación por error y, en el **administración** panel, elija **crear clúster** y siga las instrucciones del asistente.

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Una vez creado el clúster, necesitará los roles se migrado desde el clúster original a este nuevo clúster.  En el nuevo clúster, botón secundario del mouse haga clic en el nombre del clúster (CLUSTER1) y seleccione **más acciones** y **copiar Roles de clúster**.  Seguir el tutorial del Asistente para migrar los roles.

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Una vez que se han migrado todos los recursos, apague Nodo2 (clúster original) y desconecte el almacenamiento con el fin de no hacer que las interferencias.  Conectar el almacenamiento al Nodo1.  Una vez que todo está conectado, poner en línea todos los recursos y asegúrese de que funcionan como deben.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Paso 2: Volver a generar el segundo nodo para Windows Server 2019

Cuando haya comprobado que todo funciona como debería, se puede volver a compilar en Windows Server 2019 Nodo2 y unirse al clúster.

1. Realizar una instalación limpia de Windows Server 2019 en Nodo2. Asegúrese de que ha agregado todos los roles necesarios, características, controladores y las actualizaciones de seguridad.

2. Ahora que ha desaparecido el clúster original (clúster), puede dejar el nuevo nombre de clúster como CLUSTER1 o volver al nombre original.  Si desea volver al nombre original, siga estos pasos:
   
   a. En el Nodo1, en Administrador de clústeres de conmutación por error secundario del mouse, haga clic en el nombre del clúster (CLUSTER1) y elija **propiedades**.
   
   b. En el **General** pestaña, cambie el nombre del clúster al clúster.

   c. Al elegir Aceptar o aplicar, verá el siguiente mensaje emergente del cuadro de diálogo.

    ![Nodo de purga](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. Se ha detenido el servicio de clúster y era necesario volver a iniciar para que completar el cambio de nombre.

3. En el Nodo1, abra el Administrador de clústeres de conmutación por error.  Haga clic derecho del ratón en **nodos** y seleccione **agregar nodo**.  Use el asistente Agregar Nodo2 al clúster.

4. Conecte el almacenamiento al Nodo2. Esto puede incluir volver a conectar los cables de almacenamiento. 

5. Purgar todos los recursos del Nodo1 al Nodo2 por secundario del mouse al hacer clic en el nodo y seleccione **pausar** y **purgar Roles**.  Como alternativa, puede usar el comando de PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Asegúrese de todos los recursos están en línea y funcionan como deben.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Paso 3: Volver a generar el primer nodo al servidor de Windows 2019

1. Expulse el Nodo1 del clúster y desconecte el almacenamiento en el nodo de la manera en que anteriormente.

2. Volver a generar o actualizar NODE1 a Windows Server 2019.  Asegúrese de que ha agregado todos los roles necesarios, características, controladores y las actualizaciones de seguridad.

3. Vuelve a conectar el almacenamiento y agregar Nodo1 al clúster.

4. Mover todos los recursos al Nodo1 y asegúrese de que se conectan y función según sea necesario.

5. El nivel funcional del clúster se mantiene en Windows 2016.  Actualice el nivel funcional a Windows 2019 con el comando de PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Ahora se ejecuta con un clúster de conmutación por error de Windows Server 2019 totalmente funcional.

## <a name="additional-notes"></a>Notas adicionales

- Como se explicó anteriormente, desconectar el almacenamiento puede o no es posible que sea necesario.  En la documentación, queremos ser cautos.  Póngase en contacto con su proveedor de almacenamiento.
- Si el punto de partida es Windows Server 2008 o 2008 R2 clústeres, puede ser necesaria una ejecución adicional a través de pasos.
- Si el clúster se está ejecutando las máquinas virtuales, asegúrese de actualizar el nivel de máquina virtual una vez que se ha realizado el nivel funcional del clúster con el comando de PowerShell [actualización VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Tenga en cuenta que, si está ejecutando una aplicación como SQL Server, Exchange Server, etcetera, no se migrará la aplicación con el Asistente para copiar Roles de clúster.  Debe consultar el proveedor de la aplicación para conocer los pasos de migración adecuada de la aplicación.