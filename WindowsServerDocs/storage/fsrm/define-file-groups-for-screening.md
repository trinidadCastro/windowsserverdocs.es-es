---
title: Definir grupos de archivos para el filtrado
description: "En este artículo se describe cómo definir grupos de archivos para crear un espacio de nombres para un filtro de archivos, una excepción al filtro de archivos o archivos de almacenamiento de archivos por grupo de archivos"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6e043692500370b6c084a4db068027d13afc957f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="define-file-groups-for-screening"></a>Definir grupos de archivos para el filtrado

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *grupo de archivos* se usa para definir un espacio de nombres para un filtro de archivos, una excepción al filtro de archivos o un informe de almacenamiento de **Archivos por grupo de archivos**. Consta de un conjunto de patrones de nombres de archivos, que se agrupan según los siguientes criterios:

-   **Archivos incluidos**: archivos que pertenecen al grupo
-   **Archivos excluidos**: archivos que no pertenecen al grupo

> [!Note]
> Para mayor comodidad, puedes crear y editar grupos de archivos mientras editas las propiedades de los filtros de archivos, excepciones al filtro de archivos, plantillas de filtro de archivos e informes de **Archivos por grupo de archivos**. Los cambios que realices en el grupo de archivos desde estas hojas de propiedades no están limitados al elemento actual en el que estás trabajando.

## <a name="to-create-a-file-group"></a>Para crear un grupo de archivos

1.  En **Administración del filtrado de archivos**, haz clic en el nodo **Grupos de archivos**.

2.  En el panel **Acciones**, haz clic en **Crear grupo de archivos**. Se abrirá el cuadro de diálogo **Crear propiedades del grupo de archivos**.

    (Como alternativa, mientras editas las propiedades de un filtro de archivos, una excepción al filtro de archivos, una plantilla de filtro de archivos o un informe de **Archivos por grupo de archivos**, en **Mantener grupos de archivos**, haz clic en **Crear**).

3.  En el cuadro de diálogo **Crear propiedades del grupo de archivos**, escribe un nombre para el grupo de archivos.

4.  Agregar archivos para incluir y excluir:

    -   Para cada conjunto de archivos que quieres incluir en el grupo de archivos, en el cuadro **Archivos incluidos**, escribe un patrón de nombre de archivo y luego haz clic en **Agregar**.
    -   Para cada conjunto de archivos que quieres excluir del grupo de archivos, en el cuadro **Archivos excluidos**, escribe un patrón de nombre de archivo y luego haz clic en **Agregar**.
        Ten en cuenta que se aplican reglas de caracteres comodín estándar, por ejemplo, **\*.exe** selecciona todos los archivos ejecutables.

5.  Haz clic en **Aceptar**.

## <a name="see-also"></a>Consulta también

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Crear un filtro de archivos](create-file-screen.md)
-   [Crear una excepción al filtro de archivos](create-file-screen-exception.md)
-   [Crear una plantilla de filtro de archivos](create-file-screen-template.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)


