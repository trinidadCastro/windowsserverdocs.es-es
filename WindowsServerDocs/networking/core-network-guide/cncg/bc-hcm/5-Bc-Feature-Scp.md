---
title: Instalar la característica BranchCache y configurar el servidor de caché hospedada mediante el punto de conexión de servicio
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6619b09df0d4c161148d22091337a5039c7ea3af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849656"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Instalar la característica BranchCache y configurar el servidor de caché hospedada mediante el punto de conexión de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para instalar la característica BranchCache en el servidor de caché hospedada, HCS1 y para configurar el servidor para registrar un punto de conexión de servicio \(SCP\) en Active Directory Domain Services \(AD DS\).

Al registrar los servidores de caché hospedada con un SCP en AD DS, el SCP permite a cliente los equipos que están configurados correctamente para detectar automáticamente los servidores de caché hospedada por la consulta a AD DS para el SCP. Más adelante en esta guía se proporcionan instrucciones sobre cómo configurar los equipos cliente para realizar esta acción.

>[!IMPORTANT]
>Antes de realizar este procedimiento, debe unir el equipo al dominio y configurar el equipo con una dirección IP estática.

Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Para instalar la característica BranchCache y configurar el servidor de caché hospedada  

1. En el equipo servidor, ejecute Windows PowerShell como administrador. Escriba el siguiente comando y presione ENTRAR.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Para configurar el equipo como servidor de caché hospedada después de instala la característica BranchCache y registrar un punto de conexión de servicio en AD DS, escriba el siguiente comando en Windows PowerShell y, a continuación, presione ENTRAR.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Para comprobar la configuración del servidor de caché hospedada, escriba el siguiente comando y presione ENTRAR.

    ```  
    Get-BCStatus  
    ```  
  
    Los resultados del comando mostrarán estado de todos los aspectos de la instalación de BranchCache. Siguientes son algunas de la configuración de BranchCache y el valor correcto para cada elemento:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Para prepararse para el paso de copia de los paquetes de datos de los servidores de contenido a los servidores de caché hospedada, identificar un recurso compartido existente en el servidor de caché hospedada o cree una nueva carpeta y comparta la carpeta para que sea accesible desde los servidores de contenido. Después de crear los paquetes de datos en los servidores de contenido, copiará los paquetes de datos a esta carpeta compartida en el servidor de caché hospedada.
  
5. Si va a implementar más de un servidor de caché hospedada, repita este procedimiento en cada servidor.

Para continuar con esta guía, consulte [mover y cambiar el tamaño de la caché hospedada &#40;opcional&#41;](6-Bc-Move-Resize-Cache.md).
