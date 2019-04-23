---
title: Crear una excepción al filtro de archivos
description: En este artículo se describe cómo crear una excepción al filtro de archivos
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1f0e93cb2535862b9259d438de00c3b769c2282c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866306"
---
# <a name="create-a-file-screen-exception"></a>Crear una excepción al filtro de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En ocasiones, tienes que permitir excepciones al filtrado de archivos. Por ejemplo, es posible que quieras bloquear archivos de vídeo de un servidor de archivos, pero tienes que permitir a tu grupo de aprendizaje que guarde los archivos de vídeo para su aprendizaje asistido por PC. Para permitir archivos que otros filtros de archivos están bloqueando, crea un *excepción al filtro de archivos*.

Una excepción al filtro de archivos es un tipo especial de filtro de archivos que anula cualquier filtrado de archivos que, de lo contrario, se aplicaría a una carpeta y a todas sus subcarpetas en una ruta de acceso de excepción designada. Es decir, se crea una excepción a las reglas derivadas de una carpeta principal.

> [!Note]
> No puedes crear una excepción al filtro de archivos en una carpeta principal en la que ya se ha definido un filtro de archivos. Debes asignar la excepción a una subcarpeta o realizar cambios en el filtro de archivos existente.

<br />
Asigna grupos de archivos para determinar qué tipos de archivos se permitirán en la excepción al filtro de archivos.

## <a name="to-create-a-file-screen-exception"></a>Para crear una excepción al filtro de archivos

1.  En **Administración del filtrado de archivos**, haz clic en el nodo **Filtros de archivos**.

2.  Haz clic con el botón derecho en **Filtros de archivos** y haz clic en **Crear excepción al filtro de archivos** (o selecciona **Crear excepción al filtro de archivos** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear excepción al filtro de archivos**.

3.  En el cuadro de texto **Ruta de acceso de excepción**, escribe o selecciona la ruta de acceso a la que se aplicará la excepción. La excepción se aplicará a la carpeta seleccionada y a todas sus subcarpetas.

4.  Para especificar los archivos que se excluirán del filtrado de archivos:

    -   En **Grupos de archivos**, selecciona cada grupo de archivos que quieres excluir del filtrado de archivos. (Para seleccionar la casilla de verificación del grupo de archivos, haz doble clic en la etiqueta de grupo de archivos).
    -   Si desea ver los tipos de archivo incluye y excluidos de un grupo de archivos, haga clic en la etiqueta de grupo de archivos y haga clic en **editar**.
    -   Para crear un nuevo grupo de archivos, haga clic en **Crear**.

5.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Definir grupos de archivos para la selección](define-file-groups-for-screening.md)


