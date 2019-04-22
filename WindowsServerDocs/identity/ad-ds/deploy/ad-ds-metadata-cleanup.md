---
title: Limpiar los metadatos del servidor de AD DS
description: Use herramientas integradas para limpiar los metadatos de controladores de dominio quitados
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fbb6e720c9289c608d71d3c36695ba623a9df5f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818086"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Limpiar los metadatos del servidor de controlador de dominio de Active Directory

Se aplica a: Windows Server

Limpieza de metadatos es un procedimiento necesario después de una eliminación forzada de servicios de dominio de Active Directory (AD DS). Realizar la limpieza de metadatos en un controlador de dominio en el dominio del controlador de dominio que forzara la eliminación. Limpieza de metadatos quita datos de AD DS que identifica un controlador de dominio para el sistema de replicación. Limpieza de metadatos también quita las conexiones de servicio de replicación de archivos (FRS) y la replicación del sistema de archivos distribuido (DFS) y se intenta transferir o asumir las funciones de maestro (operaciones de maestro únicas también conocido como flexible o FSMO) de operaciones que el dominio retirado contiene el controlador.

Hay tres opciones para limpiar los metadatos del servidor:

- Limpiar los metadatos del servidor mediante las herramientas de GUI
- Limpiar metadatos de servidor mediante la línea de comandos
- Limpiar los metadatos del servidor mediante una secuencia de comandos

> [!NOTE]
> Si recibe un error "Acceso denegado" al usar cualquiera de estos métodos para realizar la limpieza de metadatos, asegúrese de que el objeto de equipo y el objeto configuración NTDS para el controlador de dominio no están protegidos contra eliminación accidental. Para comprobar esta con el botón secundario en el objeto de equipo o el objeto configuración NTDS, haga clic en **propiedades**, haga clic en **objeto**y desactive el **proteger objeto contra eliminación accidental** casilla de verificación. En usuarios de Active Directory y los equipos, el **objeto** pestaña de un objeto aparece si hace clic en **vista** y, a continuación, haga clic en **características avanzadas**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Limpiar los metadatos de servidor mediante las herramientas de GUI

Al usar las herramientas de administración remota de servidor (RSAT) o los usuarios de Active Directory y la consola de equipos (Dsa.msc) que se incluye con Windows Server para eliminar una cuenta de equipo del controlador de dominio de la unidad organizativa (OU), de los controladores de dominio el limpieza de metadatos del servidor se realiza automáticamente. Antes de Windows Server 2008, usted tenía que realizar un procedimiento de limpieza de metadatos independientes.

También puede usar la consola de servicios y sitios de Active Directory (Dssite.msc) para eliminar la cuenta de equipo del controlador de dominio, que también se complete la limpieza de metadatos automáticamente. Sin embargo, los servicios y sitios de Active Directory quita los metadatos automáticamente solo al eliminar primero el objeto configuración NTDS debajo de la cuenta de equipo en Dssite.msc.

Siempre y cuando usa el Windows Server 2008 o versiones más recientes de RSAT de Dsa.msc o Dssite.msc, puede limpiar los metadatos automáticamente para los controladores de dominio que ejecutan versiones anteriores de sistemas operativos de Windows.

Pertenencia a **Admins. del dominio**, o equivalente, es lo mínimo necesario para completar estos procedimientos.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Limpiar los metadatos de servidor mediante usuarios y equipos de usuarios de Active Directory

1. Abra **Usuarios y equipos de Active Directory**.
2. Si se ha identificado los asociados de replicación en preparación para este procedimiento y si no está conectado a un asociado de replicación del controlador de dominio eliminados cuyos metadatos que limpie, haga clic en **usuarios de Active Directory y Equipos** nodo y, a continuación, haga clic en **cambiar el controlador de dominio**. Haga clic en el nombre del controlador de dominio desde el que desea quitar los metadatos y, a continuación, haga clic en **Aceptar**.
3. Expanda el dominio del controlador de dominio que se ha quitado de manera forzosa y, a continuación, haga clic en **controladores de dominio**.
4. En el panel de detalles, haga clic en el objeto de equipo del controlador de dominio cuyos metadatos se van a limpiar y, a continuación, haga clic en **eliminar**.
5. En el **Active Directory Domain Services** diálogo cuadro, confirme se muestra el nombre del controlador de dominio que desea eliminar y haga clic en **Sí** para confirmar la eliminación del objeto de equipo.
6. En el **eliminando el controlador de dominio** cuadro de diálogo, seleccione **este controlador de dominio está permanentemente sin conexión y no puede degradarse con el Active Directory Domain Services instalación asistente (DCPROMO)** y, a continuación, haga clic en **eliminar**.
7. Si el controlador de dominio es un servidor de catálogo global, en el **eliminar controlador de dominio** cuadro de diálogo, haga clic en **Sí** para continuar con la eliminación.
8. Si el controlador de dominio actualmente contiene una o más operaciones de roles de maestro, haga clic en **Aceptar** para mover el rol o los roles para el controlador de dominio que se muestra. No se puede cambiar este controlador de dominio. Si desea mover el rol a otro controlador de dominio, debe mover el rol después de completar el procedimiento de limpieza de metadatos de servidor.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Limpiar los metadatos de servidor con servicios y sitios de Active Directory

