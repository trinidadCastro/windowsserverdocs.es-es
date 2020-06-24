---
title: Realizar tareas posteriores a la migración para Windows Server Essentials migration1
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b421ab7b234c9ae2ffb7d0765fe0937c4376996a
ms.sourcegitcommit: 6d6a0225b1f83b71fcb494b94d666cd5e54c7566
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85267526"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Realizar tareas posteriores a la migración para Windows Server Essentials migration1

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Las siguientes tareas le ayudarán a configurar el servidor de destino con algunos de los mismos valores que había en el servidor de origen. Puede que haya deshabilitado algunas de estas opciones en el servidor de origen durante el proceso de migración, por lo que no se han migrado al servidor de destino. O bien son pasos de configuración opcionales que desee realizar.  
  

-   [Eliminar las entradas DNS del servidor de origen](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartir carpetas de datos de la aplicación de negocios y otras aplicaciones](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Corregir problemas de los equipos cliente tras la migración](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Conceder al grupo de administradores integrado el derecho a iniciar sesión como un proceso por lotes](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="delete-dns-entries-of-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a>Eliminar las entradas DNS del servidor de origen  
 Después de retirar el servidor de origen, el servidor del Servicio de nombres de dominio (DNS) todavía puede contener entradas que apunten al servidor de origen. Elimine estas entradas DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para eliminar las entradas DNS que apuntan al servidor de origen  
  
1.  En el servidor de destino, abra el **Administrador de DNS**.  
  
2.  En el Administrador de DNS, haga clic con el botón derecho en el nombre del servidor, haga clic en **Propiedades** y, después, haga clic en la pestaña **Reenviadores**.  
  
3.  Determine si hay una entrada en la lista de reenvío que señala al servidor de origen. Si existe, haga clic en **Editar** y, después, elimine esa entrada en la ventana **Editar reenviadores**.  
  
4.  En el **Administrador de DNS**, expanda el nombre del servidor y, después, expanda **Zonas de búsqueda directa**.  
  
5.  Para cada zona de búsqueda directa, haga clic con el botón derecho en la zona, haga clic en **Propiedades** y, después, haga clic en la pestaña **Servidores de nombres**.  
  
6.  Haga clic en una entrada del cuadro **Servidores de nombres** que apunte al servidor de origen, haga clic en **Quitar**y, después, haga clic en **Aceptar**.  
  
7.  Repita los pasos 5 y 6 hasta que se quiten todos los elementos que apunten al servidor de origen.  
  
8.  Haga clic en **Aceptar** para cerrar la ventana **Propiedades**.  
  
9. En la consola **Administrador de DNS**, expanda **Zonas de búsqueda inversa**.  
  
10. Repita los pasos del 6 al 9 para quitar todas las zonas de búsqueda inversa que apunten al servidor de origen.  
  
##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Compartir las carpetas de datos de la aplicación y de la línea de negocio  
 Debe establecer los permisos de carpetas compartidas y los permisos NTFS de las carpetas de datos de la línea de negocio y otras aplicaciones que copió en el servidor de destino. Después de establecer los permisos, las carpetas compartidas se muestran en el panel de Windows Server Essentials en la sección **almacenamiento** .  
  
 Si usa un script de inicio de sesión para asignar unidades a las carpetas compartidas, debe actualizar el script para realizar la asignación a las unidades de disco en el servidor de destino.  
  
##  <a name="fix-client-computer-issues-after-migrating"></a><a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Corregir problemas del equipo cliente después de la migración  
 Si migra a Windows Server Essentials desde Windows Small Business Server 2003 Premium Edition con Microsoft Internet Security and Acceleration (ISA) Server instalado, los equipos cliente de la red todavía tienen el cliente de Firewall de Microsoft y Internet Explorer configurados para usar un servidor proxy.  
  
 Esto produce problemas de conectividad en los equipos cliente, porque el servidor proxy ya no existe. Si hay configurado un servidor proxy diferente, los equipos cliente seguirán utilizando el servidor que ejecute Windows SBS 2003 para el servidor proxy. Para corregir este problema, debe volver a configurar Internet Explorer para que deje de usar un servidor proxy o para que use el nuevo servidor proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Para volver a configurar Internet Explorer  
  
1.  En Internet Explorer, haga clic en **Herramientas** y **Opciones de Internet**.  
  
2.  Haga clic en la pestaña **Conexiones**, haga clic en **Configuración de LAN** y, después, realice una de las acciones siguientes:  
  
    -   Si no está usando un servidor proxy en la red, desactive las casillas en el cuadro de diálogo **Configuración de red de área local (LAN)**.  
  
    -   Si desea usar un nuevo servidor proxy en la red:  
  
        1.  En el cuadro de diálogo **Configuración de red de área local (LAN)**, desactive las casillas de la sección **Configuración automática**.  
  
        2.  En la sección **Servidor proxy**, compruebe que las casillas estén activadas.  
  
        3.  En el cuadro **Dirección**, escriba el nombre de dominio completo (FQDN) del servidor proxy.  
  
        4.  En el cuadro **Puerto**, escriba **80**.  
  
3.  Haga clic en **Aceptar** dos veces.  
  
4.  Vaya a un sitio web para asegurarse de que la configuración de conexión es correcta.  
  
##  <a name="give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a><a name="BKMK_AdminGroup"></a>Conceda al grupo de administradores integrado el derecho a iniciar sesión como un trabajo por lotes.  
 Después de migrar un dominio existente de Windows Small Business Server 2003 a Windows Server Essentials, debe conceder al grupo de administradores integrado el derecho a iniciar sesión como un trabajo por lotes. Compruebe que el grupo de administradores integrado aún tiene el derecho de iniciar sesión como un proceso por lotes en el servidor de destino. Los administradores necesitan este derecho para ejecutar una alerta en el servidor de destino sin iniciar sesión.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Para dar a la cuenta de grupo de administradores el derecho a iniciar sesión como un proceso por lotes  
  
1. En el servidor de destino, abra la herramienta administrativa **Administración de directivas de grupo**.  
  
2. En el árbol de consola de **Administración de directiva de grupo** , expanda **Forest:** * \><ServerName*, expanda dominios y, a continuación, expanda el servidor.  
  
3. Expanda **Controladores de dominio**, haga clic con el botón derecho en **Directiva predeterminada de controladores de dominio** y, después, haga clic en **Editar**.  
  
4. En **Editor de administración de directivas de grupo**, haga clic en **directiva predeterminada de controladores de dominio** <em><\> </em>**Directiva**de nombreDeServidor y, a continuación, expanda **configuración del equipo**.  
  
5. Expanda **Directivas**, expanda **Configuración de Windows** y, después, expanda **Configuración de seguridad**.  
  
6. En el árbol **Configuración de seguridad**, expanda **Directivas locales** y, después, haga clic en **Asignación de derechos de usuario**.  
  
7. En el panel de resultados, haga clic con el botón derecho en **Iniciar sesión como proceso por lotes** y, después, haga clic en Propiedades.  
  
8. En la página de propiedades de **Iniciar sesión como proceso por lotes**, haga clic en **Agregar usuario o grupo**.  
  
9. En el cuadro de diálogo **Agregar usuario o grupo**, haga clic en **Examinar**.  
  
10. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos**, escriba **Administrators**.  
  
11. Haga clic en **Comprobar nombres** para comprobar que el grupo de administradores integrado aparezca y, después, haga clic en **Aceptar** tres veces para guardar la configuración.  
  
## <a name="see-also"></a>Consulte también  
  

-   [Migrar desde Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar datos del servidor a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

