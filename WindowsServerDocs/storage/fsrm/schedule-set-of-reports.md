---
title: Schedule a Set of Reports
description: En este artículo se describe cómo generar un conjunto de informes según una programación periódica.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4b8b0c66bc4f6e5445635deead1f79f7cc11309d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950469"
---
# <a name="schedule-a-set-of-reports"></a>Schedule a Set of Reports

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para generar un conjunto de informes según una programación regular, se programa una *tarea de informe.* La tarea de informe especifica los informes que se van a generar y los parámetros que se van a utilizar. los volúmenes y las carpetas que se van a notificar; frecuencia con la que se generan los informes y en qué formatos de archivo se guardan.

Los informes programados se guardan en una ubicación predeterminada, que se puede especificar en el cuadro de diálogo **Opciones de administrador de recursos del servidor de archivos** . También tiene la opción de entregar los informes por correo electrónico a un grupo de administradores.

> [!Note]
> Para minimizar el impacto del procesamiento de informes sobre el rendimiento, genere varios informes en la misma programación de modo que sólo haya que recopilar los datos una vez. Para agregar rápidamente informes a las tareas de informes existentes, puede usar la acción **Agregar o quitar informes para una tarea de informe** . Esta acción permite agregar o quitar informes de varias tareas de informes y editar los parámetros de informes. Para cambiar programaciones o direcciones de entrega, debe editar tareas de informes individuales.

## <a name="to-schedule-a-report-task"></a>Para programar una tarea de informe

1. Haga clic en el nodo **Administración de informes de almacenamiento**.

2. Haga clic con el botón secundario en **Administración de informes de almacenamiento** y después en **Programar una nueva tarea de informes** (o seleccione **Programar una nueva tarea de informes** en el panel **Acciones**). De este modo, se abrirá el cuadro de diálogo **Propiedades de la tarea de informes de almacenamiento**.

3. Para seleccionar volúmenes o carpetas en los que generar informes:

   -   En **ámbito**, haga clic en **Agregar**.
   -   Busque el volumen o la carpeta en la que desea generar los informes, selecciónelo y, a continuación, haga clic en **Aceptar** para agregar la ruta de acceso a la lista.
   -   Agregue todos los volúmenes o carpetas que desee incluir en los informes. (Para quitar un volumen o una carpeta, haga clic en la ruta de acceso y, a continuación, haga clic en **quitar**).

4. Para especificar qué informes se deben generar:

   -  En **datos del informe**, seleccione cada uno de los informes que desee incluir. De manera predeterminada, se generan todos los informes para una tarea de informes programada.

   Para editar los parámetros de un informe:

   -   Haga clic en la etiqueta del informe y, a continuación, haga clic en **Editar parámetros**.
   -   En el cuadro de diálogo **parámetros del informe** , modifique los parámetros según sea necesario y, a continuación, haga clic en **Aceptar**.

   -   Para ver una lista de parámetros de todos los informes seleccionados, haga clic en **revisar informes seleccionados**. A continuación, haga clic en **Cerrar**.

5. Para especificar los formatos con los que guardar los informes:

   -  En **formatos de informe**, seleccione uno o más formatos para los informes programados. De manera predeterminada, los informes se generan en un formato DHTML (HTML dinámico). También puede seleccionar los formatos HTML, XML, CSV y texto. Los informes se guardan en la ubicación predeterminada para los informes programados.

6. Para enviar copias de los informes a los administradores mediante correo electrónico:

   - En la pestaña **entrega** , active la casilla **enviar informes a los siguientes administradores** y, a continuación, escriba los nombres de las cuentas administrativas que recibirán los informes.
   - Use el formato <em>account@domain</em> y use punto y coma para separar varias cuentas.

7. Para programar los informes:

   En la pestaña **programación** , haga clic en **crear programación**y, a continuación, en el cuadro de diálogo **programación** , haga clic en **nuevo**. Esto muestra un conjunto de programación predeterminado de 9:00 A.M. diariamente, pero puede modificar la programación predeterminada.

   -   Para especificar una frecuencia para la generación de informes, seleccione un intervalo en la lista desplegable **tarea programada** .
       Puede programar informes diarios, semanales o mensuales, o generarlos sólo una vez. También puede generar informes en el inicio del sistema o al iniciar sesión, o cuando el equipo ha estado inactivo durante un tiempo especificado.
   -   Para proporcionar información de programación adicional para el intervalo elegido, modifique o establezca los valores en las opciones de **tarea de programación** .
       Estas opciones cambian según el intervalo que elija. Por ejemplo, para un informe semanal, puede especificar cuántas semanas deben transcurrir entre los informes y en qué días de la semana se generarán.
   -   Para especificar la hora del día en que desea generar el informe, escriba o seleccione el valor en el cuadro **hora de inicio** .
   -   Para obtener acceso a otras opciones de programación (incluidas las fechas de inicio y finalización de la tarea), haga clic en **avanzadas**.
   -   Para guardar la programación, haga clic en **Aceptar**.
   -  Para crear una programación adicional para una tarea (o modificar una programación existente), en la pestaña **programación** , haga clic en **Editar programación**.

8. Para guardar la tarea de informe, haga clic en **Aceptar**.

La tarea de informes se agrega al nodo **Administración de informes de almacenamiento**. Las tareas se identifican mediante los informes que se van a generar, el espacio de nombres en el que se va a notificar y la programación del informe.

Asimismo, puede ver el estado actual del informe (aunque no se esté ejecutando), la hora a la que se ejecutó por última vez y el resultado de esa ejecución, y la hora programada para la próxima ejecución.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de informes de almacenamiento](storage-reports-management.md)
-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


