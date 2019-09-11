---
title: Editar un artículo de Windows Server existente mediante GitHub y Visual Studio Code
description: Cómo editar artículos existentes relacionados con Windows Server, con GitHub y Visual Studio Code, como empleado de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d681d2fc2b69898e841932a95738b89515ffb51a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865080"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Editar un artículo de Windows Server existente mediante GitHub y Visual Studio Code

Hay dos ubicaciones independientes donde se conserva el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs), mientras que la otra es privada (windowsserverdocs-PR). A quién está determinando qué ubicación contribuye:

- **Soy empleado de Microsoft.** Como empleado de Microsoft, tiene opciones, en función de lo que está intentando hacer:

    - **Cree un nuevo artículo de marca.** Para crear un nuevo artículo de marca, debe crear y configurar la cuenta y las herramientas de GitHub, bifurcar y clonar el repositorio windowsserverdocs-PR, configurar la rama remota, crear el artículo y, por último, crear una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, puede seguir las instrucciones del artículo [creación de nuevos artículos de Windows Server con GitHub y Visual Studio Code](create-new-using-github.md) .

    - **Realizar cambios importantes en un artículo existente.** Para realizar cambios importantes en un artículo existente, puede seguir las instrucciones de este artículo.

    - **Realizar pequeños cambios en un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones del artículo [actualización de artículos existentes de Windows Server mediante un explorador Web y GitHub](github-browser-updates.md) .

- **No soy un empleado de Microsoft.** Como empleado que no es de Microsoft, debe contribuir a la ubicación pública. Para obtener información sobre cómo hacerlo, consulte el artículo sobre la [documentación técnica de colaboración con Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="switch-your-repo-and-create-a-new-branch"></a>Cambiar el repositorio y crear una nueva rama

Siga estos pasos para editar un artículo existente.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Cree una nueva rama y busque el archivo que desea actualizar.

Antes de empezar a trabajar en el contenido, primero debe cambiar al repositorio windowsserverdocs-PR y, a continuación, buscar el artículo que quiere actualizar.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para crear una nueva rama en Git Bash

- Abra git bash y escriba los comandos (de uno en uno):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Se recomienda asignar un nombre a la rama algo obvio y único para que pueda encontrarla de nuevo más adelante.

    Una vez finalizados los comandos, estará en la nueva rama y estará listo para editar el archivo.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Para buscar el artículo y hacer las modificaciones

1. Abra Visual Studio Code, vaya a **archivo**, seleccione **Abrir carpeta**y, a continuación, vaya a la ubicación de github de la carpeta que contiene el artículo que desea editar.

2. En el panel **Explorador** , seleccione el archivo.

3. Actualice la información de la página y, a continuación, seleccione **archivo** > **Guardar**.

### <a name="preview-your-text"></a>Obtener una vista previa del texto

Después de actualizar el texto, debe obtener una vista previa de los cambios para asegurarse de que aparecen correctamente.

#### <a name="to-preview-your-text"></a>Para obtener una vista previa del texto

1. En Visual Studio Code, seleccione uno de los botones de **vista previa** en la esquina superior derecha.

    ![icono de botón de vista previa](media/create-new-using-github/preview-button-full-page.png): Cambia a una vista previa de página completa del contenido.

    ![icono de botón de vista previa](media/create-new-using-github/preview-button-side-by-side.png): Abre la página de vista previa junto a la página de trabajo, en paralelo.

2. Asegúrese de que el artículo tiene la apariencia esperada.

    Una vez que esté seguro de que parece correcto, puede confirmar los cambios y crear una solicitud de incorporación de cambios para la publicación.

### <a name="commit-your-changes"></a>Confirmar los cambios

Después de asegurarse de que el texto se ve bien, puede confirmar los cambios en la versión local del repositorio.

#### <a name="to-commit-your-changes"></a>Para confirmar los cambios

- Abra git bash y escriba los comandos (de uno en uno, quitando las etiquetas opcionales):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    El comando de estado de Git opcional muestra los archivos que se han modificado como parte de esta confirmación. El maestro opcional de extracción ascendente de extracción extrae los cambios de contenido más recientes de la rama MicrosoftDocs Master, sincronizando el contenido local con el contenido maestro principal. Esto ayuda a mostrar los posibles conflictos de combinación con anterioridad para que pueda corregirlos antes de llegar a la fase de PR.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Enviar una solicitud de incorporación de cambios para su revisión y publicación

Una vez completadas las actualizaciones, debe obtener la aprobación del escritor (deje tiempo para esto) para la publicación.

#### <a name="to-submit-your-pull-request"></a>Para enviar la solicitud de incorporación de cambios

1. Vaya a https://github.com/MicrosoftDocs/windowsserverdocs-pr y seleccione la pestaña **solicitudes de incorporación** de cambios.

2. En el área **revisores** del panel derecho, seleccione el icono de engranaje y, a continuación, escriba el alias _windowsservercontent_ para revisarlo.

    Un miembro del alias _windowsservercontent_ revisará los cambios o agregará comentarios sobre las cosas que deben cambiarse antes de que se produzca la combinación.

3. Escriba **#sign-OFF** en los comentarios para que los revisores sepan que está entregando para su revisión y publicación. Comentario **de #sign** :

    - Actualiza la etiqueta de la solicitud de incorporación de cambios de **do-not-Merge** a **Ready-to Merge**.

    - Permite que el alias y los escritores sepan que está listo para que el contenido se revise.

    - Permite que los administradores sepan que después de la aprobación, el contenido está listo.

    >[!Important]
    >Después de agregar el comentario de #sign, un miembro del equipo de windowsservercontent revisará el texto y lo enviará al maestro para que salga de la siguiente publicación programada en vivo (10:9:30 y 3: las 16:30 días laborables).
    >
    >Si no agrega #sign como comentario final a su solicitud de incorporación de precio, el contenido permanecerá en la cola sin que se inserte en el maestro y, en última instancia, en vivo.

## <a name="related-information"></a>Información relacionada

Para obtener más información sobre GitHub y el lenguaje Markdown, consulte:

### <a name="git-concepts"></a>Conceptos de Git

- [Guías de GitHub: Introducción al manual de Git](https://guides.github.com/introduction/git-handbook/)

- [Guías de GitHub: bifurcar proyectos](https://guides.github.com/activities/forking/)

- [Guías de GitHub: Descripción del flujo de GitHub](https://guides.github.com/introduction/flow/)

- [Aprendizaje de la bifurcación de Git] (https://learngitbranching.js.org/ (Excelente para los aprendizajes visuales.))

### <a name="markdown"></a>Markdown

- [Nuestra guía de Markdown interna](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Externo, tutorial de GitHub](https://www.markdowntutorial.com/)