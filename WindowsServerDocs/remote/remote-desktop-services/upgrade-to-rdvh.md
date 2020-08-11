---
title: Actualización del host de virtualización de Escritorio remoto a Windows Server 2016
description: En este artículo se describe cómo actualizar las implementaciones existentes de Servicios de Escritorio remoto a Windows Server 2016.
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 4260748ada0371e637edef23a579e7253721f4f4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948828"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Actualización del host de virtualización de Escritorio remoto a Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Actualizaciones admitidas del sistema operativo con el rol de RDS instalado
Las actualizaciones a Windows Server 2016 solo se admiten desde Windows Server 2012 R2 y Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Servidores host de virtualización de Escritorio remoto de la implementación en la que las VM se almacenan localmente
Estos servidores deben actualizarse a la vez. Sigue los pasos indicados a continuación para la actualización:

1. Cierra la sesión de todos los usuarios.
1. Desactiva o guarda todas las máquinas virtuales en cada host.
1. Actualiza los servidores a Windows Server 2016.
1. Todas las colecciones deben estar disponibles y funcionales una vez completadas las actualizaciones.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Servidores host de virtualización de Escritorio remoto de la implementación en la que las VM se almacenan localmente en Volúmenes compartidos de clúster (CSV)

1. Determina una estrategia de actualización donde se actualicen algunos de los servidores RDVH y otros sigan hospedando VM en Windows Server 2012 R2.
2. Aísla uno o varios servidores RDVH destinados para la ronda inicial de la actualización mediante la migración de todas las VM a otros servidores RDVH que aún no se vayan a actualizar que seguirán formando parte del clúster original 2012 R2.
    1. Abre el Administrador de clústeres de conmutación por error.
    1. Haz clic en **Roles**.
    1. Selecciona una o varias VM. Haz clic con el botón derecho para abrir el menú contextual.
    1. Haz clic en **Mover** y elige **Migración en vivo** o **Migración rápida** para mover las VM a uno o varios de los servidores host de virtualización de Escritorio remoto que no forman parte de la actualización inicial. Usa la migración **En vivo** o **Rápida** según factores como la compatibilidad de hardware o los requisitos en línea.
3. Desaloja del clúster original los servidores RDVH preparados para la actualización.
4. Actualiza los servidores RDVH aislados.
5. Después de actualizar correctamente los servidores de destino RDVH, crea un nuevo clúster y CSV, que debe estar en un volumen SAN completamente diferente.
6. Une todos los servidores RDVH actualizados al nuevo clúster.
7. Crea una estructura de carpetas en el nuevo archivo CSV que imite la estructura de carpetas existente en el archivo CSV existente. Esto incluye las carpetas de colecciones y las subcarpetas de nivel superior para la todas las VM.
8. De las distintas carpetas de recopilación de VM en el archivo CSV original, copia la carpeta /IMGS y el contenido en las nuevas carpetas de la colección en las mismas ubicaciones en el nuevo CSV.
9. En la máquina RDVH de origen, usa el Administrador de clústeres para quitar la configuración de la VM para alta disponibilidad:
    1. Inicia el Administrador de clústeres.
    1. Haz clic en **Roles**.
    1. Haz clic con el botón derecho en los objetos de la VM y, a continuación, en **Quitar**.
10. En uno de los servidores RDVH que no está actualizado, usa el Administrador de Hyper-V para mover todas las VM a uno de los servidores RDVH actualizados y el nuevo clúster de CSV:
    1. Abra el Administrador de Hyper-V.
    2. Selecciona uno de los servidores RDVH que no esté actualizado.
    3. Haz clic con el botón derecho en uno de las VM que quieres mover y, a continuación, haz clic en **Mover**.
    4. Elige **Mover la máquina virtual** y, a continuación, haz clic en **Siguiente**.
    5. Proporciona el nombre del servidor RDVH de destino actualizado en la página **Especificar equipo de destino** y, a continuación, haz clic en **Siguiente**.
    6. Elige **Mover los datos de la máquina virtual a una sola ubicación** y, a continuación, haz clic en **Siguiente**.
    7. Explora la ubicación de destino.
       > [!IMPORTANT]
       > Asegúrate de que esta ruta de acceso lleva a una carpeta vacía para la VM específica.

       > [!NOTE]
       > Como se mencionó, ya deberías haber creado una nueva subcarpeta de destino antes de este paso. El cuadro de diálogo Seleccionar la carpeta no te permitirá crear una subcarpeta en este paso.

       Haz clic en **Siguiente**y, después, en **Finalizado**.
11. Una vez que se han reubicado las VM, puedes agregarlas como objetos de clúster de **alta disponibilidad**:
     1. Abre el Administrador de clústeres de conmutación por error en un servidor host de virtualización de Escritorio remoto actualizado.
     1. Haz clic con el botón derecho en el nodo **Roles** y, a continuación, haz clic en **Configurar rol**. En la página **Inicio** del Asistente para alta disponibilidad, haz clic en **Siguiente**.
     1. Elige **Máquina virtual** en la lista de roles disponibles y, a continuación, haz clic en **Siguiente**. Se mostrará una lista de VM que no están configuradas.
     1. Selecciona todas las VM. Haz clic en **Siguiente** y, a continuación, vuelve a hacer clic en **Siguiente** en la página de confirmación para iniciar la tarea de configuración.
12. Una vez que se han reubicado todas las VM, actualiza los servidores RDVH restantes. Usa los pasos anteriores para equilibrar las ubicaciones de VM según corresponda.

> [!NOTE]
> Los servidores de Hyper-V heterogéneos en un clúster no se admiten.
