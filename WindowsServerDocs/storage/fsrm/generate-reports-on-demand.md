---
title: Generar informes a petición
description: En este artículo se describe cómo generar informes a petición para analizar el uso del disco en el servidor
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e91bfbc306d1d2712f7b35ec48114b3a8a84ec83
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890996"
---
# <a name="generate-reports-on-demand"></a>Generar informes a petición

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Durante las operaciones diarias, puedes usar la opción **Generar informes ahora** para generar uno o varios informes a petición. Con estos informes, puedes analizar los distintos aspectos del uso del disco actual en el servidor. Los datos actuales se recopilan antes de generar los informes.

Cuando se generan informes a petición, los informes se guardan en la ubicación predeterminada que especifiques en el cuadro de diálogo **Opción del Administrador de recursos del servidor de archivos**, pero no se crea ninguna tarea de informe para usarla más adelante. Puedes ver los informes inmediatamente después de que se generan o enviar por correo electrónico los informes a un grupo de administradores.

> [!Note]
> Si decides abrir los informes inmediatamente, debes esperar mientras los informes se generan. El tiempo de procesamiento varía en función de los tipos de informes y del ámbito de los datos.

## <a name="to-generate-reports-immediately"></a>Para generar informes inmediatamente

1.  Haz clic en el nodo **Administración de informes de almacenamiento**.

2.  Haz clic con el botón derecho en **Administración de informes de almacenamiento** y luego haz clic en **Generar informes ahora** (o selecciona **Generar informes ahora** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Propiedades de la tarea de informes de almacenamiento**.

3.  Para seleccionar volúmenes o carpetas en los que se generarán informes:

    -   En **Ámbito**, haz clic en **Agregar**.
    -   Busca el volumen o carpeta en los que quieres generar los informes, selecciónalos y luego haz clic en **Aceptar** para agregar la ruta de acceso a la lista.
    -   Agregar todos los volúmenes o carpetas posibles que quieras incluir en los informes. (Para quitar un volumen o carpeta, haz clic en la ruta de acceso y luego haz clic en **Quitar**).

4.  Para especificar los informes que han de generarse:

     -   En **Datos del informe**, selecciona cada informe que quieres incluir.

    Para editar los parámetros de un informe:

    -   Haz clic en la etiqueta del informe y luego haz clic en **Editar parámetros**.
    -   En el cuadro de diálogo **Parámetros de informes**, edita los parámetros según sea necesario y luego haz clic en **Aceptar**.
    -  Para ver una lista de parámetros de todos los informes seleccionados, haz clic en **Revisar informes seleccionados** y luego haz clic en **Cerrar**.
 
5.  Para especificar los formatos para guardar los informes:

    -  En **Formatos de informes**, selecciona uno o varios formatos para los informes programados. De forma predeterminada, los informes se generan en Dynamic HTML (DHTML). También puedes seleccionar HTML, XML, CSV y formatos de texto. Los informes se guardan en la ubicación predeterminada para los informes a petición.

6.  Para enviar copias de los informes a los administradores por correo electrónico:

    -  En la pestaña **Entrega**, selecciona la casilla de verificación **Enviar informes a los siguientes administradores** y luego escribe los nombres de las cuentas administrativas que recibirán los informes. 
    - Usa el formato *account@domain* y usa punto y coma para separar varias cuentas.

7.  Para recopilar los datos y generar los informes, haz clic en **Aceptar**. Se abrirá el cuadro de diálogo **Generar informes de almacenamiento**.

8.  Selecciona cómo quieres generar los informes a petición:

    -   Si quieres ver los informes inmediatamente después de que se generen, haz clic en **Esperar a que se generen los informes y mostrarlos a continuación**. Cada informe se abre en su propia ventana.
    -   Para ver los informes más adelante, haz clic en **Generar informes en segundo plano**.

    Ambas opciones guardan los informes y, si habilitaste la entrega por correo electrónico, envían los informes a los administradores en los formatos que has seleccionado.

## <a name="see-also"></a>Vea también

-   [Administración de informes de almacenamiento](storage-reports-management.md)
-   [Opciones del Administrador de recursos del servidor de archivos de configuración](setting-file-server-resource-manager-options.md)

