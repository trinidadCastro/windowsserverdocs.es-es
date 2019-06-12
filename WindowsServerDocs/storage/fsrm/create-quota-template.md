---
title: Crear una plantilla de cuota
description: En este artículo se describe cómo crear una plantilla de cuota para definir un límite de espacio de almacenamiento
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 236b5cb198a13441a087ad6dbfeef9a416e07e61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445985"
---
# <a name="create-a-quota-template"></a>Crear una plantilla de cuota

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Una *plantilla de cuota* define el límite de espacio, el tipo de cuota (máxima o de advertencia) y, de forma opcional, el conjunto de notificaciones que se generarán automáticamente cuando el uso de cuotas alcance los niveles de umbrales definidos.

Si las cuotas se crean exclusivamente a partir de plantillas, es posible administrarlas centralmente actualizando las plantillas en lugar de repetir los cambios en cada una de las cuotas. Esta característica simplifica la implementación de los cambios de las directivas de almacenamiento ya que proporciona un punto central donde se pueden llevar a cabo todas las actualizaciones.

## <a name="to-create-a-quota-template"></a>Para crear una plantilla de cuota

1.  En **Administración de cuotas** haga clic en el nodo **Plantillas de cuota**.

2.  Haga clic con el botón secundario en **Plantillas de cuota** y, a continuación, haga clic en **Crear plantilla de cuota** (o seleccione **Crear plantilla de cuota** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear plantilla de cuota**.

3.  Si quieres copiar las propiedades de una plantilla existente para usarla como base de la nueva plantilla, selecciona una plantilla en la lista desplegable **Copiar propiedades de la plantilla de cuota**. Luego haz clic en **Copiar**.

    Tanto si ha elegido usar las propiedades de una plantilla existente como si está creando una nueva plantilla, modifique o defina los siguientes valores en la pestaña **Configuración**:

4.  En el cuadro de texto **Nombre de plantilla**, escribe el nombre de la nueva plantilla.

5.  En el cuadro de texto **Etiqueta**, escribe una etiqueta descriptiva opcional que aparecerá junto a las cuotas derivadas de la plantilla.

6.  En **Límite de espacio**:

    -   En el cuadro de texto **Límite**, escribe un número y elige una unidad (KB, MB, GB o TB) para especificar el límite de espacio de la cuota.
    -   Haz clic en la opción **Cuota máxima** o **Cuota de advertencia**. (La cuota máxima impide a los usuarios guardar archivos una vez alcanzado el límite de espacio y genera notificaciones cuando el volumen de datos llega al umbral configurado. La cuota de advertencia no impone un límite de cuota, pero genera todas las notificaciones configuradas).

7.  Puedes configurar una o varias notificaciones de umbral opcionales para la plantilla de cuota, tal y como se indica en el siguiente procedimiento. Después de seleccionar todas las propiedades de la plantilla de cuota que desee usar, haga clic en **Aceptar** para guardar la plantilla.

## <a name="setting-optional-notification-thresholds"></a>Configurar umbrales de notificación opcionales

Cuando el almacenamiento de un volumen o carpeta llega al nivel de umbral que has definido, el Administrador de recursos del servidor de archivos puede enviar mensajes de correo electrónico a los administradores o a usuarios específicos, registrar un evento, ejecutar un comando o script o generar informes. Puedes configurar más de un tipo de notificación para cada umbral y puedes definir varios umbrales para cualquier cuota (o plantilla de cuota). De forma predeterminada, no se genera ninguna notificación.

Por ejemplo, puedes configurar umbrales para enviar un mensaje de correo electrónico al administrador y a los usuarios que estarían interesados en saber cuándo una carpeta alcanza el 85 % de su límite de cuota y luego enviar otra notificación cuando se alcanza el límite de cuota. Además, es posible que quieras ejecutar un script que usa el comando **dirquota.exe** para aumentar el límite de cuota automáticamente cuando se alcanza un umbral.

> [!Important]
> Para enviar notificaciones por correo electrónico y configurar los informes de almacenamiento con los parámetros adecuados para tu entorno de servidor, primero debes establecer las opciones generales del Administrador de recursos del servidor de archivos. Para obtener más información, consulta [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)

**Para configurar las notificaciones que generará el Administrador de recursos del servidor de archivos con un umbral de cuota**

1. En el cuadro de diálogo **Crear plantilla de cuota**, en **Umbrales de notificación**, haz clic en **Agregar**. Aparece el cuadro de diálogo **Agregar umbral**.

2. Para establecer un porcentaje del límite de cuota que generará una notificación:

   En el cuadro de texto **Generar notificaciones cuando el uso alcance (%)** , escribe un porcentaje del límite de cuota para el umbral de notificación. (El porcentaje predeterminado para el primer umbral de notificación es del 85 %).

3. Para configurar notificaciones por correo electrónico:

   En la pestaña **Mensaje de correo electrónico**, establece las siguientes opciones:

   - Para notificar a los administradores que se ha alcanzado un umbral, selecciona la casilla de verificación **Enviar mensaje de correo electrónico a los siguientes administradores** y luego escribe los nombres de las cuentas administrativas que recibirán las notificaciones. Usa el formato <em>account@domain</em> y usa punto y coma para separar varias cuentas.
   - Para enviar un correo electrónico a la persona que guardó el archivo que alcanzó el umbral de cuota, selecciona la casilla de verificación **Enviar correo electrónico al usuario que ha superado el umbral**.
   - Para configurar el mensaje, edita la línea de asunto y el cuerpo del mensaje predeterminados que se muestran. El texto que aparece entre paréntesis inserta información variable sobre el evento de cuota que originó la notificación. Por ejemplo, el **\[Source Io Owner\]** variable inserta el nombre del usuario que guardó el archivo que alcanzó el umbral de cuota. Para insertar variables adicionales en el texto, haz clic en **Insertar variable**.
   - Para configurar los encabezados adicionales (incluidos De, Cc, Bcc y Responder-a), haz clic en **Encabezados de correo electrónico adicionales**.

4. Para registrar un evento:

   En la pestaña **Registro de eventos**, selecciona la casilla de verificación **Enviar advertencia al registro de eventos** y edita la entrada de registro predeterminada.

5. Para ejecutar un comando o script:

   En la pestaña **Comando**, selecciona loa casilla de verificación **Ejecutar este comando o script**. Luego escribe el comando o haz clic en **Examinar** para buscar la ubicación en la que está almacenado el script. También puedes especificar argumentos de comando; para ello, selecciona un directorio de trabajo para el comando o script o modifica la configuración de seguridad de comandos.

6. Para generar uno o varios informes de almacenamiento:

   En la pestaña **Informe**, selecciona la casilla de verificación **Generar informes** y luego selecciona los informes que quieres generar. (Puedes elegir uno o varios destinatarios de correos electrónicos administrativos para enviar el informe o puedes enviar por correo electrónico el informe al usuario que alcanzó el umbral).

   El informe se guarda en la ubicación predeterminada para los informes de incidentes, que se puede modificar en el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

7. Haz clic en **Aceptar** para guardar el umbral de notificación.

8. Repite estos pasos si quieres configurar umbrales de notificación adicionales para la plantilla de cuota.

## <a name="see-also"></a>Vea también

-   [Administración de cuotas](quota-management.md)
-    [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Editar propiedades de plantillas de cuotas](edit-quota-template-properties.md)
-   [Herramientas de línea de comandos](command-line-tools.md)


