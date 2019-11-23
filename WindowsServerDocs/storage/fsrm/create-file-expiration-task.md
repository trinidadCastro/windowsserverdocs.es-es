---
title: Crear una tarea de expiración de archivos
description: En este artículo se describe el proceso de creación de una tarea de administración de archivos para los archivos que están a punto de expirar
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0901c17203252414a37ccc5205a0946b8bef0d41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394230"
---
# <a name="create-a-file-expiration-task"></a>Crear una tarea de expiración de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El procedimiento que se describe a continuación te guía a través del proceso de creación de una tarea de administración de archivos para los archivos que expiran. Las tareas de expiración de archivos se usan para mover automáticamente todos los archivos que coinciden con ciertos criterios a un directorio de expiración especificado, donde un administrador puede hacer copias de seguridad de esos archivos y eliminarlos.

Cuando se ejecuta una tarea de expiración de archivos, se crea un nuevo directorio en el directorio de expiración, agrupado por el nombre de servidor en el que se ejecutó la tarea.

El nombre de directorio nuevo se basa en el nombre de la tarea de administración de archivos y la hora en la que se ejecutó. Cuando se encuentra un archivo expirado, se mueve al nuevo directorio, conservando así la estructura de directorios original.

## <a name="to-create-a-file-expiration-task"></a>Para crear una tarea de expiración de archivos

1. Haga clic en el nodo **Tareas de administración de archivos**.

