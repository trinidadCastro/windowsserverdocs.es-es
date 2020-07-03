---
title: Editar artículos existentes de Windows Server mediante un explorador Web y GitHub
description: Cómo hacer cambios rápidos en la documentación de Windows Server existente mediante un explorador Web y GitHub, como un empleado de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 07/02/2020
ms.openlocfilehash: 61ceb05b6cb9602faaa4d17b4dc2d978cb571ddb
ms.sourcegitcommit: d754f9e39fc28e4c8768b77bcc9d02ffe2fa6535
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85911224"
---
# <a name="update-existing-windows-server-and-azure-stack-hci-articles-using-a-web-browser-and-github"></a>Actualice los artículos existentes de Windows Server y Azure Stack HCI mediante un explorador Web y GitHub

Hay dos ubicaciones independientes donde se conserva el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs), mientras que la otra es privada (windowsserverdocs-PR). A quién está determinando qué ubicación contribuye:

- **No soy un empleado de Microsoft.** Como empleado que no es de Microsoft, debe contribuir a la ubicación pública. Para obtener información sobre cómo hacerlo, vea el archivo [Contributing.MD](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

- **Soy empleado de Microsoft.** Como empleado de Microsoft, tiene opciones, en función de lo que está intentando hacer:

    - **Cree un nuevo artículo de marca.** Para crear un nuevo artículo de marca, debe crear y configurar la cuenta y las herramientas de GitHub, bifurcar y clonar el repositorio windowsserverdocs-PR, configurar la rama remota, crear el artículo y, por último, crear una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, consulte el artículo [creación de nuevos artículos de Windows Server con GitHub y Visual Studio Code](create-new-using-github.md) .

    - **Realizar cambios importantes en un artículo existente.** Para realizar cambios importantes en un artículo existente, puede seguir las instrucciones del artículo [edición de un artículo existente de Windows Server con GitHub y Visual Studio Code](edit-existing-using-github.md) .

    - **Realizar pequeños cambios en un artículo existente.** Para realizar cambios menores en un artículo existente, siga las instrucciones de este artículo.

    > [!IMPORTANT]
    > Todos los repositorios que publican en docs.microsoft.com han adoptado el [Código de conducta de código abierto de Microsoft](https://opensource.microsoft.com/codeofconduct/) o el [Código de conducta de .NET Foundation](https://dotnetfoundation.org/code-of-conduct). Para obtener más información, vea las [preguntas más frecuentes del Código de conducta](https://opensource.microsoft.com/codeofconduct/faq/). O bien póngase en contacto con [opencode@microsoft.com](mailto:opencode@microsoft.com) o [conduct@dotnetfoundation.org](mailto:conduct@dotnetfoundation.org) para enviar sus preguntas o comentarios.
    >
    > Las correcciones menores o aclaraciones de la documentación y los ejemplos de código de los repositorios públicos se rigen por los [Términos de uso del sitio docs.microsoft.com](https://docs.microsoft.com/legal/termsofuse). Los cambios importantes o nuevos generan un comentario en la solicitud de incorporación de cambios que le pide que acepte el contrato de licencia de colaboración (CLA) si no es un empleado de Microsoft. Necesitamos que rellene el formulario en línea para revisar o aceptar su solicitud de incorporación de cambios.

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>Ediciones rápidas de artículos existentes mediante GitHub y un explorador Web

Las ediciones rápidas optimizan el proceso de notificación y corrección de pequeños errores y omisiones en los documentos. A pesar de todos los esfuerzos, en ocasiones los documentos que publicamos _contienen_ algunos errores gramaticales y erratas.

1. Siga las instrucciones de [configuración](https://review.docs.microsoft.com/en-us/help/contribute/contribute-get-started-setup-github?branch=master)de la cuenta de github.

1. Vaya al repositorio privado de [Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs) o [Azure Stack HCl](https://github.com/MicrosoftDocs/azure-stack-docs-pr/tree/master/azure-stack/hci) . Los repositorios privados se supervisan con mayor frecuencia, por lo que el tiempo de aprobación es más rápido, se benefician de las comprobaciones de mayor calidad y ofrecen la posibilidad de ver el contenido en el entorno de ensayo, ya que aparecerá en nuestro sitio activo.

2. Navegue hasta el artículo que desea editar y, a continuación, seleccione el botón **Editar este archivo** .

   ![Botón Editar este archivo](media/github-browser-updates/edit-this-file.png)

3. Edite el tema, desplácese hacia abajo hasta la parte inferior, describa brevemente los cambios y, a continuación, seleccione **confirmar cambios**.

    ![Confirmar cambios con información de título](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>Enviar la solicitud de incorporación de cambios

Después de crear la solicitud de incorporación de cambios, debe enviarla para su aprobación y publicación.

### <a name="to-submit-your-pull-request"></a>Para enviar la solicitud de incorporación de cambios

1. En la página **abrir una solicitud de incorporación** de cambios, actualice el mensaje de confirmación para que sea más adecuado para un PR. Por ejemplo: corregir errores tipográficos en el primer párrafo.

2. Asegúrese de que solo se incluyan las confirmaciones y los archivos que espera incluir. Compruebe también que la solicitud de incorporación de existencias se dirige a la rama correcta del repositorio ascendente, ya sea maestra (normalmente) o una rama de versión (en ocasiones).

3. En el área **revisores** del panel derecho, seleccione el icono de engranaje y, a continuación, escriba _windowsservercontent_. Un miembro del alias será en punto para revisar los cambios y mezclar la solicitud de incorporación de cambios o agregar comentarios sobre las cosas que se deben cambiar antes de la combinación.

4. Seleccione **Crear solicitud de incorporación de cambios**. El nuevo PR se vincula a la rama de trabajo en la bifurcación. Hasta que se combine la solicitud de incorporación de cambios, las nuevas confirmaciones que inserte en la misma rama de trabajo en la bifurcación se incluirán automáticamente en la solicitud de incorporación de cambios. Se agrega una nueva etiqueta a la solicitud de incorporación de cambios que indica que no se deben **combinar**. Esto significa simplemente que el contenido todavía está en curso y no debe revisarse ni insertarse en el sitio activo.

5. Cuando esté listo para que un usuario del alias Revise su contenido, debe agregar el texto, **#sign** a los comentarios. Este comentario:

    - Actualiza la etiqueta de la solicitud de incorporación de cambios de **do-not-Merge** a **Ready-to Merge**.

    - Permite que el alias y los escritores sepan que está listo para que el contenido se revise.

    - Permite que los administradores sepan que después de la aprobación, el contenido está listo.
