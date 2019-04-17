---
title: Crear el contenido de paquetes de datos de servidor para la Web y el archivo de contenido (opcional)
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f814bbac5c74081259d8eef6deda79d914bfec7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Crear el contenido de paquetes de datos de servidor para la Web y el archivo de contenido (opcional)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar este procedimiento para prehash contenido en los servidores Web y el archivo y, a continuación, crear paquetes de datos para importar en el servidor de la memoria caché hospedada. 

Este procedimiento es opcional, ya que no es necesarios para el contenido prehash y precarga en los servidores de la memoria caché hospedada. Si no precargar contenido, se agregan automáticamente a la memoria caché hospedada datos como los clientes descargan con la conexión WAN.

Este procedimiento proporciona instrucciones para prehashing contenido en servidores de archivos y servidores Web. Si no tienes uno de esos tipos de servidores de contenido, no es necesario que realizar las instrucciones para ese tipo de contenido del servidor.

>[!IMPORTANT]
>Antes de realizar este procedimiento, debes instalar y configurar BranchCache en los servidores de contenido. Además, si tienes previsto sobre cómo cambiar la clave secreta de servidor en un servidor de contenido, hacerlo antes de hash pre\ contenido: modificación del secreto servidor invalida hash previously\ generado.

Para realizar este procedimiento, debe ser miembro del grupo de administradores.

## <a name="to-create-content-server-data-packages"></a>Para crear paquetes de datos de servidor de contenido

1. En cada servidor contenido, busque las carpetas y archivos que quieres prehash y agrega un paquete de datos. Identificar o crear una carpeta donde quieres guardar el paquete de datos más adelante en este procedimiento.

2. En el equipo servidor, abre Windows PowerShell con privilegios de administrador.

3. Realiza una o ambas de las siguientes acciones, en función de los tipos de contenido servidores que tienes:

    > [!NOTE]
    > El valor de la ruta de acceso parámetro es la carpeta donde se encuentra el contenido. Debes reemplazar los valores de ejemplo en los siguientes comandos con una ubicación de carpeta válido en el servidor de contenido que contiene datos que quieres prehash y agregar a un paquete.
  
    - Si el contenido que desea prehash está en un servidor de archivos, escribe el siguiente comando y, a continuación, presione ENTRAR.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Si el contenido que desea prehash está en un servidor Web, escribe el siguiente comando y, a continuación, presione ENTRAR.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Crea el paquete de datos ejecutando el siguiente comando en cada uno de los servidores de contenido. Reemplaza el \(D:\\temp\) valor ejemplo – parámetro de destino con la ubicación que identifican o se crean al comienzo de este procedimiento.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Desde el servidor de contenido, acceso al recurso compartido en los servidores de la memoria caché hospedada donde quieras precargar contenido y copiar los paquetes de datos a los recursos compartidos en los servidores de la memoria caché hospedada.

Para continuar con esta guía, consulte [importar paquetes de datos en el servidor de la memoria caché hospedada & #40; opcional & #41;](9-Bc-Import-Data.md).