2. Haz clic con el botón derecho en **Tareas de administración de archivos**y luego haz clic en **Crear tarea de administración de archivos** (o haz clic en **Crear tarea de administración de archivos** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear tarea de administración de archivos**.

3. En la ficha **General**, escriba la siguiente información:

   -   **Nombre**. Escriba un nombre para la nueva tarea.  

   -   **Descripción**. Escriba una etiqueta descriptiva opcional para esta tarea.  
    
   -   **Ámbito**. Agrega los directorios en los que esta tarea debe operar con el botón **Agregar**. De forma opcional, los directorios se pueden quitar de la lista con el botón **Quitar**. La tarea de administración de archivos se aplicará a todas las carpetas y a sus subcarpetas de esta lista.

4. En la ficha **Acción**, especifique la siguiente información:

   - **Tipo**. Selecciona **Expiración de archivo** del menú desplegable.

   - **Directorio de expiración**. Selecciona un directorio donde expirarán los archivos.

     > [!Warning]
     > No seleccione un directorio que se encuentre dentro del ámbito de la tarea tal como se define en el paso anterior. Si lo hace, podría generar un bucle iterativo que podría provocar la inestabilidad del sistema y la pérdida de datos.

5. De forma opcional, en la pestaña **Notificación**, haz clic en **Agregar** para enviar notificaciones por correo electrónico, registrar un evento o ejecutar un comando o script una cantidad mínima de días especificada antes de que la tarea ejecute una acción en un archivo.

   - En el cuadro combinado **Número de días antes de ejecutar la tarea para enviar una notificación**, escribe o selecciona un valor para especificar el número mínimo de días antes de actuar en un archivo para enviar una notificación.

     > [!Note]
     > Las notificaciones solo se envían cuando se ejecuta una tarea. Si el número de días especificado para enviar una notificación no coincide con una tarea programada, la notificación se enviará el día de la tarea programada anterior.

   - Para configurar notificaciones por correo electrónico, haz clic en la pestaña **Mensaje de correo electrónico** y escribe la siguiente información:

     - Para notificar a los administradores que se ha alcanzado un umbral, selecciona la casilla de verificación **Enviar mensaje de correo electrónico a los siguientes administradores** y luego escribe los nombres de las cuentas administrativas que recibirán las notificaciones. Usa el formato <em>account@domain</em> y usa punto y coma para separar varias cuentas.  

     - Para enviar un correo electrónico a la persona cuyos archivos están a punto de expirar, selecciona la casilla de verificación **Enviar mensaje de correo electrónico al usuario cuyos archivos están a punto de expirar**.

     - Para configurar el mensaje, edita la línea de asunto y el cuerpo del mensaje predeterminados que se muestran. El texto que aparece entre paréntesis inserta información variable sobre el evento de cuota que originó la notificación. Por ejemplo, la variable **\[propietario del archivo de origen\]** inserta el nombre del usuario cuyo archivo está a punto de expirar. Para insertar variables adicionales en el texto, haz clic en **Insertar variable**.

     - Para adjuntar una lista de los archivos que están a punto de expirar, haz clic en **Adjuntar a la lista de correo electrónico de los archivos en los que se va a realizar la acción**y escribe o selecciona un valor para **Número máximo de archivos en la lista**.

     - Para configurar los encabezados adicionales (incluidos De, Cc, Bcc y Responder-a), haz clic en **Encabezados de correo electrónico adicionales**.  

   - Para registrar un evento, haz clic en la pestaña **Registro de eventos** y selecciona la casilla de verificación **Enviar advertencia al registro de eventos** y luego edita la entrada de registro predeterminada.  

   - Para ejecutar un comando o script, haz clic en la pestaña **Comando** y selecciona la casilla de verificación **Ejecutar este comando o script**. Luego escribe el comando o haz clic en **Examinar** para buscar la ubicación en la que está almacenado el script. También puedes especificar argumentos de comando; para ello, selecciona un directorio de trabajo para el comando o script o modifica la configuración de seguridad de comandos.

6. Otra alternativa es usar la pestaña **Informe** para generar uno o varios registros o informes de almacenamiento.

   - Para generar registros, selecciona la casilla de verificación **Generar registro** y luego selecciona uno o varios registros disponibles.  

   - Para generar informes, selecciona la casilla de verificación **Generar un informe** y luego selecciona uno o varios formatos de informe disponibles.  

   - Para crear informes de almacenamiento o registros generados por correo electrónico, selecciona la casilla de verificación **Enviar informes a los siguientes administradores** y escribe uno o varios destinatarios de correos electrónicos administrativos con el formato <em>account@domain</em>. Usa punto y coma para separar varias direcciones.

     > [!Note]
     > El informe se guarda en la ubicación predeterminada para los informes de incidentes, que se puede modificar en el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.
        
7. Otra opción es usar la pestaña **Condiciones** para ejecutar esta tarea solo en archivos que reúnan una serie de condiciones definidas. Las siguientes configuraciones están disponibles:

    -   **Condiciones de propiedad**. Haz clic en **Agregar** para crear una nueva condición basada en la clasificación del archivo. Esto abrirá el cuadro de diálogo **Condición de propiedad**, que te permite seleccionar una propiedad, un operador para realizar una acción en la propiedad y el valor con el que comparar la propiedad. Después de hacer clic en **Aceptar**, puedes crear condiciones adicionales o editar o quitar una condición existente.

    -   **Días desde la última modificación del archivo**. Haz clic en la casilla de verificación y luego escribe un número de días en el cuadro de número. Esto dará como resultado que la tarea de administración de archivos solo se aplicará a los archivos que no se han modificado durante un tiempo superior al número de días especificado.

    -   **Días desde el último acceso al archivo**. Haz clic en la casilla de verificación y luego escribe un número de días en el cuadro de número. Si el servidor está configurado para hacer un seguimiento de las marcas de tiempo de la última vez a la que se tuvo acceso a los archivos, esto dará como resultado que la tarea de administración de archivos solo se aplicará a los archivos a los que no se ha tenido acceso durante un tiempo superior al número de días especificado. Si el servidor no está configurado para realizar un seguimiento de las marcas de tiempo, esta condición no tendrá efecto.

    -   **Días desde que se creó el archivo**. Haz clic en la casilla de verificación y luego escribe un número de días en el cuadro de número. Esto dará como resultado que la tarea solo se aplicará a los archivos que se crearon como mínimo en el número de días especificado.  

    -   **Vigente a partir de**. Establece una fecha para que esta tarea de administración de archivos comience con el procesamiento de archivos. Esta opción es útil para retrasar la tarea hasta que hayas tenido la oportunidad de notificar a los usuarios o realizar otros preparativos con antelación.

8. En la pestaña **Programación**, haz clic en **Crear una programación** y luego en **Programación**, haz clic en **Nueva**. Se muestra una programación predeterminada para las 9:00 a.m. todos los días, pero puedes modificar la programación predeterminada. Cuando haya terminado de configurar la programación, haz clic en **Aceptar** y luego vuelve a hacer clic en **Aceptar**.

## <a name="see-also"></a>Consulte también

-   [Administración de clasificaciones](classification-management.md)
-   [Tareas de administración de archivos](file-management-tasks.md)