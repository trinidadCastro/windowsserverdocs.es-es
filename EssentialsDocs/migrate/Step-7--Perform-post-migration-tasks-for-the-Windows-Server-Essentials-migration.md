---
title: 'Paso 7: Realizar tareas postmigración para la migración de Windows Server Essentials'
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b8819b654d05a1e63c7f30b4359cabcc2906c7b8
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180411"
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Paso 7: Realizar tareas postmigración para la migración de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

Las siguientes tareas le ayudarán a configurar el servidor de destino con algunos de los mismos valores que había en el servidor de origen. Puede que haya deshabilitado algunas de estas opciones en el servidor de origen durante el proceso de migración, por lo que no se han migrado al servidor de destino.

1.  [Eliminar las entradas DNS del servidor de origen](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)

2.  [Compartir carpetas de datos de la aplicación de negocios y otras aplicaciones](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)

##  <a name="delete-dns-entries-for-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a>Eliminar entradas DNS para el servidor de origen
 Después de retirar el servidor de origen, el servidor del Servicio de nombres de dominio (DNS) todavía puede contener entradas que apunten al servidor de origen. Elimine estas entradas DNS.

#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para eliminar las entradas DNS que apuntan al servidor de origen

1.  En el servidor de destino, abra el **Administrador de DNS**.

2.  En el Administrador de DNS, haga clic con el botón derecho en el nombre del servidor, haga clic en **Propiedades** y, después, haga clic en la pestaña **Reenviadores**.

3.  Determine si hay una entrada en la lista de reenvío que señala al servidor de origen. Si existe, haga clic en **Editar** y, después, elimine esa entrada en la ventana **Editar reenviadores**.

4.  En el **Administrador de DNS**, expanda el nombre del servidor y, después, expanda **Zonas de búsqueda directa**.

5.  Para cada zona de búsqueda directa, haga clic con el botón derecho en la zona, haga clic en **Propiedades** y, después, haga clic en la pestaña **Servidores de nombres**.

6.  Seleccione una entrada del cuadro de texto **Servidores de nombres** que señale al servidor de origen. Después, haga clic en **Quitar** y en **Aceptar**.

7.  Repita los pasos 5 y 6 hasta que se quiten todos los elementos que apunten al servidor de origen.

8.  Haga clic en **Aceptar** para cerrar la ventana **Propiedades**.

9. En el **Administrador de DNS**, expanda **Zonas de búsqueda inversa**.

10. Repita los pasos del 6 al 9 para quitar todas las zonas de búsqueda inversa que apunten al servidor de origen.

##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Compartir las carpetas de datos de la aplicación y de la línea de negocio
 Debe establecer los permisos de carpetas compartidas y los permisos NTFS de las carpetas de datos de la línea de negocio y otras aplicaciones que copió en el servidor de destino. Después de establecer los permisos, se muestran las carpetas compartidas en el panel, en la pestaña **Almacenamiento**.

 Si usa un script de inicio de sesión para asignar unidades a las carpetas compartidas, debe actualizar el script para realizar la asignación a las unidades de disco en el servidor de destino.

## <a name="next-steps"></a>Pasos siguientes
 Ha realizado las tareas posteriores a la migración para la migración de Windows Server Essentials. Ahora, vaya al [paso 8: ejecutar el analizador de procedimientos recomendados de Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).


Para ver todos los pasos, vea [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