1. Abra Sitios y servicios de Active Directory.
2. Si se ha identificado los asociados de replicación en preparación para este procedimiento y si no está conectado a un asociado de replicación del controlador de dominio eliminados cuyos metadatos que limpie, haga clic en **servicios y sitios de Active Directory** y, a continuación, haga clic en **cambiar el controlador de dominio**. Haga clic en el nombre del controlador de dominio desde el que desea quitar los metadatos y, a continuación, haga clic en **Aceptar**.
3. Expanda el sitio del controlador de dominio que se forzara la eliminación, **servidores**, expanda el nombre del controlador de dominio, haga clic en el objeto configuración NTDS y, a continuación, haga clic en **eliminar**.
4. En el **servicios y sitios de Active Directory** cuadro de diálogo, haga clic en **Sí** para confirmar la eliminación de configuración NTDS.
5. En el **eliminando el controlador de dominio** cuadro de diálogo, seleccione **este controlador de dominio está permanentemente sin conexión y no puede degradarse con el Active Directory Domain Services instalación asistente (DCPROMO)** y, a continuación, haga clic en **eliminar**.
6. Si el controlador de dominio es un servidor de catálogo global, en el **eliminar controlador de dominio** cuadro de diálogo, haga clic en **Sí** para continuar con la eliminación.
7. Si el controlador de dominio actualmente contiene una o más operaciones de roles de maestro, haga clic en **Aceptar** para mover el rol o los roles para el controlador de dominio que se muestra.
8. Haga clic en el controlador de dominio que se ha quitado de manera forzosa y, a continuación, haga clic en eliminar.
9. En el **Active Directory Domain Services** cuadro de diálogo, haga clic en **Sí** para confirmar la eliminación del controlador de dominio.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Limpiar metadatos de servidor mediante la línea de comandos

Como alternativa, puede limpiar los metadatos mediante el uso de Ntdsutil.exe, una herramienta de línea de comandos que se instala automáticamente en todos los controladores de dominio y los servidores que tienen Active Directory Lightweight Directory Services (AD LDS) instalado. Ntdsutil.exe también está disponible en equipos que tienen instalado RSAT.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Para limpiar los metadatos del servidor mediante Ntdsutil

1. Abra un símbolo del sistema como administrador: En el menú **Inicio**, haga clic con el botón secundario en **Símbolo del sistema** y, a continuación, haga clic en **Ejecutar como administrador**. Si el **User Account Control** aparece el cuadro de diálogo, proporcione las credenciales de un administrador de empresa si es necesario y, a continuación, haga clic en **continuar**.
2. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

   `ntdsutil`

3. En `ntdsutil:`, presione ENTRAR después de escribir:

   `metadata cleanup`

4. En `metadata cleanup:`, presione ENTRAR después de escribir:

   `remove selected server <ServerName>`

5. En **el cuadro de diálogo Quitar de configuración de servidor**, revise la información y una advertencia y, a continuación, haga clic en **Sí** para quitar el objeto de servidor y los metadatos.

   En este momento, Ntdsutil confirma que el controlador de dominio se quitó correctamente. Si recibe un mensaje de error que indica que no se encuentra el objeto, el controlador de dominio podría haber quitado anteriormente.

6. En el `metadata cleanup:` y `ntdsutil:` indicaciones, escriba `quit`, y, a continuación, presione ENTRAR.

7. Para confirmar la eliminación del controlador de dominio:

   Abra los equipos y usuarios de Active Directory. En el dominio del controlador de dominio eliminados, haga clic en **controladores de dominio**. En el panel de detalles, no debe aparecer un objeto para el controlador de dominio que se haya quitado.

   Abra Servicios y sitios de Active Directory. Navegue hasta la **servidores** contenedor y confirme que el objeto de servidor para el controlador de dominio que se haya quitado no contiene un objeto configuración NTDS. Si no hay objetos secundarios aparecen debajo del objeto de servidor, puede eliminar el objeto de servidor. Si aparece un objeto secundario, no elimine el objeto de servidor porque otra aplicación está utilizando el objeto.

## <a name="see-also"></a>Vea también

* [Degradar controladores de dominio](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Referencia de comandos Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
