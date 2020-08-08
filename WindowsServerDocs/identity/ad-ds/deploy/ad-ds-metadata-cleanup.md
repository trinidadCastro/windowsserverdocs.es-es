---
title: Limpiar metadatos de AD DS Server
description: Usar herramientas integradas para limpiar metadatos de controladores de dominio eliminados
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.openlocfilehash: 9601ead1621aef187aaf6dfed83e31184e61e0d4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953290"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Limpiar metadatos de servidor del controlador de Dominio de Active Directory

Se aplica a: Windows Server

La limpieza de metadatos es un procedimiento necesario después de una eliminación forzada de Active Directory Domain Services (AD DS). La limpieza de los metadatos se realiza en un controlador de dominio del dominio del controlador de dominio que se ha quitado de manera forzada. La limpieza de metadatos quita datos de AD DS que identifica un controlador de dominio en el sistema de replicación. La limpieza de metadatos también quita las conexiones de replicación del servicio de replicación de archivos (FRS) y Sistema de archivos distribuido (DFS) e intenta transferir o asumir las funciones de maestro de operaciones (también conocidas como operaciones de maestro único flexible o FSMO) que contiene el controlador de dominio retirado.

Hay tres opciones para limpiar los metadatos del servidor:

- Limpiar metadatos de servidor mediante herramientas de GUI
- Limpiar metadatos de servidor mediante la línea de comandos
- Limpiar metadatos de servidor mediante un script

> [!NOTE]
> Si recibe un error de "acceso denegado" cuando use cualquiera de estos métodos para realizar la limpieza de metadatos, asegúrese de que el objeto de equipo y el objeto de configuración NTDS para el controlador de dominio no están protegidos contra la eliminación accidental. Para comprobarlo, haga clic con el botón secundario en el objeto equipo o en el objeto configuración NTDS, haga clic en **propiedades**, haga clic en **objeto**y desactive la casilla **proteger objeto contra eliminación accidental** . En Active Directory usuarios y equipos, la pestaña **objeto** de un objeto aparece si hace clic en **Ver** y, a continuación, hace clic en **características avanzadas**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Limpiar metadatos de servidor mediante herramientas de GUI

Al usar Herramientas de administración remota del servidor (RSAT) o la consola de usuarios y equipos de Active Directory (DSA. msc) que se incluye con Windows Server para eliminar una cuenta de equipo de controlador de dominio de la unidad organizativa (OU) controladores de dominio, la limpieza de los metadatos del servidor se realiza automáticamente. Antes de Windows Server 2008, tenía que realizar un procedimiento de limpieza de metadatos independiente.

También puede usar la consola sitios y servicios de Active Directory (Dssite. msc) para eliminar la cuenta de equipo de un controlador de dominio, que también completa automáticamente la limpieza de los metadatos. Sin embargo, Active Directory sitios y servicios quita los metadatos automáticamente solo cuando se elimina por primera vez el objeto de configuración NTDS debajo de la cuenta de equipo en Dssite. msc.

Siempre que use las versiones de Windows Server 2008 o RSAT más recientes de DSA. msc o Dssite. msc, puede limpiar los metadatos automáticamente para los controladores de dominio que ejecutan versiones anteriores de los sistemas operativos Windows.

El requisito mínimo para completar estos procedimientos es la pertenencia al grupo **Admins**. del dominio o un grupo equivalente.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Limpieza de metadatos de servidor mediante usuarios y equipos de Active Directory

1. Abra **Usuarios y equipos de Active Directory**.
2. Si ha identificado asociados de replicación como preparación para este procedimiento y, si no está conectado a un asociado de replicación del controlador de dominio quitado cuyos metadatos está limpiando, haga clic con el botón secundario en **Active Directory nodo usuarios y equipos** y, a continuación, haga clic en **cambiar controlador de dominio**. Haga clic en el nombre del controlador de dominio del que desea quitar los metadatos y, a continuación, haga clic en **Aceptar**.
3. Expanda el dominio del controlador de dominio que se quitó forzosamente y, a continuación, haga clic en **controladores de dominio**.
4. En el panel de detalles, haga clic con el botón secundario en el objeto de equipo del controlador de dominio cuyos metadatos desea limpiar y, a continuación, haga clic en **eliminar**.
5. En el cuadro de diálogo **Active Directory Domain Services** , confirme que aparece el nombre del controlador de dominio que desea eliminar y haga clic en **sí** para confirmar la eliminación del objeto de equipo.
6. En el cuadro de diálogo **eliminando el controlador de dominio** , seleccione **este controlador de dominio está permanentemente sin conexión y ya no se puede disminuir de nivel mediante el Asistente para instalación de Active Directory Domain Services (Dcpromo)** y, a continuación, haga clic en **eliminar**.
7. Si el controlador de dominio es un servidor de catálogo global, en el cuadro de diálogo **eliminar controlador de dominio** , haga clic en **sí** para continuar con la eliminación.
8. Si el controlador de dominio contiene una o más funciones de maestro de operaciones, haga clic en **Aceptar** para trasladar el rol o los roles al controlador de dominio que se muestra. No se puede cambiar este controlador de dominio. Si desea trasladar el rol a otro controlador de dominio, debe trasladar el rol después de completar el procedimiento de limpieza de metadatos del servidor.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Limpieza de metadatos de servidor mediante sitios y servicios de Active Directory

