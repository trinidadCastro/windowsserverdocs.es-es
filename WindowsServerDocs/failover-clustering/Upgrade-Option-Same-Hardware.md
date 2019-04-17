---
title: Actualización de clústeres de conmutación por error con el mismo Hardware
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: En este artículo se describe la actualización de un clúster de conmutación por error de 2 nodos con el mismo hardware
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 2c2027b597e2483eea8967d0710d65c2247b6751
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "9121324"
---
# Actualización de clústeres de conmutación por error en el mismo hardware

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error es un grupo de ordenadores independientes que trabajan juntos para aumentar la disponibilidad de aplicaciones y servicios. Los servidores en clústeres (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno de los nodos del clúster, otro nodo comienza a prestar servicio (un proceso conocido como conmutación por error). Los usuarios experimentan una cantidad mínima de interrupciones del servicio.

Esta guía describe los pasos para actualizar los nodos del clúster a Windows Server 2019 o Windows Server 2016 desde una versión anterior con el mismo hardware.

## Introducción

Actualización del sistema operativo en una conmutación por error existente clúster solo se admite al pasar de Windows Server 2016 para Windows 2019.  Si el clúster de conmutación por error se está ejecutando una versión anterior, tales como Windows Server 2012 R2 y versiones anteriores, actualizar mientras se ejecutan los servicios de clúster no permite unir nodos.  Si usas el mismo hardware, los pasos pueden realizarse acceder a ella a la versión más reciente.  

Antes de realizar cualquier actualización de su clúster de conmutación por error, consulte el [Centro de la actualización de Windows](https://www.microsoft.com/upgradecenter).  Cuando se actualiza una servidor de Windows en contexto, puedes mover de una versión de sistema operativo existente a una versión más reciente sin cambiar en el mismo hardware. Windows Server puede ser actualizada en contexto al menos uno y a veces, dos versiones hacia delante. Por ejemplo, Windows Server 2012 R2 y Windows Server 2016 pueden actualizarse en contexto a Windows Server 2019.  También Ten en cuenta que puede usarse el [Asistente para la migración de clúster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) , pero solo se admite hasta dos versiones atrás. El gráfico siguiente muestra las rutas de actualización para Windows Server. Hacia abajo flechas señaladores representan la ruta de actualización admitida mover desde versiones anteriores hasta Windows Server 2019.

![Diagrama de actualización en contexto](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

Los siguientes pasos son un ejemplo de pasar de un servidor de clúster de conmutación por error de Windows Server 2012 a Windows Server 2019 con el mismo hardware.  

Antes de iniciar cualquier actualización, asegúrese que se ha realizado una copia de seguridad actual, incluyendo el estado del sistema.  Asegúrate también de todos los controladores y firmware se han actualizado a los niveles de certificados para el sistema operativo que se va a utilizar.  Estos dos notas no se explicarán aquí.

En el siguiente ejemplo, el nombre del clúster de conmutación por error es el clúster y los nombres de nodo son nodo 1 y nodo 2.

## Paso 1: Expulsar el primer nodo y actualizar a Windows Server 2016

1. En el Administrador de clústeres de conmutación por error, consumir todos los recursos de nodo 1 al nodo 2 por derecho del ratón al hacer clic en el nodo y seleccionando **Pausar** y **Purgar Roles**.  Como alternativa, puedes usar el comando de PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Nodo de vaciado](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. Expulsar nodo 1 del clúster por secundario del mouse haciendo clic en el nodo y seleccionando **Más acciones** y **Expulsar**.  Como alternativa, puedes usar el comando de PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Nodo de vaciado](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. Como precaución, desasociación nodo 1 desde el almacenamiento que estás usando.  En algunos casos, será suficiente desconectar los cables de almacenamiento de la máquina.  Ponte en contacto con el proveedor de almacenamiento para conocer los pasos de desasociación adecuada si es necesario.  Según el almacenamiento, esto puede no ser necesario.

4. Recompila el nodo 1 con Windows Server 2016.  Asegúrate de que hayas agregado todos los roles necesarios, características, controladores y actualizaciones de seguridad.

5. Crear un nuevo clúster denominado CLUSTER1 con el nodo 1.  Abre el Administrador de clústeres de conmutación por error y en el panel de **administración** , elegir **Crear el clúster** y sigue las instrucciones del asistente.

    ![Nodo de vaciado](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. Una vez creado el clúster, los roles que se migrar desde el clúster original a este nuevo clúster.  En el nuevo clúster, derecho ratón, haz clic en el nombre de clúster (CLUSTER1) y seleccionar **Más acciones** y **Los Roles de clúster de copia**.  Siga a lo largo del Asistente para migrar los roles.

    ![Nodo de vaciado](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  Una vez que se hayan migrado todos los recursos, se apague nodo 2 (clúster original) y desconecta el almacenamiento con el fin de no hacer que las interferencias.  Conecta el almacenamiento al nodo 1.  Una vez que todo está conectado, poner todos los recursos en línea y asegúrate de que funcionan como deberían.

## Paso 2: Volver a compilar el segundo nodo a Windows Server 2019

Una vez que hayas comprobado que todo funciona como debe, nodo 2 puede volver a compilar a Windows Server 2019 y se une al clúster.

1. Realizar una instalación limpia de Windows Server 2019 en el nodo 2. Asegúrate de que hayas agregado todos los roles necesarios, características, controladores y actualizaciones de seguridad.

2. Ahora que se quite el clúster original (clúster), puedes dejar el nuevo nombre de clúster como CLUSTER1 o volver al nombre original.  Si deseas volver al nombre original, sigue estos pasos:
   
   a. En el nodo 1, en el Administrador de clústeres de conmutación por error derecho ratón, haz clic en el nombre del clúster (CLUSTER1) y elige **las propiedades**.
   
   b. En la pestaña **General** , cambia el nombre del clúster a clúster.

   c. Al elegir Aceptar o aplicar, verás el siguiente elemento emergente del cuadro de diálogo.

    ![Nodo de vaciado](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. Detendrá y debía puede iniciar de nuevo para que cambiar el nombre completar el servicio de clúster.

3. En el nodo 1, abre el Administrador de clústeres de conmutación por error.  Haga clic con el mouse en los **nodos** y selecciona **Agregar nodo**.  Ve a través del Asistente para agregar nodo 2 al clúster.

4. Adjunta el almacenamiento para el nodo 2. Esto podría incluir volver a conectar los cables de almacenamiento. 

5. Consumir todos los recursos de nodo 1 al nodo 2 por derecho del ratón al hacer clic en el nodo y seleccionando **Pausar** y **Purgar Roles**.  Como alternativa, puedes usar el comando de PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Asegúrate de que todos los recursos estén en línea y funcionan como deberían.

## Paso 3: Volver a compilar el primer nodo a Windows Server 2019

1. Expulsar nodo 1 del clúster y desconectar el almacenamiento desde el nodo de la manera en que anteriormente.

2. Volver a compilar o actualizar nodo 1 a Windows Server 2019.  Asegúrate de que hayas agregado todos los roles necesarios, características, controladores y actualizaciones de seguridad.

3. Volver a conectar el almacenamiento y agregar nodo 1 al clúster.

4. Mover todos los recursos al nodo 1 y asegúrate de que se conectan y funcionan según sea necesario.

5. El nivel funcional del clúster permanece en Windows 2016.  Actualizar el nivel funcional a Windows 2019 con el comando de PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Ahora se ejecuta con un clúster de conmutación por error de Windows Server 2019 totalmente funcional.

## Notas adicionales

- Como se explicó anteriormente, desconectar el almacenamiento puede o no puede ser necesario.  En nuestra documentación, queremos pensar precaución.  Póngase en contacto con el proveedor de almacenamiento.
- Si el punto de partida es Windows Server 2008 o 2008 R2 clústeres, puede ser necesarias una ejecución adicional a través de pasos.
- Si el clúster es ejecutar máquinas virtuales, asegúrate de que actualizar el nivel de la máquina virtual, una vez que se ha realizado el nivel funcional del clúster con el comando de PowerShell [VMVERSION de actualización](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Ten en cuenta que si se ejecuta una aplicación, como SQL Server, Exchange Server, etcetera, la aplicación no se migran con el Asistente de Roles de clúster de copia.  Debes consultar el proveedor de la aplicación para conocer los pasos de migración correcta de la aplicación.