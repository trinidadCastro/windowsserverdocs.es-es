---
title: Implementación de servidores de caché hospedada (opcional)
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 75f82b660edeac538bda511d71526aacc2433b5b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964251"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Implementación de servidores de caché hospedada (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar y configurar los servidores de caché hospedada de BranchCache que se encuentran en las sucursales donde desea implementar el modo caché hospedada de BranchCache. Con BranchCache en Windows Server 2016, puede implementar varios servidores de caché hospedada en una sucursal.

> [!IMPORTANT]
> Este paso es opcional porque el modo caché distribuida no requiere un equipo servidor de caché hospedada en las sucursales. Si no planea implementar el modo caché hospedada en las sucursales, no es necesario implementar un servidor de caché hospedada y no es necesario realizar los pasos de este procedimiento.

Debe ser miembro de **los administradores**o equivalente para realizar este procedimiento.

### <a name="to-install-and-configure-a-hosted-cache-server"></a>Para instalar y configurar un servidor de caché hospedada

1.  En el equipo que deseas configurar como servidor de caché hospedada, ejecuta el siguiente comando en un símbolo del sistema de Windows PowerShell para instalar la característica BranchCache.

    `Install-WindowsFeature BranchCache -IncludeManagementTools`

2.  Configure el equipo como un servidor de caché hospedada mediante uno de los siguientes comandos:

    -   Para configurar un equipo no unido a un dominio como servidor de caché hospedada, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

        `Enable-BCHostedServer`

    -   Para configurar un equipo unido a un dominio como un servidor de caché hospedada, y para registrar un punto de conexión de servicio en Active Directory para la detección automática de servidores de caché hospedada por equipos cliente, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

        `Enable-BCHostedServer -RegisterSCP`

3.  Para comprobar la configuración correcta del servidor de caché hospedada, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

    `Get-BCStatus`

    > [!NOTE]
    > Después de ejecutar este comando, en la sección **HostedCacheServerConfiguration**, el valor de **HostedCacheServerIsEnabled** es **true**. Si configuró un servidor de caché hospedada unido a un dominio para registrar un punto de conexión de servicio (SCP) en Active Directory, el valor de **HostedCacheScpRegistrationEnabled** es **true**.