1. Abra Sitios y servicios de Active Directory.
2. Si ha identificado asociados de replicación como preparación para este procedimiento y si no está conectado a un asociado de replicación del controlador de dominio quitado cuyos metadatos está limpiando, haga clic con el botón secundario en **Active Directory sitios y servicios**y, a continuación, haga clic en **cambiar el controlador de dominio**. Haga clic en el nombre del controlador de dominio del que desea quitar los metadatos y, a continuación, haga clic en **Aceptar**.
3. Expanda el sitio del controlador de dominio que se quitó forzosamente, expanda **servidores**, expanda el nombre del controlador de dominio, haga clic con el botón secundario en el objeto configuración NTDS y, a continuación, haga clic en **eliminar**.
4. En el cuadro de diálogo **Active Directory sitios y servicios** , haga clic en **sí** para confirmar la eliminación de la configuración NTDS.
5. En el cuadro de diálogo **eliminando el controlador de dominio** , seleccione **este controlador de dominio está permanentemente sin conexión y ya no se puede disminuir de nivel mediante el Asistente para instalación de Active Directory Domain Services (Dcpromo)** y, a continuación, haga clic en **eliminar**.
6. Si el controlador de dominio es un servidor de catálogo global, en el cuadro de diálogo **eliminar controlador de dominio** , haga clic en **sí** para continuar con la eliminación.
7. Si el controlador de dominio contiene una o más funciones de maestro de operaciones, haga clic en **Aceptar** para trasladar el rol o los roles al controlador de dominio que se muestra.
8. Haga clic con el botón secundario en el controlador de dominio que se quitó forzosamente y, a continuación, haga clic en eliminar.
9. En el cuadro de diálogo **Active Directory Domain Services** , haga clic en **sí** para confirmar la eliminación del controlador de dominio.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Limpiar metadatos de servidor mediante la línea de comandos

Como alternativa, puede limpiar los metadatos mediante Ntdsutil.exe, una herramienta de línea de comandos que se instala automáticamente en todos los controladores de dominio y servidores que tienen instalado Active Directory Lightweight Directory Services (AD LDS). Ntdsutil.exe también está disponible en los equipos que tienen instalado RSAT.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Para limpiar los metadatos del servidor mediante ntdsutil

1. Abra un símbolo del sistema como administrador: en el menú **Inicio** , haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**. Si aparece el cuadro de diálogo **control de cuentas de usuario** , proporcione las credenciales de un administrador de empresa si es necesario y, a continuación, haga clic en **continuar**.
2. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

   `ntdsutil`

3. En `ntdsutil:`, presione ENTRAR después de escribir:

   `metadata cleanup`

4. En `metadata cleanup:`, presione ENTRAR después de escribir:

   `remove selected server <ServerName>`

5. En el **cuadro de diálogo quitar configuración del servidor**, revise la información y la advertencia y, a continuación, haga clic en **sí** para quitar el objeto y los metadatos del servidor.

   En este momento, Ntdsutil confirma que el controlador de dominio se quitó correctamente. Si recibe un mensaje de error que indica que no se puede encontrar el objeto, es posible que el controlador de dominio se haya quitado anteriormente.

6. En el `metadata cleanup:` símbolo del sistema de y `ntdsutil:` , escriba y, `quit` a continuación, presione Entrar.

7. Para confirmar la eliminación del controlador de dominio:

   Abra Usuarios y equipos de Active Directory. En el dominio del controlador de dominio que se ha quitado, haga clic en **controladores de dominio**. En el panel de detalles, no debe aparecer un objeto para el controlador de dominio que ha quitado.

   Abra Active Directory sitios y servicios. Desplácese hasta el contenedor **servidores** y confirme que el objeto de servidor para el controlador de dominio que quitó no contiene un objeto de configuración NTDS. Si no aparecen objetos secundarios debajo del objeto de servidor, puede eliminar el objeto de servidor. Si aparece un objeto secundario, no elimine el objeto de servidor porque otra aplicación está usando el objeto.

## <a name="see-also"></a>Consulte también

* [Degradación de controladores de dominio](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Referencia de comandos de Ntdsutil](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc753343(v=ws.10))
