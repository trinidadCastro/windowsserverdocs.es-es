---
title: Create a File Screen Template
description: En este artículo se describe cómo crear una plantilla de filtro de archivos.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 87df941015b240fd34028e59b8aea489e9410834
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475632"
---
# <a name="create-a-file-screen-template"></a>Create a File Screen Template

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Una *plantilla de filtro de archivos* define un conjunto de grupos de archivos para la pantalla, el tipo de filtrado que se va a realizar (activo o pasivo) y, opcionalmente, un conjunto de notificaciones que se generarán automáticamente cuando un usuario guarde o intente guardar un archivo no autorizado.

El Administrador de recursos del servidor de archivos puede enviar notificaciones de correo electrónico a los administradores o a usuarios concretos, registrar eventos, ejecutar un comando o un script, o generar informes. Puede configurar más de un tipo de notificación para un evento de filtro de archivos.

Si los filtros de archivos se crean exclusivamente a partir de plantillas, es posible administrarlos centralmente actualizando las plantillas en lugar de replicar los cambios en cada filtro de archivos. Esta característica simplifica la implementación de los cambios de las directivas de almacenamiento ya que proporciona un punto central donde se pueden llevar a cabo todas las actualizaciones.

> [!Important]
> Para enviar notificaciones por correo electrónico y configurar los informes de almacenamiento con parámetros adecuados para su entorno de servidor, primero debe establecer las opciones generales del servidor de archivos Administrador de recursos. Para obtener más información, vea [configurar las opciones del administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md).

## <a name="to-create-a-file-screen-template"></a>Para crear una plantilla de filtro de archivos

1.  En **Administración del filtrado de archivos**, haga clic en el nodo **plantillas de filtro de archivos** .

2.  Haga clic con el botón secundario en **plantillas de filtro de archivos**y, a continuación, haga clic en **Crear plantilla de filtro** de archivos (o seleccione **Crear plantilla de filtro de archivos** en el panel **acciones** ). Se abrirá el cuadro de diálogo **Crear plantilla de filtro de archivos** .

3.  Si desea copiar las propiedades de una plantilla existente para usarla como base para la nueva plantilla, seleccione una plantilla en la lista desplegable **copiar propiedades de la plantilla** y, a continuación, haga clic en **copiar**.

    Tanto si ha elegido usar las propiedades de una plantilla existente como si está creando una nueva plantilla, modifique o defina los siguientes valores en la pestaña **Configuración**:

4.  En el cuadro de texto **nombre de plantilla** , escriba un nombre para la nueva plantilla.

5.  En **tipo de filtrado**, haga clic en la opción **filtrado activo** o **filtrado pasivo** . (El filtrado activo impide a los usuarios guardar los archivos que pertenecen a los grupos de archivos bloqueados y genera notificaciones cuando los usuarios intentan guardar archivos no autorizados. El filtrado pasivo envía notificaciones configuradas, pero no impide que los usuarios guarden archivos.

6.  Para especificar qué grupos de archivos se deben filtrar:

    En **grupos de archivos**, seleccione cada grupo de archivos que desee incluir. (Para activar la casilla del grupo de archivos, haga doble clic en su etiqueta).

    Si desea ver los tipos de archivo que un grupo de archivos incluye y excluye, haga clic en la etiqueta del grupo de archivos y, a continuación, haga clic en **Editar**. Para crear un nuevo grupo de archivos, haga clic en **crear**.

    Además, puede configurar el Administrador de recursos del servidor de archivos para generar una o varias notificaciones mediante el establecimiento de las siguientes opciones en las pestañas **mensaje de correo electrónico**, **registro de eventos**, **comando**e **Informe** .

7.  Para configurar notificaciones de correo electrónico:

    En la pestaña **mensaje de correo electrónico** , establezca las siguientes opciones:

    -   Para notificar a los administradores cuando un usuario o una aplicación intenta guardar un archivo no autorizado, active la casilla **Enviar correo electrónico a los siguientes administradores** y, a continuación, escriba los nombres de las cuentas administrativas que recibirán las notificaciones. Use el formato *account* @ *dominio*de cuenta y use punto y coma para separar varias cuentas.
    -   Para enviar un mensaje de correo electrónico al usuario que ha intentado guardar el archivo, active la casilla **Enviar correo electrónico al usuario que intentó guardar un archivo no autorizado** .
    -   Para configurar el mensaje, edite la línea de asunto y el cuerpo de mensaje predeterminados que se proporcionan. El texto entre paréntesis inserta información de variables sobre el evento de filtro de archivos que causó la notificación. Por ejemplo, la \[ variable **propietaria de e/s de origen** \] inserta el nombre del usuario que ha intentado guardar un archivo no autorizado. Para insertar variables adicionales en el texto, haga clic en **Insertar variable**.
    -   Para configurar encabezados adicionales (incluidos de, CC, CCO y responder a), haga clic en **encabezados de correo electrónico adicionales**.

8.  Para registrar un error en el registro de eventos cuando un usuario intenta guardar un archivo no autorizado:

    En la pestaña **registro de eventos** , active la casilla **Enviar advertencia al registro de eventos** y edite la entrada de registro predeterminada.

9.  Para ejecutar un comando o un script cuando un usuario intenta guardar un archivo no autorizado:

    En la pestaña **comando** , active la casilla **ejecutar este comando o script** . A continuación, escriba el comando o haga clic en **examinar** para buscar la ubicación donde se almacena el script. También puede especificar argumentos de comandos, seleccionar un directorio de trabajo para el comando o script, o modificar la configuración de seguridad del comando.

10. Para generar uno o más informes de almacenamiento cuando un usuario intenta guardar un archivo no autorizado:

    En la pestaña **Informe** , active la casilla **generar informes** y, a continuación, seleccione los informes que desea generar. (Puede elegir uno o más destinatarios de correo electrónico administrativos para el informe o enviar el informe por correo electrónico al usuario que intentó guardar el archivo.)

    El informe se guarda en la ubicación predeterminada para los informes de incidentes, que se puede modificar en el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

11. Después de seleccionar todas las propiedades de la plantilla de archivo que desea usar, haga clic en **Aceptar** para guardar la plantilla.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Editar las propiedades de la plantilla de filtro de archivos](edit-file-screen-template-properties.md)

