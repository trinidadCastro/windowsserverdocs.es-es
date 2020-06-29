---
title: Create a Quota Template
description: En este artículo se describe cómo crear una plantilla de cuota para definir un límite de espacio de almacenamiento.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ad05d72098851e39bc245711b73e5ad721c5a784
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473562"
---
# <a name="create-a-quota-template"></a>Create a Quota Template

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Una *plantilla de cuota* define un límite de espacio, el tipo de cuota (fuerte o débil) y, opcionalmente, un conjunto de notificaciones que se generarán automáticamente cuando el uso de cuotas alcance los niveles de umbrales definidos.

Si las cuotas se crean exclusivamente a partir de plantillas, es posible administrarlas centralmente actualizando las plantillas en lugar de repetir los cambios en cada una de las cuotas. Esta característica simplifica la implementación de los cambios de las directivas de almacenamiento ya que proporciona un punto central donde se pueden llevar a cabo todas las actualizaciones.

## <a name="to-create-a-quota-template"></a>Para crear una plantilla de cuota

1.  En **Administración de cuotas** haga clic en el nodo **Plantillas de cuota**.

2.  Haga clic con el botón secundario en **Plantillas de cuota** y, a continuación, haga clic en **Crear plantilla de cuota** (o seleccione **Crear plantilla de cuota** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear plantilla de cuota**.

3.  Si desea copiar las propiedades de una plantilla existente para usarla como base para la nueva plantilla, seleccione una plantilla en la lista desplegable **copiar propiedades de la plantilla de cuota** . A continuación, haga clic en **copiar**.

    Tanto si ha elegido usar las propiedades de una plantilla existente como si está creando una nueva plantilla, modifique o defina los siguientes valores en la pestaña **Configuración**:

4.  En el cuadro de texto **nombre de plantilla** , escriba un nombre para la nueva plantilla.

5.  En el cuadro de texto **etiqueta** , escriba una etiqueta descriptiva opcional que aparecerá junto a las cuotas derivadas de la plantilla.

6.  En **Límite de espacio**:

    -   En el cuadro de texto **límite** , escriba un número y elija una unidad (KB, MB, GB o TB) para especificar el límite de espacio de la cuota.
    -   Haga clic en la opción **cuota máxima** o **cuota de software** . (La cuota máxima impide a los usuarios guardar archivos una vez alcanzado el límite de espacio y genera notificaciones cuando el volumen de datos llega al umbral configurado. La cuota de advertencia no impone un límite de cuota, pero genera todas las notificaciones configuradas).

7.  Puede configurar uno o varios umbrales de notificación opcionales para la plantilla de cuota, tal como se describe en el siguiente procedimiento. Después de seleccionar todas las propiedades de la plantilla de cuota que desee usar, haga clic en **Aceptar** para guardar la plantilla.

## <a name="setting-optional-notification-thresholds"></a>Configuración de umbrales de notificación opcionales

Cuando el almacenamiento en un volumen o una carpeta alcance un nivel de umbral definido, el Administrador de recursos del servidor de archivos puede enviar mensajes de correo electrónico a los administradores o a usuarios determinados, registrar un evento, ejecutar un comando o un script o generar informes. Puede configurar más de un tipo de notificaciones para cada umbral y puede definir varios umbrales para cualquier cuota (o plantilla de cuota). De manera predeterminada, no se generan notificaciones.

Por ejemplo, puede configurar umbrales para enviar mensajes de correo electrónico al administrador y a los usuarios que necesiten saber cuándo alcanza una carpeta el 85% de su límite de cuota y, a continuación, enviar otra notificación cuando se alcance el límite de cuota. Además, es posible que desee ejecutar un script que use el comando **dirquota.exe** para aumentar el límite de cuota automáticamente cuando se alcance un umbral.

> [!Important]
> Para enviar notificaciones por correo electrónico y configurar los informes de almacenamiento con parámetros adecuados para su entorno de servidor, primero debe establecer las opciones generales del servidor de archivos Administrador de recursos. Para obtener más información, consulte [establecer opciones de administrador de recursos de servidor de archivos](setting-file-server-resource-manager-options.md) .

**Para configurar las notificaciones que generará el Administrador de recursos del servidor de archivos al alcanzar un umbral de cuota**

1. En el cuadro de diálogo **Crear plantilla de cuota** , en **umbrales de notificación**, haga clic en **Agregar**. Aparece el cuadro de diálogo **Agregar umbral** .

2. Para establecer un porcentaje de límite de cuota que generará una notificación:

   En el cuadro de texto **generar notificaciones cuando el uso alcance (%)** , escriba un porcentaje del límite de cuota para el umbral de notificación. (El porcentaje predeterminado para el primer umbral de notificación es 85%.)

3. Para configurar notificaciones de correo electrónico:

   En la pestaña **mensaje de correo electrónico** , establezca las siguientes opciones:

   - Para notificar a los administradores cuando se alcanza un umbral, active la casilla **Enviar correo electrónico a los siguientes administradores** y, a continuación, escriba los nombres de las cuentas administrativas que recibirán las notificaciones. Use el formato <em>account@domain</em> y use punto y coma para separar varias cuentas.
   - Para enviar un mensaje de correo electrónico a la persona que guardó el archivo que alcanzó el umbral de cuota, active la casilla **Enviar correo electrónico al usuario que superó el umbral** .
   - Para configurar el mensaje, edite la línea de asunto y el cuerpo de mensaje predeterminados que se proporcionan. El texto entre paréntesis inserta información de variables sobre el evento de cuota que causó la notificación. Por ejemplo, la variable ** \[ propietaria \] de e/s de origen** inserta el nombre del usuario que guardó el archivo que alcanzó el umbral de cuota. Para insertar variables adicionales en el texto, haga clic en **Insertar variable**.
   - Para configurar encabezados adicionales (incluidos de, CC, CCO y responder a), haga clic en **encabezados de correo electrónico adicionales**.

4. Para registrar un evento:

   En la pestaña **registro de eventos** , active la casilla **Enviar advertencia al registro de eventos** y edite la entrada de registro predeterminada.

5. Para ejecutar un comando o un script:

   En la pestaña **comando** , active la casilla **ejecutar este comando o script** . A continuación, escriba el comando o haga clic en **examinar** para buscar la ubicación donde se almacena el script. También puede especificar argumentos de comandos, seleccionar un directorio de trabajo para el comando o script, o modificar la configuración de seguridad del comando.

6. Para generar uno o varios informes de almacenamiento:

   En la pestaña **Informe** , active la casilla **generar informes** y, a continuación, seleccione los informes que desea generar. (Puede elegir uno o más destinatarios de correo electrónico administrativos para el informe o enviar el informe por correo electrónico al usuario que alcanzó el umbral.)

   El informe se guarda en la ubicación predeterminada para los informes de incidentes, que se puede modificar en el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

7. Haga clic en **Aceptar** para guardar el umbral de notificación.

8. Repita estos pasos si desea configurar umbrales de notificación adicionales para la plantilla de cuota.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de cuotas](quota-management.md)
-    [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Editar propiedades de plantillas de cuotas](edit-quota-template-properties.md)
-   [Herramientas de línea de comandos](command-line-tools.md)


