---
title: Generate Reports on Demand
description: En este artículo se describe cómo generar informes a petición para analizar el uso de disco en el servidor.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a44db85dee565bfa3c6032d52b4d752c61c09cc0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961369"
---
# <a name="generate-reports-on-demand"></a>Generate Reports on Demand

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Durante las operaciones diarias, puede usar la opción **generar informes ahora** para generar uno o más informes a petición. Con estos informes puede analizar los distintos aspectos del uso actual del disco en el servidor. Los datos actuales se recopilan antes de generar los informes.

Cuando se generan informes a petición, los informes se guardan en una ubicación predeterminada que se especifica en el cuadro de diálogo de la **opción servidor de archivos administrador de recursos** , pero no se crea ninguna tarea de informe para su uso posterior. Puede ver los informes inmediatamente después de generarse o enviarlos por correo electrónico a un grupo de administradores.

> [!Note]
> Si elige abrir los informes inmediatamente, tendrá que esperar mientras se generan los informes. El tiempo de procesamiento varía según el tipo de informe y el ámbito de los datos.

## <a name="to-generate-reports-immediately"></a>Para generar informes inmediatamente

1. Haga clic en el nodo **Administración de informes de almacenamiento**.

2. Haga clic con el botón derecho en **Administración de informes de almacenamiento**y, a continuación, haga clic en **generar informes ahora** (o seleccione **generar informes ahora** en el panel **acciones** ). De este modo, se abrirá el cuadro de diálogo **Propiedades de la tarea de informes de almacenamiento**.

3. Para seleccionar volúmenes o carpetas en los que generar informes:

   -   En **ámbito**, haga clic en **Agregar**.
   -   Busque el volumen o la carpeta en la que desea generar los informes, selecciónelo y, a continuación, haga clic en **Aceptar** para agregar la ruta de acceso a la lista.
   -   Agregue todos los volúmenes o carpetas que desee incluir en los informes. (Para quitar un volumen o una carpeta, haga clic en la ruta de acceso y, a continuación, haga clic en **quitar**).

4. Para especificar qué informes se deben generar:

    -   En **datos del informe**, seleccione cada uno de los informes que desee incluir.

   Para editar los parámetros de un informe:

   -   Haga clic en la etiqueta del informe y, a continuación, haga clic en **Editar parámetros**.
   -   En el cuadro de diálogo **parámetros del informe** , modifique los parámetros según sea necesario y, a continuación, haga clic en **Aceptar**.
   -  Para ver una lista de parámetros de todos los informes seleccionados, haga clic en **revisar informes seleccionados** y, a continuación, haga clic en **cerrar**.

5. Para especificar los formatos con los que guardar los informes:

   -  En **formatos de informe**, seleccione uno o más formatos para los informes programados. De manera predeterminada, los informes se generan en un formato DHTML (HTML dinámico). También puede seleccionar los formatos HTML, XML, CSV y texto. Los informes se guardan en la ubicación predeterminada para los informes a petición.

6. Para enviar copias de los informes a los administradores mediante correo electrónico:

   - En la pestaña **entrega** , active la casilla **enviar informes a los siguientes administradores** y, a continuación, escriba los nombres de las cuentas administrativas que recibirán los informes.
   - Use el formato <em>account@domain</em> y use punto y coma para separar varias cuentas.

7. Para recopilar los datos y generar los informes, haga clic en **Aceptar**. De este modo, se abrirá el cuadro de diálogo **Generar informes de almacenamiento**.

8. Seleccione cómo desea generar los informes a petición:

   -   Si desea ver los informes inmediatamente después de generarlos, haga clic en **esperar a que se generen los informes y, a continuación, mostrarlos**. Cada informe se abre en su propia ventana.
   -   Para ver los informes más adelante, haga clic en **generar informes en segundo plano**.

   Ambas opciones guardan los informes y, si ha habilitado la entrega por correo electrónico, envíe los informes a los administradores con los formatos que haya seleccionado.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de informes de almacenamiento](storage-reports-management.md)
-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)

