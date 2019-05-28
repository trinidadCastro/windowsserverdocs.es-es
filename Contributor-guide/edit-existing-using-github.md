---
title: Editar un artículo existente de Windows Server mediante GitHub y Visual Studio Code
description: Cómo editar una existente artículos relacionados con Windows Server, mediante GitHub y Visual Studio Code, como empleado de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d2d95cc28089ceb74bf9690f6bd78611e7d9a27a
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624587"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Editar un artículo existente de Windows Server mediante GitHub y Visual Studio Code

Hay dos ubicaciones independientes, que es donde guardamos el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs) mientras la otra es privada (windowsserverdocs-pr). Quién es usted determina qué ubicación que contribuye al:

- **Soy empleado de Microsoft.** Como empleado de Microsoft, dispone de opciones, según lo que está intentando hacer:

    - **Crear un nuevo artículo.** Para crear un nuevo artículo, debe crear y configurar su cuenta de GitHub y herramientas, bifurcar y clonar el repositorio windowsserverdocs-pr, configure la rama remota, crear el artículo y, por último, cree una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, puede seguir las instrucciones de la [crear artículos nuevos de Windows Server mediante GitHub y Visual Studio Code](create-new-using-github.md) artículo.

    - **Asegúrese de realizar grandes cambios en un artículo existente.** Para realizar cambios sustanciales en un artículo existente, puede seguir las instrucciones de este artículo.

    - **Realizar cambios menores a un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones de la [actualizar artículos existentes de Windows Server mediante un explorador web y GitHub](github-browser-updates.md) artículo.

- **No soy empleado de Microsoft.** Como un empleado que no sean de Microsoft, debe contribuir a la ubicación pública. Para obtener información acerca de cómo hacerlo, consulte el [que contribuyen a la documentación técnica de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) artículo.

## <a name="switch-your-repo-and-create-a-new-branch"></a>Cambie el repositorio y cree una nueva rama

Siga estos pasos para editar un artículo existente.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Crear una nueva rama y busque el archivo que desea actualizar

Antes de empezar a trabajar en su contenido, debe en primer lugar cambie al repositorio windowsserverdocs-pr y, a continuación, busque el artículo que desee actualizar.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para crear una nueva rama en Git Bash

- Abra Git Bash y escriba los comandos (uno a la vez):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Se recomienda encarecidamente la nomenclatura de la rama de algo obvio y único para que pueda encontrar de nuevo más tarde.

    Después de fin los comandos, deberá estar en la nueva rama y listo para editar el archivo.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Para buscar el artículo y realice las modificaciones

1. Abra Visual Studio Code y vaya a **archivo**, seleccione **Abrir carpeta**y, a continuación, vaya a la ubicación de GitHub de la carpeta que contiene el artículo que desee editar.

2. Desde el **Explorer** panel, seleccione el archivo.

3. Actualizar la información en la página y, a continuación, seleccione **archivo** > **guardar**.

### <a name="preview-your-text"></a>Obtener una vista previa de texto

Después de actualizar el texto, debe obtener una vista previa de los cambios para asegurarse de que aparecen correctamente.

#### <a name="to-preview-your-text"></a>Para obtener una vista previa de texto

1. En Visual Studio Code, seleccione uno de los **Preview** botones, desde la esquina superior derecha.

    ![icono de botón de vista previa](media/create-new-using-github/preview-button-full-page.png): Cambia a una vista previa de página completa de su contenido.

    ![icono de botón de vista previa](media/create-new-using-github/preview-button-side-by-side.png): Abre la página de vista previa junto a su página de trabajo en paralelo.

2. Asegúrese de que el artículo se analiza cómo esperar que se muestre.

    Una vez que esté seguro de que es correcta, puede confirmar los cambios y crear una solicitud de extracción para la publicación.

### <a name="commit-your-changes"></a>Confirme los cambios

Después asegúrese de que el texto es correcto, puede confirmar los cambios en la versión local del repositorio.

#### <a name="to-commit-your-changes"></a>Para confirmar los cambios

- Abra Git Bash y escriba los comandos (de uno en uno, quitar las etiquetas opcionales):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    El comando status de git opcional muestra qué archivos se han modificado como parte de esta confirmación. El git opcional de extracción extrae maestro ascendente hacia abajo los cambios de contenido más recientes de la rama master MicrosoftDocs, sincronización de su contenido con el contenido principal maestra local. Esto ayuda a mostrar los posibles conflictos de combinación antes de tiempo para que pueda corregirlos antes de llegar a la fase de incorporación de cambios.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Enviar una solicitud de incorporación de cambios para su revisión y publicación

Después de haber completado las actualizaciones, debe obtener aprobación desde el sistema de escritura (esperar un poco para esto) para la publicación.

#### <a name="to-submit-your-pull-request"></a>Para enviar la solicitud de incorporación de cambios

1. Vaya a https://github.com/MicrosoftDocs/windowsserverdocs-pr y seleccione el **solicitudes de extracción** ficha.

2. En el **revisores** área del panel derecho, seleccione el icono de engranaje y, a continuación, escriba el _windowsservercontent_ alias para su revisión.

    Un miembro de la _windowsservercontent_ alias se revise los cambios o agregue comentarios acerca de las cosas que deben cambiarse antes de combinar puede ocurrir.

3. Tipo **#sign-off** en los comentarios para que los revisores sepan va a entregar para su revisión y publicación. El **#sign-off** comentario:

    - Actualiza la etiqueta para la solicitud de incorporación de **do-not-merge** a **listo para combinar**.

    - Permite que los alias y escritores que se sabe que está preparado para recibir el contenido revisado.

    - Permite que los administradores de saben que tras la aprobación, el contenido está listo go live.

    >[!Important]
    >Después de agregar el comentario #sign-off, un miembro del equipo windowsservercontent revise el texto y los insertará dominar por lo que enviará con el siguiente programada de publicación para live (10:30 a.m. y 3:30 durante los días laborables).
    >
    >Si no agrega #sign-off como un comentario final para la incorporación de cambios, el contenido permanecerá en la cola sin que se va a insertar en Master y, finalmente, vida.

## <a name="related-information"></a>Información relacionada

Para obtener más información acerca de GitHub y el lenguaje de marcado, consulte:

### <a name="git-concepts"></a>Conceptos de GIT

- [Introducción manual de Git de guías de GitHub](https://guides.github.com/introduction/git-handbook/)

- [Proyectos de bifurcación de guías de GitHub](https://guides.github.com/activities/forking/)

- [El flujo de GitHub de comprensión de las guías de GitHub](https://guides.github.com/introduction/flow/)

- [Obtenga información sobre ramificaciones en Git](https://learngitbranching.js.org/ (excelente para aprendices visuales!))

### <a name="markdown"></a>Markdown

- [Nuestra guía de markdown interno](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Tutorial de GitHub externo,](https://www.markdowntutorial.com/)