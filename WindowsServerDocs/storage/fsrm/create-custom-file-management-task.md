---
title: Crear una tarea de administración de archivos personalizada
description: En este artículo se describe cómo crear una tarea de administración de archivos personalizada y tareas personalizadas.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7fcfd9958787817a6945b220115c507f48924cbf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954682"
---
# <a name="create-a-custom-file-management-task"></a>Crear una tarea de administración de archivos personalizada

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La expiración no es siempre la acción deseada que se realizará en los archivos. Las tareas de administración de archivos también permiten ejecutar comandos personalizados.

> [!Note]
> En este procedimiento se supone que está familiarizado con las tareas de administración de archivos y, por lo tanto, solo cubre la pestaña **acción** en la que se configura la configuración personalizada.

## <a name="to-create-a-custom-task"></a>Para crear una tarea personalizada

1.  Haga clic en el nodo **Tareas de administración de archivos**.

2.  Haga clic con el botón secundario en **tareas de administración de archivos**y haga clic en **crear tarea de administración de archivos** (o haga clic en **crear tarea de administración de archivos** en el panel **acciones** ). Se abrirá el cuadro de diálogo **Crear tarea de administración de archivos**.

3.  En la pestaña **Acción**, especifique la siguiente información:

    -   **Tipo**. Seleccione **personalizado** en el menú desplegable.
    -   **Archivo ejecutable**. Escriba o busque un comando que se ejecutará cuando la tarea de administración de archivos procese los archivos. Este ejecutable se debe establecer para que solo los administradores y el sistema puedan escribir en él. Si cualquier otro usuario tiene acceso de escritura al archivo ejecutable, no se ejecutará correctamente.
    -   **Configuración del comando**. Para configurar los argumentos que se pasan al archivo ejecutable cuando un trabajo de administración de archivos procesa archivos, edite el cuadro de texto con la etiqueta **arguments**. Para insertar variables adicionales en el texto, coloque el cursor en la ubicación del cuadro de texto donde desea insertar la variable, seleccione la variable que desea insertar y, a continuación, haga clic en **Insertar variable**. El texto entre paréntesis inserta información de variables que el ejecutable puede recibir. Por ejemplo, la \[ variable de ruta de acceso del archivo de origen \] inserta el nombre del archivo que debe ser procesado por el ejecutable. Opcionalmente, haga clic en el botón **Directorio de trabajo** para especificar la ubicación del archivo ejecutable personalizado.
    -   **Seguridad de comandos**. Configure los valores de seguridad para este archivo ejecutable. De forma predeterminada, el comando se ejecuta como servicio local, que es la cuenta más restrictiva disponible.

4.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de clasificaciones](classification-management.md)
-   [Tareas de administración de archivos](file-management-tasks.md)