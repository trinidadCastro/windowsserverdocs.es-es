---
title: Crear una tarea de expiración de archivos
description: En este artículo se describe el proceso de creación de una tarea de administración de archivos para que los archivos expiren.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0ff4b46064ca780d63c6f06898c114cb180c3665
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971132"
---
# <a name="create-a-file-expiration-task"></a>Crear una tarea de expiración de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El siguiente procedimiento le guiará por el proceso de creación de una tarea de administración de archivos para los archivos que van a expirar. Las tareas de expiración de archivos se usan para mover automáticamente todos los archivos que coinciden con determinados criterios a un directorio de expiración especificado, donde un administrador puede hacer una copia de seguridad de esos archivos y eliminarlos.

Cuando se ejecuta una tarea de expiración de archivo, se crea un nuevo directorio en el directorio de expiración, agrupado por el nombre del servidor en el que se ejecutó la tarea.

El nombre de directorio nuevo se basa en el nombre de la tarea de administración de archivos y la hora en la que se ejecutó.Cuando se encuentra un archivo que ha expirado, se mueve al directorio nuevo, a la vez que se conserva la estructura de directorio original.

## <a name="to-create-a-file-expiration-task"></a>Para crear una tarea de expiración de archivo

1. Haga clic en el nodo **Tareas de administración de archivos**.

2. Haga clic con el botón secundario en **tareas de administración de archivos**y haga clic en **crear tarea de administración de archivos** (o haga clic en **crear tarea de administración de archivos** en el panel **acciones** ). Se abrirá el cuadro de diálogo **Crear tarea de administración de archivos**.

3. En la pestaña **General**, escriba la siguiente información:

   -   **Nombre**. Escriba un nombre para la nueva tarea.

   -   **Descripción**. Escriba una etiqueta descriptiva opcional para esta tarea.

   -   **Ámbito**. Agregue los directorios en los que esta tarea debe funcionar mediante el botón **Agregar** . Opcionalmente, los directorios se pueden quitar de la lista mediante el botón **quitar** . La tarea de administración de archivos se aplicará a todas las carpetas y sus subcarpetas en esta lista.

4. En la pestaña **Acción**, especifique la siguiente información:

   - **Tipo**. Seleccione **expiración de archivo** en el cuadro desplegable.

   - **Directorio de expiración**. Seleccione un directorio en el que vayan a expirar los archivos.

     > [!Warning]
     > No seleccione un directorio que se encuentre dentro del ámbito de la tarea tal como se define en el paso anterior. Si lo hace, podría generar un bucle iterativo que podría provocar la inestabilidad del sistema y la pérdida de datos.

5. Opcionalmente, en la pestaña **notificación** , haga clic en **Agregar** para enviar notificaciones por correo electrónico, registrar un evento o ejecutar un comando o un script un número mínimo de días especificado antes de que la tarea realice una acción en un archivo.

   - En el cuadro combinado **número de días antes de que se ejecute la tarea para enviar la notificación** , escriba o seleccione un valor para especificar el número mínimo de días antes de que se envíe una notificación a un archivo.

     > [!Note]
     > Las notificaciones se envían solo cuando se ejecuta una tarea. Si el número mínimo de días especificado para enviar una notificación no coincide con una tarea programada, la notificación se enviará el día de la tarea programada anterior.

   - Para configurar las notificaciones de correo electrónico, haga clic en la pestaña **mensaje de correo electrónico** y escriba la siguiente información:

     - Para notificar a los administradores cuando se alcanza un umbral, active la casilla **Enviar correo electrónico a los siguientes administradores** y, a continuación, escriba los nombres de las cuentas administrativas que recibirán las notificaciones. Use el formato <em>account@domain</em> y use punto y coma para separar varias cuentas.

     - Para enviar un mensaje de correo electrónico a la persona cuyos archivos están a punto de expirar, active la casilla **Enviar correo electrónico al usuario cuyos archivos están a punto de expirar** .

     - Para configurar el mensaje, edite la línea de asunto y el cuerpo de mensaje predeterminados que se proporcionan. El texto entre paréntesis inserta información de variables sobre el evento de cuota que causó la notificación. Por ejemplo, la variable de ** \[ propietario \] del archivo de código fuente** inserta el nombre del usuario cuyo archivo está a punto de expirar. Para insertar variables adicionales en el texto, haga clic en **Insertar variable**.

     - Para adjuntar una lista de los archivos que van a expirar, haga clic en **adjuntar a la lista de correo electrónico de los archivos en los que se realizará la acción**y escriba o seleccione un valor para el **número máximo de archivos de la lista**.

     - Para configurar encabezados adicionales (incluidos de, CC, CCO y responder a), haga clic en **encabezados de correo electrónico adicionales**.

   - Para registrar un evento, haga clic en la pestaña **registro de eventos** y active la casilla **Enviar advertencia al registro de eventos** y, a continuación, edite la entrada de registro predeterminada.

   - Para ejecutar un comando o un script, haga clic en la pestaña **comando** y active la casilla **ejecutar este comando o script** . A continuación, escriba el comando o haga clic en **examinar** para buscar la ubicación donde se almacena el script. También puede especificar argumentos de comandos, seleccionar un directorio de trabajo para el comando o script, o modificar la configuración de seguridad del comando.

