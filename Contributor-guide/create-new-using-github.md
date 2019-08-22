---
title: Cree nuevos artículos de Windows Server mediante GitHub y Visual Studio Code
description: Cómo crear nuevos artículos relacionados con Windows Server, mediante GitHub y Visual Studio Code, como empleado de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: f5e7e3d0cd17c64175fddaaac73c12daa2c2a32c
ms.sourcegitcommit: ffd9c42374c7448deb5f53f7a865cb427b5e4e9e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887946"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Cree nuevos artículos de Windows Server mediante GitHub y Visual Studio Code

Hay dos ubicaciones independientes donde se conserva el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs), mientras que la otra es privada (windowsserverdocs-PR). A quién está determinando qué ubicación contribuye:

- **Soy empleado de Microsoft.** Como empleado de Microsoft, tiene opciones, en función de lo que está intentando hacer:

    - **Cree un nuevo artículo de marca.** Para crear un nuevo artículo de marca, debe crear y configurar la cuenta y las herramientas de GitHub, bifurcar y clonar el repositorio windowsserverdocs-PR, configurar la rama remota, crear el artículo y, por último, crear una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, siga leyendo este artículo.

    - **Realizar cambios importantes en un artículo existente.** Para realizar cambios importantes en un artículo existente, puede seguir las instrucciones del artículo [edición de un artículo existente de Windows Server con GitHub y Visual Studio Code](edit-existing-using-github.md) .

    - **Realizar pequeños cambios en un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones del artículo [actualización de artículos existentes de Windows Server mediante un explorador Web y GitHub](github-browser-updates.md) .

- **No soy un empleado de Microsoft.** Como empleado que no es de Microsoft, debe contribuir a la ubicación pública. Para obtener información sobre cómo hacerlo, consulte el artículo sobre la [documentación técnica de colaboración con Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a trabajar en el repositorio, debe crear y configurar la cuenta de GitHub, configurar la comprobación en dos fases e instalar y configurar todas las herramientas necesarias. Si ya lo ha hecho, puede omitir la [sección bifurcar el repositorio](#fork-the-repository) de este artículo.

1. [Creación de una cuenta y un perfil de GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Vincular su cuenta a su cuenta de Microsoft y a las organizaciones de Microsoft y MicrosoftDocs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Activar la comprobación en dos fases](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autorización del sistema de compilación para acceder a la cuenta de GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Instalar Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Instalación de GitHub y sus herramientas](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Instalar el paquete de creación de docs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurar su propia versión del repositorio

Una vez que haya creado y configurado las herramientas y la cuenta de GitHub, puede crear una versión personal del repositorio. Aquí es donde creará las bifurcaciones y realizará todos los cambios.

### <a name="fork-the-repository"></a>Bifurque el repositorio.

Necesita una copia local de los archivos de origen, por lo que puede crear solicitudes de incorporación de cambios de la bifurcación en el repositorio de producción.

#### <a name="to-fork-the-repository"></a>Para bifurcar el repositorio

1. Inicie sesión en su cuenta de GitHub y vaya https://github.com/microsoftdocs/windowsserverdocs-pr a.

2. Seleccione **bifurcar**.

    ![Botón bifurcar resaltado en la página](media/create-new-using-github/fork-button.png)

3. Seleccione la cuenta de GitHub como ubicación de la bifurcación.

    ![Botón bifurcar resaltado en la página](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Clone el repositorio.

Debe clonar el repositorio para obtener una copia local del repositorio en el dispositivo local.

#### <a name="to-clone-the-repository"></a>Para clonar el repositorio

1. Vaya a https://github.com/settings/developers y, a continuación, seleccione tokens de **acceso personal** en el panel izquierdo.

2. Seleccione **generar nuevo token**, asigne un nombre significativo y único al token, seleccione todos los ámbitos disponibles y, a continuación, seleccione **generar token**.

3. Copie el token y colóquelo en un lugar seguro. Lo necesitará para el resto del proceso y, después de salir de la página, no podrá volver a ella.

4. Abra un comando de Git bash y cambie los directorios en los que desea almacenar el repositorio. Se recomienda usar, `C:\users\<your_name>\GitHub`.

5. Escriba los siguientes comandos con su información específica, de una en una, para clonar el repositorio y configurar las ramas remotas:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Ejecute este comando para asegurarse de que el equipo remoto está configurado correctamente:

    `git remote -v`

7. Debería ver algo parecido a este resultado:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Si el resultado remoto no tiene este aspecto, puede intentarlo de nuevo ejecutando `git remote remove upstream`.

## <a name="create-a-branch-and-a-new-article"></a>Creación de una rama y un nuevo artículo

Siga estos pasos para crear un artículo.

### <a name="create-a-new-branch-and-a-new-file"></a>Crear una nueva rama y un nuevo archivo

Antes de empezar a trabajar en el contenido, debe crear una nueva rama en el repositorio local.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para crear una nueva rama en Git Bash

- Abra git bash y escriba los comandos (de uno en uno):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Se recomienda asignar un nombre a la rama algo obvio y único para que pueda encontrarla de nuevo más adelante.

    Una vez finalizados los comandos, estará en la nueva rama y estará listo para crear el nuevo archivo. Solo tiene que cambiar al repositorio windowsserverdocs-PR una vez por cada instancia de Git bash. Si cierra git Bash, tendrá que volver a cambiar los directorios después de abrirlo.

#### <a name="to-create-a-new-file-in-your-branch"></a>Para crear un nuevo archivo en la rama

1. Abra el explorador de Windows, vaya al directorio de GitHub y cree un nuevo archivo de texto en la estructura de carpetas. El nombre de archivo debe estar en minúsculas y estar separado por guiones. Por ejemplo, _What-is-Windows-Server.MD_.

     También debe cambiar la extensión de archivo de. txt a. MD. En el mensaje de error que aparece, seleccione **sí** para guardar el archivo con la nueva extensión de archivo.

2. Abra Visual Studio Code, vaya a **archivo**, seleccione **Abrir carpeta**y, a continuación, vaya a la ubicación de github del archivo que creó en el paso 1.

3. En el panel **Explorador** , seleccione el nuevo archivo.

4. Agregue el texto a la página. Si va a crear un nuevo artículo, asegúrese de [Agregar las etiquetas de metadatos necesarias al artículo relacionado con Windows Server](metadata-requirements-for-articles.md).

5. Seleccione **archivo** > **Guardar**.

### <a name="preview-your-text"></a>Obtener una vista previa del texto

Después de agregar el texto al nuevo archivo, debe obtener una vista previa de los cambios para asegurarse de que aparecen correctamente.

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

Después de completar el artículo, debe obtener la aprobación del escritor (deje tiempo para esto) para la publicación.

#### <a name="to-submit-your-pull-request"></a>Para enviar la solicitud de incorporación de cambios

1. Vaya a https://github.com/MicrosoftDocs/windowsserverdocs-pr y seleccione la pestaña **solicitudes de incorporación** de cambios.

2. En el área revisores del panel derecho, seleccione el icono de engranaje y, a continuación, escriba el alias _windowsservercontent_ para revisarlo.

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

- [Aprendizaje] de la bifurcación de Git (https://learngitbranching.js.org/ (Excelente para los aprendizajes visuales.))

### <a name="markdown"></a>Markdown

- [Nuestra guía de Markdown interna](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Externo, tutorial de GitHub](https://www.markdowntutorial.com/)
