---
title: Instalar la característica BranchCache y configurar el servidor de caché hospedada mediante el punto de conexión de servicio
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 4a059cad3bc3faf03882030528d85764eb13959d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949501"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Instalar la característica BranchCache y configurar el servidor de caché hospedada mediante el punto de conexión de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para instalar la característica BranchCache en el servidor de caché hospedada, HCS1, y configurar el servidor para que registre un punto \( de conexión de servicio SCP \) en Active Directory Domain Services \( AD DS \) .

Al registrar servidores de caché hospedada con un SCP en AD DS, el SCP permite que los equipos cliente que están configurados correctamente detecten automáticamente los servidores de caché hospedada mediante la consulta de AD DS para el SCP. Más adelante en esta guía se proporcionan instrucciones sobre cómo configurar los equipos cliente para realizar esta acción.

>[!IMPORTANT]
>Antes de llevar a cabo este procedimiento, debe unir el equipo al dominio y configurar el equipo con una dirección IP estática.

Para llevar a cabo este procedimiento, debe ser miembro del grupo Administradores.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Para instalar la característica BranchCache y configurar el servidor de caché hospedada

1. En el equipo servidor, ejecute Windows PowerShell como administrador. Escriba el siguiente comando y presione ENTRAR.

    ```
    Install-WindowsFeature BranchCache
    ```

2.  Para configurar el equipo como un servidor de caché hospedada después de instalar la característica BranchCache y para registrar un punto de conexión de servicio en AD DS, escriba el siguiente comando en Windows PowerShell y, a continuación, presione Entrar.

    ```
    Enable-BCHostedServer -RegisterSCP
    ```

3. Para comprobar la configuración del servidor de caché hospedada, escriba el siguiente comando y presione Entrar.

    ```
    Get-BCStatus
    ```

    Los resultados del comando muestran el estado de todos los aspectos de la instalación de BranchCache. A continuación se muestran algunos de los valores de BranchCache y el valor correcto para cada elemento:

    -   BranchCacheIsEnabled: true

    -   HostedCacheServerIsEnabled: true

    -   HostedCacheScpRegistrationEnabled: true

4. Para preparar el paso de copia de los paquetes de datos de los servidores de contenido en los servidores de caché hospedada, identifique un recurso compartido existente en el servidor de caché hospedada o cree una nueva carpeta y comparta la carpeta para que sea accesible desde los servidores de contenido. Después de crear los paquetes de datos en los servidores de contenido, copiará los paquetes de datos a esta carpeta compartida en el servidor de caché hospedada.

5. Si va a implementar más de un servidor de caché hospedada, repita este procedimiento en cada servidor.

Para continuar con esta guía, vea [movimiento y cambio de tamaño de la caché hospedada &#40;&#41;opcional ](6-Bc-Move-Resize-Cache.md).
