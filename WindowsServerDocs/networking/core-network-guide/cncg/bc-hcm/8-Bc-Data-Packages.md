---
title: Crear contenido de paquetes de datos de servidor para contenido web y de archivos (opcional)
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e8e60e30cb31aeede47acdcfa74d7061d0cdb094
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952617"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Crear contenido de paquetes de datos de servidor para contenido web y de archivos (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para el hash de contenido en servidores web y de archivos y, a continuación, crear paquetes de datos para importar en el servidor de caché hospedada.

Este procedimiento es opcional porque no es necesario aplicar previamente hash y precargar contenido en los servidores de caché hospedada. Si no carga previamente el contenido, los datos se agregan automáticamente a la caché hospedada a medida que los clientes lo descargan a través de la conexión WAN.

Este procedimiento proporciona instrucciones para aplicar un algoritmo hash al contenido en los servidores de archivos y en los servidores Web. Si no tiene uno de estos tipos de servidores de contenido, no tiene que realizar las instrucciones para ese tipo de servidor de contenido.

>[!IMPORTANT]
>Antes de llevar a cabo este procedimiento, debe instalar y configurar BranchCache en los servidores de contenido. Además, si piensa cambiar el secreto del servidor en un servidor de contenido, hágalo antes de \- aplicar el algoritmo hash al contenido; al modificar el secreto del servidor, se invalidan los \- hashes generados previamente.

Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores.

## <a name="to-create-content-server-data-packages"></a>Para crear paquetes de datos del servidor de contenido

1. En cada servidor de contenido, busque las carpetas y los archivos que desea prehash y agregue a un paquete de datos. Identifique o cree una carpeta donde desee guardar el paquete de datos más adelante en este procedimiento.

2. En el equipo servidor, abra Windows PowerShell con privilegios de administrador.

3. Realice una o ambas de las siguientes acciones, en función de los tipos de servidores de contenido que tenga:

    > [!NOTE]
    > El valor del parámetro – Path es la carpeta donde se encuentra el contenido. Debe reemplazar los valores de ejemplo de los siguientes comandos por una ubicación de carpeta válida en el servidor de contenido que contenga los datos que desea precifrar y agregar a un paquete.

    - Si el contenido que desea prehash está en un servidor de archivos, escriba el siguiente comando y, a continuación, presione Entrar.

        ```
        Publish-BCFileContent -Path D:\share -StageData
        ```

    -   Si el contenido que desea prehash está en un servidor Web, escriba el siguiente comando y, a continuación, presione Entrar.

        ```
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```

4. Cree el paquete de datos mediante la ejecución del comando siguiente en cada uno de los servidores de contenido. Reemplace el valor de ejemplo \( D: \\ temp \) para el parámetro – Destination por la ubicación que identificó o creó al principio de este procedimiento.

    ```
    Export-BCDataPackage –Destination D:\temp
    ```

5. En el servidor de contenido, acceda al recurso compartido en los servidores de caché hospedada donde desea cargar previamente el contenido y copie los paquetes de datos en los recursos compartidos de los servidores de caché hospedada.

Para continuar con esta guía, consulte [Importar paquetes de datos en el servidor de caché hospedada &#40;&#41;opcional ](9-Bc-Import-Data.md).

