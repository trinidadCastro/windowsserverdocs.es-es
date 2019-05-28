---
title: Creación de nuevos artículos de Windows Server mediante GitHub y Visual Studio Code
description: Cómo crear nuevos artículos relacionados con Windows Server, mediante GitHub y Visual Studio Code, como empleado de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d75bf135266a4783ab2617977b344782cea679ef
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624565"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Creación de nuevos artículos de Windows Server mediante GitHub y Visual Studio Code

Hay dos ubicaciones independientes, que es donde guardamos el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs) mientras la otra es privada (windowsserverdocs-pr). Quién es usted determina qué ubicación que contribuye al:

- **Soy empleado de Microsoft.** Como empleado de Microsoft, dispone de opciones, según lo que está intentando hacer:

    - **Crear un nuevo artículo.** Para crear un nuevo artículo, debe crear y configurar su cuenta de GitHub y herramientas, bifurcar y clonar el repositorio windowsserverdocs-pr, configure la rama remota, crear el artículo y, por último, cree una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, continúe leyendo este artículo.

    - **Asegúrese de realizar grandes cambios en un artículo existente.** Para realizar cambios sustanciales en un artículo existente, puede seguir las instrucciones de la [editar un artículo existente de Windows Server mediante GitHub y Visual Studio Code](edit-existing-using-github.md) artículo.

    - **Realizar cambios menores a un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones de la [actualizar artículos existentes de Windows Server mediante un explorador web y GitHub](github-browser-updates.md) artículo.

- **No soy empleado de Microsoft.** Como un empleado que no sean de Microsoft, debe contribuir a la ubicación pública. Para obtener información acerca de cómo hacerlo, consulte el [que contribuyen a la documentación técnica de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) artículo.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a trabajar en el repositorio, debe crear y configurar una cuenta de GitHub, configuren una comprobación de dos fases e instalar y configurar todas las herramientas necesarias. Si ya lo ha hecho, puede pasar a la [bifurcar la sección de repositorio](#fork-the-repository) de este artículo.

1. [Crear un perfil y cuenta de GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Vincular la cuenta de tu cuenta Microsoft y a las organizaciones de Microsoft y Microsoft docs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Activar la verificación de dos factores](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autorización al sistema de compilación para acceder a su cuenta de GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Instalación de Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Instale GitHub y sus herramientas](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Instalar el paquete de creación de Docs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurar su propia versión del repositorio

Una vez que ha creado y configurado la cuenta de GitHub y herramientas, puede crear una versión personal del repositorio. Esto es donde creará las ramas y realizar todos los cambios.

### <a name="fork-the-repository"></a>Bifurque el repositorio.

Necesita una copia local de los archivos de origen, para que pueda crear solicitudes de incorporación de cambios desde la bifurcación en el repositorio de producción.

#### <a name="to-fork-the-repository"></a>Para bifurcar el repositorio

1. Inicie sesión en su cuenta de GitHub y vaya a https://github.com/microsoftdocs/windowsserverdocs-pr.

2. Seleccione **bifurcación**.

    ![Botón de bifurcación resaltada en la página](media/create-new-using-github/fork-button.png)

3. Seleccione la cuenta de GitHub como la ubicación de la bifurcación.

    ![Botón de bifurcación resaltada en la página](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Clone el repositorio.

Necesario clonar el get del repositorio una copia local del repositorio en el dispositivo local.

#### <a name="to-clone-the-repository"></a>Para clonar el repositorio

1. Vaya a https://github.com/settings/developersy, a continuación, seleccione **tokens de acceso Personal** en el panel izquierdo.

2. Seleccione **generar nuevo token**, asigne un nombre único y descriptivo a su token, seleccione todos los ámbitos disponibles y, a continuación, seleccione **generar token**.

3. Copie el token y colóquelo en un lugar seguro. Necesitará esto para el resto del proceso y después de salir de la página, no podrá volver a ella.

4. Abra un comando de Git Bash y cambie al directorio donde desea almacenar el repositorio. Se recomienda usar, `C:\users\<your_name>\GitHub`.

5. Escriba los siguientes comandos con su información específica, una vez para clonar el repositorio y configurar sus ramas remotas:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Ejecute este comando para asegurarse de que el control remoto está configurado correctamente:

    `git remote -v`

7. Debería ver algo parecido a este resultado:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Si la salida remota no tener este aspecto, puede intentarlo mediante la primera ejecución `git remote remove upstream`.

## <a name="create-a-branch-and-a-new-article"></a>Crear una rama y un nuevo artículo

Siga estos pasos para crear un artículo.

### <a name="create-a-new-branch-and-a-new-file"></a>Crear una nueva rama y un nuevo archivo

Antes de empezar a trabajar en su contenido, debe crear una nueva rama en el repositorio local.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para crear una nueva rama en Git Bash

- Abra Git Bash y escriba los comandos (uno a la vez):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Se recomienda encarecidamente la nomenclatura de la rama de algo obvio y único para que pueda encontrar de nuevo más tarde.

    Después de fin los comandos, deberá estar en la nueva rama y está listo para crear el nuevo archivo. Solo deberá cambiar en el repositorio windowsserverdocs pr una vez por instancia de Git Bash. Si cierra Git Bash, deberá cambiar los directorios de nuevo después de abrirlo.

#### <a name="to-create-a-new-file-in-your-branch"></a>Para crear un nuevo archivo en la rama

1. Abra el Explorador de Windows, vaya a su directorio de GitHub y crear un nuevo archivo de texto en la estructura de carpetas. El nombre del archivo debe ser todo en minúsculas y separados por guiones. Por ejemplo, _what-es-windows-server.md_.

     También debe cambiar la extensión de archivo de .txt a. md. En el mensaje de error que aparece, seleccione **Sí** para guardar el archivo con la nueva extensión de archivo.

2. Abra Visual Studio Code y vaya a **archivo**, seleccione **Abrir carpeta**y, a continuación, vaya a la ubicación de GitHub del archivo que creó en el paso 1.

3. Desde el **Explorer** panel, seleccione el nuevo archivo.

4. Agregue el texto a la página y, a continuación, seleccione **archivo** > **guardar**.

### <a name="preview-your-text"></a>Obtener una vista previa de texto

Después de agregar el texto para el nuevo archivo, debe obtener una vista previa de los cambios para asegurarse de que aparecen correctamente.

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

Después de haber completado su artículo, debe obtener aprobación desde el sistema de escritura (esperar un poco para esto) para la publicación.

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
