---
title: Instalar la característica BranchCache y configurar el servidor de la memoria caché hospedada por punto de conexión de servicio
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 854ff9f80a2221a857fab4e6ea7f7c8e6d51f1ef
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Instalar la característica BranchCache y configurar el servidor de la memoria caché hospedada por punto de conexión de servicio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar este procedimiento para instalar la característica BranchCache en el servidor de la memoria caché hospedada, HCS1 y para configurar el servidor para registrar un punto de conexión de servicio \(SCP\) en los servicios de dominio de Active Directory \(AD DS\).

Cuando te registras servidores de la memoria caché hospedada con un SCP en AD DS, el SCP permite a cliente equipos que están configurados correctamente para detectar automáticamente los servidores de memoria caché hospedada por consultas a AD DS para el SCP. Más adelante en esta guía se proporcionan instrucciones sobre cómo configurar los equipos cliente para realizar esta acción.

>[!IMPORTANT]
>Antes de realizar este procedimiento, debes unir el equipo al dominio y configurar el equipo con una dirección IP estática.

Para realizar este procedimiento, debe ser miembro del grupo de administradores.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Para instalar la característica BranchCache y configurar el servidor de la memoria caché hospedada  

1. En el equipo servidor, ejecuta Windows PowerShell como administrador. Escribe el siguiente comando y, a continuación, presione ENTRAR.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Para configurar el equipo como un servidor de la memoria caché hospedada después de instala la característica BranchCache y registrar un punto de conexión de servicio en AD DS, escribe el siguiente comando en Windows PowerShell y, a continuación, presione ENTRAR.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Para comprobar la configuración del servidor de memoria caché hospedada, escribe el siguiente comando y presione ENTRAR.

    ```  
    Get-BCStatus  
    ```  
  
    Los resultados del comando mostrarán estado de todos los aspectos de la instalación BranchCache. Siguientes son algunas de las opciones de configuración BranchCache y el valor correcto para cada elemento:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Para preparar el paso de copiar los paquetes de datos de los servidores de contenido a los servidores de la memoria caché hospedada, identificar un recurso existente en el servidor de la memoria caché hospedada o crea una nueva carpeta y comparte la carpeta para que sea accesible desde los servidores de contenido. Después de crear los paquetes de datos en los servidores de contenido, se copiarán los paquetes de datos a esta carpeta compartida en el servidor de la memoria caché hospedada.
  
5. Si vas a implementar más de un servidor de la memoria caché hospedada, repite este procedimiento en cada servidor.

Para continuar con esta guía, consulte [mover y cambiar el tamaño de la memoria caché hospedada & #40; opcional & #41;](6-Bc-Move-Resize-Cache.md).
