---
title: Define File Groups for Screening
description: En este artículo se describe cómo definir grupos de archivos para crear un espacio de nombres para el filtro de archivos, la excepción al filtro de archivos o los informes de almacenamiento de grupos de archivos
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cbcf96a4ab5c6516b87ebde57f6adaf1cf4df17f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957482"
---
# <a name="define-file-groups-for-screening"></a>Define File Groups for Screening

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *grupo de archivos* se usa para definir un espacio de nombres para un filtro de archivos, una excepción al filtro de archivos o **archivos por grupo de archivos** informe de almacenamiento. Constan de un conjunto de patrones de nombre de archivo, que se se agrupan según las siguientes opciones:

-   **Archivos que se van a incluir**: archivos que pertenecen al grupo
-   **Archivos que se van a excluir**: archivos que no pertenecen al grupo

> [!Note]
> Para mayor comodidad, puede crear y editar grupos de archivos mientras edita las propiedades de los filtros de archivos, las excepciones del filtro de archivos, las plantillas de filtro de archivos y **archivos por grupo de archivos** informes. Cualquier cambio del grupo de archivos que se realice desde estas hojas de propiedades no se limita al elemento actual en el que se está trabajando.

## <a name="to-create-a-file-group"></a>Para crear un grupo de archivos

1.  En **Administración del filtrado de archivos**, haga clic en el nodo **grupos de archivos** .

2.  En el panel **acciones** , haga clic en **Crear grupo de archivos**. Se abrirá el cuadro de diálogo **crear propiedades del grupo de archivos** .

    (Como alternativa, mientras edita las propiedades de un filtro de archivos, una excepción al filtro de archivos, una plantilla de filtro de archivos o un informe de **archivos por grupo de archivos** , en **mantener grupos de archivos**, haga clic en **crear**).

3.  En el cuadro de diálogo **crear propiedades del grupo de archivos** , escriba un nombre para el grupo de archivos.

4.  Agregue los archivos que se van a incluir y los que se van a excluir:

    -   Para cada conjunto de archivos que desee incluir en el grupo de archivos, en el cuadro **archivos que se incluirán** , escriba un patrón de nombre de archivo y, a continuación, haga clic en **Agregar**.
    -   Para cada conjunto de archivos que desee excluir del grupo de archivos, en el cuadro **archivos que se van a excluir** , escriba un patrón de nombre de archivo y, a continuación, haga clic en **Agregar**.
        Tenga en cuenta que se aplican las reglas de caracteres comodín estándar, por ejemplo, ** \* . exe** selecciona todos los archivos ejecutables.

5.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Crear un filtro de archivos](create-file-screen.md)
-   [Crear una excepción al filtro de archivos](create-file-screen-exception.md)
-   [Crear una plantilla de filtro de archivos](create-file-screen-template.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)


