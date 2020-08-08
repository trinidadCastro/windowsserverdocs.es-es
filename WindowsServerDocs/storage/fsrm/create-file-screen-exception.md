---
title: Create a File Screen Exception
description: En este artículo se describe cómo crear una excepción de filtro de archivos.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9d8f0e4a8bc89312b846421c64b14518a9246aaa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942144"
---
# <a name="create-a-file-screen-exception"></a>Create a File Screen Exception

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En algunas ocasiones, es necesario permitir excepciones al filtrado de archivos. Por ejemplo, puede que desee bloquear los archivos de vídeo procedentes de un servidor de archivos pero necesite permitir que los integrantes de un grupo de aprendizaje guarden algunos archivos de vídeo en sus equipos como parte del programa de aprendizaje. Para permitir que los archivos que otros filtros de archivos bloqueen, cree una *excepción de filtro de archivos*.

Una excepción de filtro de archivos es un tipo especial de filtro de archivos que supera cualquier filtrado de archivos que, de otro modo, se aplicaría a una carpeta y a todas sus subcarpetas en una ruta de acceso de excepción designada. Es decir, crea una excepción a las reglas que se derivan de la carpeta principal.

> [!Note]
> No se puede crear una excepción a un filtro de archivos en una carpeta principal donde ya se ha definido un filtro de archivos. Es preciso asignar la excepción a una subcarpeta o realizar los cambios en el filtro de archivos existente.

<br />
Los grupos de archivos se asignan para determinar qué tipos de archivos se permitirán en la excepción al filtro de archivos.

## <a name="to-create-a-file-screen-exception"></a>Para crear una excepción de filtro de archivos

1.  En **Administración del filtrado de archivos**, haga clic en el nodo **filtros de archivos** .

2.  Haga clic con el botón derecho en **filtros de archivos**y haga clic en **Crear excepción al filtro** de archivos (o seleccione **Crear excepción al filtro de archivos** en el panel **acciones** ). Se abrirá el cuadro de diálogo **Crear excepción al filtro de archivos** .

3.  En el cuadro de texto **ruta de acceso** de la excepción, escriba o seleccione la ruta de acceso a la que se aplicará la excepción. La excepción se aplicará a la carpeta seleccionada y a todas sus subcarpetas.

4.  Para especificar qué archivos se excluirán del filtrado de archivos:

    -   En **grupos de archivos**, seleccione cada grupo de archivos que desee excluir del filtrado de archivos. (Para activar la casilla del grupo de archivos, haga doble clic en su etiqueta).
    -   Si desea ver los tipos de archivo que un grupo de archivos incluye y excluye, haga clic en la etiqueta del grupo de archivos y, a continuación, haga clic en **Editar**.
    -   Para crear un nuevo grupo de archivos, haga clic en **Crear**.

5.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Definir grupos de archivos para el filtrado](define-file-groups-for-screening.md)


