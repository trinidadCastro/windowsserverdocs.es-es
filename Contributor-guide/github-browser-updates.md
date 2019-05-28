---
title: Edite artículos existentes de Windows Server con un explorador web y GitHub
description: Cómo hacer ediciones rápidas en la documentación de Windows Server existente con un explorador web y GitHub, como empleado de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d4574de7774a43092815a3d154020559c50fdcf9
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624591"
---
# <a name="update-existing-windows-server-articles-using-a-web-browser-and-github"></a>Actualizar los artículos existentes de Windows Server con un explorador web y GitHub

Hay dos ubicaciones independientes, que es donde guardamos el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs) mientras la otra es privada (windowsserverdocs-pr). Quién es usted determina qué ubicación que contribuye al:

- **No soy empleado de Microsoft.** Como un empleado que no sean de Microsoft, debe contribuir a la ubicación pública. Para obtener información acerca de cómo hacerlo, consulte el [Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) archivo.

- **Soy empleado de Microsoft.** Como empleado de Microsoft, dispone de opciones, según lo que está intentando hacer:

    - **Crear un nuevo artículo.** Para crear un nuevo artículo, debe crear y configurar su cuenta de GitHub y herramientas, bifurcar y clonar el repositorio windowsserverdocs-pr, configure la rama remota, crear el artículo y, por último, cree una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, consulte el [crear artículos nuevos de Windows Server mediante GitHub y Visual Studio Code](create-new-using-github.md) artículo.

    - **Asegúrese de realizar grandes cambios en un artículo existente.** Para realizar cambios sustanciales en un artículo existente, puede seguir las instrucciones de la [editar un artículo existente de Windows Server mediante GitHub y Visual Studio Code](edit-existing-using-github.md) artículo.

    - **Realizar cambios menores a un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones de este artículo.

    > [!IMPORTANT]
    > Todos los repositorios que publican en docs.microsoft.com han adoptado el [Microsoft código de conducta de código abierto](https://opensource.microsoft.com/codeofconduct/) o [código de conducta de .NET Foundation](https://dotnetfoundation.org/code-of-conduct). Para obtener más información, consulte el [código de conducta preguntas más frecuentes sobre](https://opensource.microsoft.com/codeofconduct/faq/). O póngase en contacto con [ opencode@microsoft.com ](mailto:opencode@microsoft.com), o [ conduct@dotnetfoundation.org ](mailto:conduct@dotnetfoundation.org) con cualquier pregunta o comentario.
    >
    > Las correcciones menores o aclaraciones de la documentación y ejemplos de código en repositorios públicos están cubiertos por el [docs.microsoft.com términos de uso](https://docs.microsoft.com/legal/termsofuse). Cambios importantes o nuevos generan un comentario en la solicitud de incorporación de cambios, pedirle que envíe un contrato de licencia de colaboración (CLA) en línea si no es un empleado de Microsoft. Necesitamos que rellene el formulario en línea antes de que podemos revisar o aceptar su solicitud de incorporación de cambios.

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>Ediciones rápidas en artículos existentes con GitHub y un explorador web

Las ediciones rápidas optimizan el proceso de notificación y corrección de pequeños errores y omisiones en documentos. A pesar de todos los esfuerzos, pequeños errores gramaticales y erratas _hacer_ lleguen a nuestros documentos publicados.

1. Ve a https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs.

2. Vaya al artículo que desea editar y, a continuación, seleccione el **editar este archivo** botón.

   ![Editar este botón de archivo](media/github-browser-updates/edit-this-file.png)

3. Editar el tema, a continuación, desplácese hacia abajo hasta la parte inferior, se describen brevemente los cambios y, a continuación, seleccione **Confirmar cambios**.

    ![Confirmar los cambios con información de título](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>Enviar la solicitud de incorporación de cambios

Después de crear la solicitud de incorporación de cambios, debe enviarlo para su aprobación y publicación.

### <a name="to-submit-your-pull-request"></a>Para enviar la solicitud de incorporación de cambios

1. En el **abrir una solicitud de incorporación de cambios** página, actualice el mensaje de confirmación para que sea más adecuado para un PR. Por ejemplo: Corregir el error de escritura en el primer párrafo.

2. Asegúrese de que solo las confirmaciones y esperar a que se incluirán los archivos se incluyen. Compruebe también que la solicitud se dirige a la rama adecuada en el repositorio ascendente, maestro (normalmente) o una rama de versión (en ocasiones).

3. En el **revisores** área del panel derecho, seleccione el icono de engranaje y, a continuación, escriba _windowsservercontent_. Será un miembro del alias en el punto para revisar los cambios y la extracción de mezcla cualquiera solicitar o agregue comentarios acerca de las cosas para cambiar antes de la fusión.

4. Seleccione **crear solicitud de incorporación de cambios**. La nueva incorporación de cambios está vinculado a la rama de trabajo en la bifurcación. Hasta que la solicitud se ha combinado, las nuevas confirmaciones que inserte en la misma rama de trabajo en la bifurcación se incluyen automáticamente en el PR. Se agrega una nueva etiqueta a la solicitud de incorporación de cambios que dice, **do-not-merge**. Esto significa simplemente que su contenido aún está en curso y no debe ser revisado o insertan en el sitio activo.

5. Cuando esté listo para que alguien del alias para revisar el contenido, debe agregar el texto, **#sign-off** a los comentarios. Este comentario:

    - Actualiza la etiqueta para la solicitud de incorporación de **do-not-merge** a **listo para combinar**.

    - Permite que los alias y escritores que se sabe que está preparado para recibir el contenido revisado.

    - Permite que los administradores de saben que tras la aprobación, el contenido está listo go live.