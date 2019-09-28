---
title: Crear una tarea de administración de archivos personalizada
description: En este artículo describe cómo crear una tarea de administración de archivos personalizada y tareas personalizadas.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 894c4e3c0b9fa0fde7b749e6effce531c3f999bb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403175"
---
# <a name="create-a-custom-file-management-task"></a>Crear una tarea de administración de archivos personalizada

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La expiración no es siempre una acción que se quiera realizar en los archivos. Las tareas de administración de archivos también te permiten ejecutar comandos personalizados.

> [!Note]
> En este procedimiento se asume que estás familiarizado con las tareas de administración de archivos y, por tanto, solo cubre la pestaña **Acción** donde se establecen las opciones de configuración personalizadas.

## <a name="to-create-a-custom-task"></a>Para crear una tarea personalizada

1.  Haga clic en el nodo **Tareas de administración de archivos**.

2.  Haz clic con el botón derecho en **Tareas de administración de archivos**y luego haz clic en **Crear tarea de administración de archivos** (o haz clic en **Crear tarea de administración de archivos** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear tarea de administración de archivos**.

3.  En la ficha **Acción**, especifique la siguiente información:

    -   **Tipo**. Selecciona **Personalizar** en el menú desplegable.
    -   **Ejecutable**. Escribe o busca un comando para ejecutarse cuando la tarea de administración de archivos procese los archivos. Este archivo ejecutable debe establecerse para que puedan escribir en él únicamente los administradores y el sistema. Si otros usuarios tienen acceso de escritura al archivo ejecutable, no se ejecutará correctamente.
    -   **Configuración del comando**. Para configurar los argumentos pasados al archivo ejecutable cuando un trabajo de administración de archivos procesa los archivos, edita el cuadro de texto denominado **Argumentos**. Para insertar variables adicionales en el texto, coloca el cursor en la ubicación del cuadro de texto donde quieres insertar la variable, selecciona la variable que quieres insertar y luego haz clic en **Insertar variable**. El texto que aparece entre paréntesis inserta información de variables que el archivo ejecutable puede recibir. Por ejemplo, la variable de ruta de acceso de archivo \[Source @ no__t-1 inserta el nombre del archivo que debe ser procesado por el ejecutable. También puedes hacer clic en el botón **Directorio de trabajo** para especificar la ubicación del archivo ejecutable personalizado.
    -   **Seguridad del comando**. Establece la configuración de seguridad para este archivo ejecutable. De forma predeterminada, el comando se ejecuta como Servicio Local, que es la cuenta más restrictiva disponible.

4.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Administración de clasificaciones](classification-management.md)
-   [Tareas de administración de archivos](file-management-tasks.md)