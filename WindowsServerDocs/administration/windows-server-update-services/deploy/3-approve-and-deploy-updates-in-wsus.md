---
title: 'Paso 3: aprobar e implementar actualizaciones en WSUS'
description: 'Tema de Windows Server Update Services (WSUS): aprobar e implementar actualizaciones en WSUS es el paso tres en un proceso de cuatro pasos para implementar WSUS.'
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 8d728ff9-170f-47e6-aefe-52be93315a75
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68ff4c893302167815e3e8368d8b03f97d9be131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361694"
---
# <a name="step-3-approve-and-deploy-updates-in-wsus"></a>Paso 3: Aprobar e implementar actualizaciones en WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los equipos que pertenecen a un grupo se comunican automáticamente con el servidor WSUS en las siguientes 24 horas, para obtener actualizaciones. La característica de informes de WSUS se puede usar para determinar si esas actualizaciones fueron implementadas en los equipos de prueba. Cuando las pruebas finalizan correctamente, se pueden aprobar las actualizaciones para los grupos de equipos de la organización que correspondan. La siguiente lista de comprobación describe los pasos para aprobar e implementar actualizaciones mediante el uso de la Consola de administración de WSUS.

|Tarea|Descripción|
|----|--------|
|[3,1. Aprobar e implementar actualizaciones de WSUS @ no__t-0|Apruebe e implemente actualizaciones de WSUS mediante el uso de la Consola de administración de WSUS.|
|[3,2. Configurar reglas de aprobación automática @ no__t-0|Configure WSUS para aprobar automáticamente la instalación de actualizaciones para grupos seleccionados y el método para aprobar las revisiones de las actualizaciones existentes.|
|[3,3. Revisar las actualizaciones instaladas con informes de WSUS @ no__t-0|Revise las actualizaciones que se instalaron, los equipos que recibieron esas actualizaciones y otros detalles mediante el uso de la característica de informes de WSUS.|

## <a name="BKM_3.1."></a>3,1. Aprobar e implementar actualizaciones de WSUS
Use el siguiente procedimiento para aprobar e implementar actualizaciones.

#### <a name="to-approve-and-deploy-wsus-updates"></a>Para aprobar e implementar actualizaciones de WSUS

1.  En la Consola de administración de WSUS, haga clic en **Actualizaciones**. En el panel derecho, se muestra un resumen de estado de actualización para **Todas las actualizaciones**, **Actualizaciones críticas**, **Actualizaciones de seguridad** y **Actualizaciones de WSUS**.

2.  En la sección **Todas la actualizaciones** , haga clic en **Actualizaciones que los equipos necesitan**.

3.  En la lista de actualizaciones, seleccione las que desee aprobar para su instalación en el grupo de equipos de prueba. Podrá encontrar información sobre una actualización seleccionada, en el panel inferior del panel **Actualizaciones** . Para seleccionar varias actualizaciones contiguas, mantenga presionada la tecla **MAYÚS** mientras hace clic en los nombres de las actualizaciones. Para seleccionar varias actualizaciones no contiguas, mantenga presionada la tecla **CTRL**, mientras hace clic en los nombres de las actualizaciones.

4.  Haga clic con el botón secundario y, a continuación, haga clic en **Aprobar**.

5.  En el cuadro de diálogo **Aprobar actualizaciones** , seleccione el grupo de prueba y, a continuación, haga clic en la flecha abajo.

6.  Haga clic en **Aprobada para su instalación**y, a continuación, en **Aceptar**.

7.  Aparecerá la ventana **Progreso de la aprobación** , que muestra el progreso de las tareas relacionadas con la aprobación de actualizaciones. Cuando se haya completado el proceso de aprobación, haga clic en **Cerrar**.

## <a name="BKM_3.2.a."></a>3,2. Configurar reglas de aprobación automática
Las aprobaciones automáticas permiten especificar cómo aprobar automáticamente la instalación de actualizaciones de determinados grupos y como aprobar las revisiones de las actualizaciones existentes.

#### <a name="to-configure-automatic-approvals"></a>Para configurar aprobaciones automáticas

1.  En la consola de administración de WSUS, en **Update Services**, expanda el servidor WSUS y luego haga clic en **Opciones**. Se abre la ventana **Opciones**.

2.  En **Opciones**, haga clic en **Aprobaciones automáticas**. Se abre el cuadro de diálogo Aprobaciones automáticas.

3.  En **Reglas de actualización**, haga clic en **Nueva regla**. Se abre el cuadro de diálogo **Agregar regla** .

4.  En **Agregar regla**, en **paso 1: seleccione Propiedades**, seleccione una opción única o una combinación de opciones de lo siguiente:

    -   **Cuando una actualización está en una clasificación específica**

    -   **Cuando una actualización está en un producto específico**

    -   **Establecer una fecha límite para la aprobación**

5.  En **paso 2: editar las propiedades**, haga clic en cada una de las opciones que aparecen y, a continuación, seleccione las opciones adecuadas para cada una.

6.  En @no__t 0Step 3: Especifique un nombre @ no__t-0, escriba un nombre para la regla y, a continuación, haga clic en **Aceptar**.

7.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo Aprobaciones automáticas.

## <a name="BKM_3.3."></a>3,3. Revisar actualizaciones instaladas con informes de WSUS
24 horas después de aprobar las actualizaciones, puede usar la característica de informes de WSUS para determinar si las actualizaciones se implementaron en los equipos del grupo de prueba. Para comprobar el estado de una actualización, use la característica de informes de WSUS de la siguiente manera.

#### <a name="to-review-updates"></a>Para revisar actualizaciones

1.  En el panel de navegación de la Consola de administración de WSUS, haga clic en **Informes**.

2.  En la página **Informes** , haga clic en el informe **Resumen de estado de actualización** . Aparecerá la ventana **Informe de actualizaciones**.

3.  Si desea filtrar la lista de actualizaciones, seleccione los criterios que desea usar; por ejemplo, **Incluir actualizaciones en estas clasificaciones**y, a continuación, haga clic en **Ejecutar informe**.

4.  Verá el panel **Informe de actualizaciones**. Puede comprobar el estado de actualizaciones individuales al seleccionar la actualización en la sección izquierda del panel. La última sección del panel de informes muestra el resumen de estado de la actualización.

5.  Puede guardar o imprimir este informe, al hacer clic en el icono correspondiente de la barra de herramientas.

6.  Una vez que prueba las actualizaciones, puede aprobarlas para su instalación en los grupos de equipos correspondientes de la organización.
