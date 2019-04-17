---
title: "Realizar tareas posteriores a la migración de Windows Server Essentials migration1"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 535a547ded55cb4afc0942259eadf5222a815274
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Realizar tareas posteriores a la migración de Windows Server Essentials migration1

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Las siguientes tareas te ayudan a terminar de configurar el servidor de destino con algunos de los mismos valores que se encontraban en el servidor de origen. Se pueden haber deshabilitado algunas de estas opciones en el servidor de origen durante el proceso de migración, por lo que no se migraron al servidor de destino. O bien, son los pasos de configuración opcional que quieras realizar.  
  

-   [Eliminar entradas DNS del servidor de origen](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartir line-of-business y otras carpetas de datos de aplicación](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Solucionar problemas del equipo de cliente después de migrar](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Otorgar al grupo integrado Administradores el derecho a iniciar sesión como trabajo por lotes](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Eliminar entradas DNS del servidor de origen](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartir line-of-business y otras carpetas de datos de aplicación](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Solucionar problemas del equipo de cliente después de migrar](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Otorgar al grupo integrado Administradores el derecho a iniciar sesión como trabajo por lotes](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a>Eliminar entradas DNS del servidor de origen  
 Después de retirar el servidor de origen, el servidor del servicio de nombres de dominio (DNS) aún puede contener entradas que apuntan al servidor de origen. Eliminar estas entradas DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para eliminar entradas DNS que apuntan al servidor de origen  
  
1.  En el servidor de destino, abre **Administrador de DNS**.  
  
2.  En el Administrador de DNS, haz clic en el nombre del servidor, haz clic en **propiedades**y, a continuación, haz clic en el **servidores de reenvío** pestaña.  
  
3.  Determinar si hay una entrada en la lista de reenviador que apunta al servidor de origen. Si no existe, haz clic en **editar**y, a continuación, elimine esa entrada en el **editar servidores de reenvío** ventana.  
  
4.  En **Administrador de DNS**, expande el nombre del servidor y, a continuación, expanda **zonas de búsqueda directa**.  
  
5.  Para cada zona de búsqueda directa, en la zona, haz clic **propiedades**y, a continuación, haz clic en el **servidores de nombres** pestaña.  
  
6.  Haz clic en una entrada en el **servidores de nombres** cuadro que señala al servidor de origen, haz clic en **quitar**y, a continuación, haz clic en **Aceptar**.  
  
7.  Repite los pasos 5 y 6 hasta que se han quitado todos los punteros al servidor de origen.  
  
8.  Haz clic en **Aceptar** para cerrar la **propiedades** ventana.  
  
9. En la **Administrador de DNS** de consola, expande **zonas de búsqueda inversa**.  
  
10. Repite los pasos 6 a 9 para quitar todas las zonas de búsqueda inversa que apuntan al servidor de origen.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Compartir line-of-business y otras carpetas de datos de aplicación  
 Debes establecer los permisos de carpetas compartidas y los permisos NTFS para la línea de negocios y otras carpetas de datos de aplicación que has copiado en el servidor de destino. Después de configurar los permisos, se muestran las carpetas compartidas en el escritorio de Windows Server Essentials en el **almacenamiento** sección.  
  
 Si estás usando un script de inicio de sesión para asignar unidades a las carpetas compartidas, debes actualizar el script se asignan a las unidades en el servidor de destino.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Solucionar problemas del equipo de cliente después de migrar  
 Si se migra a Windows Server Essentials de Windows Small Business Server 2003 Premium Edition con Microsoft Internet Security y Acceleration (ISA) Server instalado, los equipos cliente en la red aún tienen el cliente de Firewall de Microsoft y configurar Internet Explorer para usar un servidor proxy.  
  
 Esto hace que los problemas de conectividad en el cliente equipos, porque ya no existe el servidor proxy. Si hay un servidor proxy diferentes configurado, los equipos cliente seguirán usando el servidor que ejecuta Windows SBS 2003 del servidor proxy. Para solucionar este problema, debe volver a configurar Internet Explorer para no usar un servidor proxy o para usar el nuevo servidor proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Volver a configurar Internet Explorer  
  
1.  En Internet Explorer, haga clic en **herramientas**y, a continuación, haz clic en **opciones de Internet**.  
  
2.  Haz clic en el **conexiones**, haga clic **Configuración LAN**, y, a continuación, realiza una de las siguientes acciones:  
  
    -   Si no usas un servidor proxy en la red, desactiva las casillas de verificación en el **configuración de red de área Local (LAN)** cuadro de diálogo.  
  
    -   Si quieres usar un servidor proxy nuevo en la red:  
  
        1.  En la **configuración de red de área Local (LAN)** cuadro de diálogo, desactive las casillas de verificación en el **configuración automática** sección.  
  
        2.  En la **servidor Proxy** sección, compruebe que están activadas las casillas de verificación.  
  
        3.  En la **dirección**, escriba el nombre de dominio completo (FQDN) del servidor proxy.  
  
        4.  En la **puerto**, escriba **80**.  
  
3.  Haz clic en **Aceptar** dos veces.  
  
4.  Vaya a un sitio Web para asegurarte de que la configuración de conexión es correcta.  
  
##  <a name="BKMK_AdminGroup"></a>Otorgar al grupo integrado Administradores el derecho a iniciar sesión como trabajo por lotes  
 Después de migrar un dominio de Windows Small Business Server 2003 existente a Windows Server Essentials, debes ofrecer al grupo integrado Administradores el derecho de iniciar sesión como trabajo por lotes. Comprueba que el grupo integrado Administradores aún tiene el derecho de iniciar sesión como trabajo por lotes al servidor de destino. Los administradores necesitan este derecho para ejecutar una alerta en el servidor de destino sin iniciar sesión.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Para dar integrado el derecho de iniciar sesión como trabajo por lotes de grupo de administradores  
  
1.  En el servidor de destino, abre el **Group Policy Management** herramienta administrativa.  
  
2.  En la **Group Policy Management** árbol de consola, expande **bosque:***< ServerName\ >*, dominios y, a continuación, el servidor.  
  
3.  Expande **controladores de dominio**, haz clic en **directiva predeterminada de controladores de dominio**y, a continuación, haz clic en **editar**.  
  
4.  En **el Editor de administración de directivas de grupo**, haz clic en **directiva predeterminada de controladores de dominio***< ServerName\ >***directiva**y después expande **configuración del equipo**.  
  
5.  Expande **directivas**, expanda **configuración de Windows**y después expande **la configuración de seguridad**.  
  
6.  En la **la configuración de seguridad** árbol, expanda **directivas locales**y, a continuación, haz clic en **asignación de derechos de usuario**.  
  
7.  En el panel de resultados, haz clic en **iniciar sesión como trabajo por lotes**y, a continuación, haz clic en Propiedades.  
  
8.  En la **iniciar sesión como trabajo por lotes propiedades** página, haz clic en **Agregar usuario o grupo**.  
  
9. En la **Agregar usuario o grupo** cuadro de diálogo, haz clic en **examinar**.  
  
10. En la **Seleccionar usuarios, equipos o grupos** cuadro de diálogo, escribe **administradores**.  
  
11. Haz clic en **comprobar nombres** para comprobar que el grupo integrado Administradores aparece y, a continuación, haz clic en **Aceptar** tres veces para guardar la configuración.  
  
## <a name="see-also"></a>Consulta también  
  

-   [Migrar de Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar los datos del servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar los datos del servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