6. Otra alternativa es usar la pestaña **Informe** para generar uno o varios registros o informes de almacenamiento.

   - Para generar registros, active la casilla **generar registro** y, a continuación, seleccione uno o más registros disponibles.

   - Para generar informes, active la casilla **generar un informe** y, a continuación, seleccione uno o más formatos de informe disponibles.

   - Para crear registros generados por correo electrónico o informes de almacenamiento, active la casilla **enviar informes a los siguientes administradores** y escriba uno o más destinatarios de correo electrónico administrativos con el formato <em>account@domain</em> . Use un punto y coma para separar varias direcciones.

     > [!Note]
     > El informe se guarda en la ubicación predeterminada para los informes de incidentes, que se puede modificar en el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

7. Otra opción es usar la pestaña **Condiciones** para ejecutar esta tarea solo en archivos que reúnan una serie de condiciones definidas. Las siguientes configuraciones están disponibles:

    -   **Condiciones de propiedad**. Haga clic en **Agregar** para crear una nueva condición basada en la clasificación del archivo. Se abrirá el cuadro de diálogo **condición de propiedad** , que permite seleccionar una propiedad, un operador que se va a realizar en la propiedad y el valor con el que se va a comparar la propiedad. Después de hacer clic en **Aceptar**, puede crear condiciones adicionales o editar o quitar una condición existente.

    -   **Días desde la última modificación del archivo**. Haga clic en la casilla y, a continuación, escriba un número de días en el cuadro de número. Esto hará que la tarea de administración de archivos solo se aplique a los archivos que no se han modificado durante más de un número especificado de días.

    -   **Días desde el último acceso al archivo**. Haga clic en la casilla y, a continuación, escriba un número de días en el cuadro de número. Si el servidor está configurado para realizar el seguimiento de las marcas de tiempo de los archivos a los que se tuvo acceso por última vez, esto hará que la tarea de administración de archivos se aplique solo a los archivos a los que no se ha tenido acceso durante más de un número especificado de días. Si el servidor no está configurado para realizar un seguimiento de las horas de acceso, esta condición no tendrá efecto.

    -   **Días transcurridos desde que se creó el archivo**. Haga clic en la casilla y, a continuación, escriba un número de días en el cuadro de número. Esto hará que la tarea solo se aplique a los archivos que se crearon al menos el número de días especificado.

    -   **Inicio efectivo**. Establecer una fecha en la que esta tarea de administración de archivos debe empezar a procesar archivos. Esta opción es útil para retrasar la tarea hasta que haya tenido la oportunidad de notificar a los usuarios o realizar otras preparaciones de antemano.

8. En la pestaña **programación** , haga clic en **crear programación**y, a continuación, en el cuadro de diálogo **programación** , haga clic en **nuevo**. Esto muestra un conjunto de programación predeterminado de 9:00 A.M. diariamente, pero puede modificar la programación predeterminada. Cuando haya terminado de configurar la programación, haga clic en **Aceptar** y, a continuación, haga clic en **Aceptar** de nuevo.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de clasificaciones](classification-management.md)
-   [Tareas de administración de archivos](file-management-tasks.md)