---
title: Programar un conjunto de informes
description: "En este artículo se describe cómo generar un conjunto de informes con regularidad"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 15b69e723af3a30375beae73782ab122c68f8880
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="schedule-a-set-of-reports"></a>Programar un conjunto de informes

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para generar un conjunto de informes con regularidad, debes programar una *tarea de informes*. La tarea de informes especifica los informes que se generan y los parámetros que se usan, los volúmenes y carpetas sobre los que realizar los informes, la frecuencia para generar los informes y los formatos de archivo en los que se guardan.

Los informes programados se guardan en una ubicación predeterminada, que puedes especificar en el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**. También tienes la opción de enviar los informes por correo electrónico a un grupo de administradores.

> [!Note]
> Para reducir al mínimo el impacto del procesamiento de informes en el rendimiento, genera varios informes en la misma programación para que los datos solo se recopilen una vez. Para agregar rápidamente informes a tareas de informes existentes, puedes usar la acción **Agregar o quitar informes para una tarea de informes**. Esto te permite agregar o quitar informes de varias tareas de informes y editar los parámetros de informes. Para cambiar las programaciones o direcciones de envío, debes editar tareas de informes individuales.

## <a name="to-schedule-a-report-task"></a>Para programar una tarea de informes

1.  Haz clic en el nodo **Administración de informes de almacenamiento**.

2.  Haz clic con el botón derecho en **Administración de informes de almacenamiento** y luego haz clic en **Programar una nueva tarea de informes** (o selecciona **Programar una nueva tarea de informes** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Propiedades de la tarea de informes de almacenamiento**.

3.  Para seleccionar volúmenes o carpetas en los que se generarán informes:

    -   En **Ámbito**, haz clic en **Agregar**.
    -   Busca el volumen o carpeta en los que quieres generar los informes, selecciónalos y luego haz clic en **Aceptar** para agregar la ruta de acceso a la lista.
    -   Agregar todos los volúmenes o carpetas posibles que quieras incluir en los informes. (Para quitar un volumen o carpeta, haz clic en la ruta de acceso y luego haz clic en **Quitar**).

4.  Para especificar los informes que han de generarse:

    -  En **Datos del informe**, selecciona cada informe que quieres incluir. De forma predeterminada, todos los informes se generan para una tarea de informes programada.

    Para editar los parámetros de un informe:

    -   Haz clic en la etiqueta del informe y luego haz clic en **Editar parámetros**.
    -   En el cuadro de diálogo **Parámetros de informes**, edita los parámetros según sea necesario y luego haz clic en **Aceptar**.

    -   Para ver una lista de los parámetros de todos los informes seleccionados, haz clic en **Revisar informes seleccionados**. Luego haz clic en **Cerrar**.

5.  Para especificar los formatos para guardar los informes:

    -  En **Formatos de informes**, selecciona uno o varios formatos para los informes programados. De forma predeterminada, los informes se generan en DynamicHTML (DHTML). También puedes seleccionar HTML, XML, CSV y formatos de texto. Los informes se guardan en la ubicación predeterminada para los informes programados.

6.  Para enviar copias de los informes a los administradores por correo electrónico:

    - En la pestaña **Entrega**, selecciona la casilla de verificación **Enviar informes a los siguientes administradores** y luego escribe los nombres de las cuentas administrativas que recibirán los informes. 
    - Usa el formato *account@domain* y usa punto y coma para separar varias cuentas.

7.  Para programar los informes:

    En la pestaña **Programación**, haz clic en **Crear una programación** y luego en **Programación**, haz clic en **Nueva**. Se muestra una programación predeterminada para las 9:00a.m. todos los días, pero puedes modificar la programación predeterminada.

    -   Para especificar una frecuencia de generación de informes, selecciona un intervalo en la lista desplegable **Programar tarea**.
        Puedes programar informes diarios, semanales o mensuales o generar los informes una sola vez. También puedes generar informes en el inicio del sistema o en el inicio de sesión o cuando el equipo haya estado inactivo durante un tiempo determinado.
    -   Para ofrecer información adicional de programación para el intervalo elegido, modifica o establece los valores en las opciones **Programar tarea**.
        Estas opciones cambian según el intervalo que elijas. Por ejemplo, para un informe semanal, puedes especificar el número de semanas entre los informes y los días de la semana que quieres generar los informes.
    -   Para especificar la hora del día en que quieres generar el informe, escribe o selecciona el valor en el cuadro **Hora de inicio**.
    -   Para acceder a opciones de programación adicionales (como por ejemplo, fecha de inicio y fecha de finalización de la tarea), haz clic en **Opciones avanzadas**.
    -   Para guardar la programación, haz clic en **Aceptar**.
    -  Para crear una programación adicional para una tarea (o modificar una existente), en la pestaña **Programación**, haz clic en **Editar programación**.

8.  Para guardar la tarea de informes, haz clic en **Aceptar**.

La tarea de informes se agrega al nodo **Administración de informes de almacenamiento**. Las tareas se identifican por los informes que se generan, el espacio de nombres sobre el que se crea los informes y la programación de informes.

Además, puedes ver el estado actual del informe (sin importar si el informe se ejecuta o no), la última hora de ejecución, el resultado de dicha ejecución y la próxima hora de ejecución programada.

## <a name="see-also"></a>Consulta también

-   [Administración de informes de almacenamiento](storage-reports-management.md)
-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


