---
title: Crear contenido de paquetes de datos de servidor para contenido web y de archivos (opcional)
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8cd284a83736d17859968947f381af171fd6bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817176"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Crear contenido de paquetes de datos de servidor para contenido web y de archivos (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para hacer un hash previo contenido en servidores Web y de archivo y, a continuación, crear paquetes de datos para importar en el servidor de caché hospedada. 

Este procedimiento es opcional porque no es necesario para prehash y precarga de contenido en los servidores de caché hospedada. Si no precargar contenido, se agregan automáticamente a la memoria caché hospedada datos como los clientes lo descargan a través de la conexión WAN.

Este procedimiento proporciona instrucciones para la aplicación de hash previo contenido en servidores de archivos y servidores Web. Si no tiene uno de esos tipos de servidores de contenido, no es necesario seguir las instrucciones para ese tipo de servidor de contenido.

>[!IMPORTANT]
>Antes de realizar este procedimiento, debe instalar y configurar BranchCache en los servidores de contenido. Además, si tiene previsto cambiar el secreto de servidor en un servidor de contenido, hacerlo antes de pre\-hash de contenido: modificar el secreto de servidor previamente invalida\-generan hashes.

Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores.

## <a name="to-create-content-server-data-packages"></a>Para crear paquetes de datos de servidor de contenido

1. En cada servidor de contenido, busque las carpetas y archivos que desea hacer un hash previo y agregar a un paquete de datos. Identifique o cree una carpeta donde desea guardar el paquete de datos más adelante en este procedimiento.

2. En el equipo del servidor, abra Windows PowerShell con privilegios de administrador.

3. Realice una o ambas de las acciones siguientes, dependiendo de los tipos de servidores de contenido que tiene:

    > [!NOTE]
    > El valor de – ruta de acceso de parámetro es la carpeta donde se encuentra el contenido. Debe reemplazar los valores de ejemplo en los siguientes comandos con una ubicación de carpeta válida en el servidor de contenido que contiene los datos que desea hacer un hash previo y agregar a un paquete.
  
    - Si el contenido que desea hacer un hash previo está en un servidor de archivos, escriba el siguiente comando y, a continuación, presione ENTRAR.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Si el contenido que desea hacer un hash previo está en un servidor Web, escriba el siguiente comando y, a continuación, presione ENTRAR.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Cree el paquete de datos ejecutando el siguiente comando en cada uno de los servidores de contenido. Reemplace el valor de ejemplo \(D:\\temp\) para el parámetro – Destination con la ubicación que identifica o que creó al principio de este procedimiento.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Desde el servidor de contenido, acceder al recurso compartido en los servidores de caché hospedada en la que desea precargar contenido y copie los paquetes de datos a los recursos compartidos en los servidores de caché hospedada.

Para continuar con esta guía, consulte [importar paquetes de datos en el servidor de caché hospedada &#40;opcional&#41;](9-Bc-Import-Data.md).

