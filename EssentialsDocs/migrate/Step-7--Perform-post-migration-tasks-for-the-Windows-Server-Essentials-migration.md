---
title: "Paso 7: Realizar tareas posteriores a la migración de la migración de Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6a61d28f29097bcb6993a471587f4cc1ae0bcc3f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Paso 7: Realizar tareas posteriores a la migración de la migración de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Las siguientes tareas te ayudan a terminar de configurar el servidor de destino con algunos de los mismos valores que se encontraban en el servidor de origen. Se pueden haber deshabilitado algunas de estas opciones en el servidor de origen durante el proceso de migración, por lo que no se migraron al servidor de destino.  
  
1.  [Eliminar entradas DNS para el servidor de origen](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Compartir line-of-business y otras carpetas de datos de aplicación](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a>Eliminar entradas DNS para el servidor de origen  
 Después de retirar el servidor de origen, el servidor del servicio de nombres de dominio (DNS) aún puede contener entradas que apuntan al servidor de origen. Eliminar estas entradas DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para eliminar entradas DNS que apuntan al servidor de origen  
  
1.  En el servidor de destino, abre **Administrador de DNS**.  
  
2.  En el Administrador de DNS, haz clic en el nombre del servidor, haz clic en **propiedades**y, a continuación, haz clic en el **servidores de reenvío** pestaña.  
  
3.  Determinar si hay una entrada en la lista de reenviador que apunta al servidor de origen. Si no existe, haz clic en **editar**y, a continuación, elimine esa entrada en el **editar servidores de reenvío** ventana.  
  
4.  En **Administrador de DNS**, expande el nombre del servidor y, a continuación, expanda **zonas de búsqueda directa**.  
  
5.  Para cada zona de búsqueda directa, en la zona, haz clic **propiedades**y, a continuación, haz clic en el **servidores de nombres** pestaña.  
  
6.  Selecciona una entrada en el **servidores de nombres** cuadro de texto que señala al servidor de origen, haz clic en **quitar**y, a continuación, haz clic en **Aceptar**.  
  
7.  Repite los pasos 5 y 6 hasta que se han quitado todos los punteros al servidor de origen.  
  
8.  Haz clic en **Aceptar** para cerrar la **propiedades** ventana.  
  
9. En **Administrador de DNS**, expanda **zonas de búsqueda inversa**.  
  
10. Repite los pasos 6 a 9 para quitar todas las zonas de búsqueda inversa que apuntan al servidor de origen.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Compartir line-of-business y otras carpetas de datos de aplicación  
 Debes establecer los permisos de carpetas compartidas y los permisos NTFS para la línea de negocios y otras carpetas de datos de aplicación que has copiado en el servidor de destino. Después de configurar los permisos, se muestran las carpetas compartidas en el panel en el **almacenamiento** pestaña.  
  
 Si estás usando un script de inicio de sesión para asignar unidades a las carpetas compartidas, debes actualizar el script se asignan a las unidades en el servidor de destino.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Realizar las tareas posteriores a la migración para la migración de Windows Server Essentials. Ahora ve a [paso 8: ejecutar el analizador de procedimientos recomendados de Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

