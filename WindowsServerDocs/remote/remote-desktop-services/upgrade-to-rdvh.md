---
title: Actualizar el Host de virtualización de escritorio remoto a Windows Server 2016
description: En este artículo se describe cómo actualizar las implementaciones existentes de servicios de escritorio remoto a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 17bf3a49155d29960684acebea870c0b51f664a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812036"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Actualizar el Host de virtualización de escritorio remoto a Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Admite actualizaciones del sistema operativo con instalado el rol RDS
Se admiten actualizaciones a Windows Server 2016 solo desde Windows Server 2012 R2 y Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Servidores de Host de virtualización de escritorio remoto en la implementación de máquinas virtuales que se almacenan localmente
Estos servidores deben actualizarse a la vez. Siga los pasos siguientes para actualizar:

1. Cerrar todos los usuarios.
1. Activar o desactivar guardar todas las máquinas virtuales en cada host. 
1. Actualizar los servidores a Windows Server 2016. 
1. Todas las colecciones deberían estar disponibles y funcional, una vez completadas las actualizaciones.      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Servidores de Host de virtualización de escritorio remoto en la implementación de máquinas virtuales que se almacenan en volúmenes compartidos de clúster (CSV) 

1. Determinar una estrategia de actualización donde algunos de los servidores RDVH se actualizarán y algunos continuará para hospedar máquinas virtuales en Windows Server 2012 R2.  
1. Aísle uno o varios de los servidores RDVH, destinados para la Ronda inicial de la actualización, si migra todas las máquinas virtuales a otro ' no se puede actualizar aún' servidores RDVH que continuarán formando parte del clúster original 2012 R2.
    1. Abra el Administrador de clústeres de conmutación por error. 
    1. Haz clic en **Roles**. 
    1. Seleccione una o más máquinas virtuales. Haga doble clic para abrir el menú contextual. 
    1. Haga clic en **mover** y elija **Live** o **migración rápida** para mover las máquinas virtuales a uno o varios de los servidores de Host de virtualización de escritorio remoto que no forman parte de la actualización inicial. Use **Live** o **rápido** migración según factores como la compatibilidad de hardware o los requisitos en línea. 
1. Expulsar a los servidores RDVH, preparados para la actualización del clúster original. 
1. Actualizar los servidores RDVH aislados. 
1. Después de actualizar correctamente los servidores de destino RDVH, cree un nuevo clúster y CSV, que debe estar en un volumen SAN completamente diferente.
1. Únase a todos los servidores RDVH actualizados al nuevo clúster. 
1. Crear una estructura de carpetas en el nuevo archivo CSV que imita la estructura de carpetas existente en el archivo CSV existente. Esto incluye las carpetas de colecciones y las subcarpetas de nivel superiores para la todas las máquinas virtuales. 
1. De las distintas carpetas de recopilación de VM en el archivo CSV original, copiar la carpeta /IMGS y el contenido en las nuevas carpetas de la colección en las mismas ubicaciones en el nuevo CSV. 
1. En el equipo RDVH de origen, use el Administrador de clústeres para quitar la configuración de la máquina virtual de alta disponibilidad:
    1. Launch Cluster Manager. 
    1. Haz clic en **Roles**. 
    1. Haga clic en los objetos de máquina virtual y, a continuación, haga clic en **quitar**. 
1. En uno de los servidores RDVH que no está actualizado, utilice el Administrador de Hyper-V para mover todas las máquinas virtuales a uno de los servidores RDVH actualizados y el nuevo clúster de CSV:
    1. Abre el Administrador Hyper-V. 
    1. Seleccione uno de los servidores RDVH que no está actualizado. 
    1. Haga clic en uno de las máquinas virtuales que desea mover y, a continuación, haga clic en **mover**. 
    1. Elija **mover la máquina virtual**y, a continuación, haga clic en **siguiente**. 
    1. Proporcione el nombre de destino actualizado RDVH del servidor en el **especificar equipo de destino** página y, a continuación, haga clic en **siguiente**. 
    1. Elija **mover los datos de la máquina virtual a una sola ubicación**y, a continuación, haga clic en **siguiente**. 
    1. Vaya a la ubicación de destino. 
    > [!IMPORTANT]
    > Asegúrese de que esta ruta de acceso es una carpeta vacía para la máquina virtual específica. 

    > [!NOTE]
    > Como se mencionó, deberá ya ha creado una subcarpeta de destino antes de este paso. El cuadro de diálogo Seleccionar la carpeta, no podrá crear una subcarpeta en este paso. 
    
    Haz clic en **Siguiente** y, después, en **Finalizado**. 
1. Una vez que se reubican las máquinas virtuales, puede agregarlas como clúster **alta disponibilidad** objetos:
    1. Abra el Administrador de clústeres de conmutación por error en un servidor de Host de virtualización de escritorio remoto actualizado. 
    1. Haga clic en el **Roles** nodo y, a continuación, haga clic en **configurar rol**. Haga clic en **siguiente** en el **iniciar** página del Asistente para alta disponibilidad. 
    1. Elija **Máquina Virtual** en la lista de roles disponibles y, a continuación, haga clic en **siguiente**. Se mostrará una lista de máquinas virtuales que no están configurados. 
    1. Seleccione todas las máquinas virtuales. Haga clic en **siguiente** y, a continuación, haga clic en **siguiente** nuevo en la página de confirmación para iniciar la tarea de configuración.  
1. Una vez que se han reubicado todas las máquinas virtuales, actualizar los servidores RDVH restantes. Utilice los pasos anteriores para el equilibrio de las ubicaciones de la máquina virtual según corresponda.

> [!NOTE]  
> No se admiten servidores de Hyper-V heterogéneos en un clúster. 
