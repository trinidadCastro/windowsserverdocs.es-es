---
title: Implementar servidores de la memoria caché hospedada (opcional)
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d7345b9acf5ef5003cc2a811569083d7c12894a1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-hosted-cache-servers-optional"></a>Implementar servidores de la memoria caché hospedada (opcional)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para instalar y configurar los servidores de caché BranchCache hospedada que se encuentran en sucursales donde quieres implementar el modo de caché BranchCache hospedado. Con BranchCache en Windows Server 2016, puedes implementar varios servidores de la memoria caché hospedada en una sucursal.  
  
> [!IMPORTANT]  
> Este paso es opcional porque el modo de caché distribuida no requiere un equipo de servidor de la memoria caché hospedada en sucursales. Si no vas a implementar hospedado el modo de caché en las sucursales, no es necesario implementar un servidor de la memoria caché hospedada y no es necesario realizar los pasos de este procedimiento.  
  
Debe ser miembro del **administradores**, o equivalente para realizar este procedimiento.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Para instalar y configurar un servidor de la memoria caché hospedada  
  
1.  En el equipo que quieres configurar como un servidor de la memoria caché hospedada, ejecuta el siguiente comando en un símbolo del sistema de Windows PowerShell para instalar la característica BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configurar el equipo como un servidor de la memoria caché hospedada mediante uno de los siguientes comandos:  
  
    -   Para configurar los equipos unidos a un dominio no como un servidor de la memoria caché hospedada, escribe el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
  
        `Enable-BCHostedServer`  
  
    -   Para configurar un dominio unido a un equipo como un servidor de la memoria caché hospedada y para registrar un punto de conexión de servicio en Active Directory para la detección automática de memoria caché hospedada del servidor, los equipos cliente, escribe el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Para comprobar la configuración correcta del servidor de la memoria caché hospedada, escribe el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Después de ejecutar este comando, en la sección **HostedCacheServerConfiguration**, el valor de **HostedCacheServerIsEnabled** es **True **. Si has configurado un servidor de caché hospedada estén unidos a un dominio para registrar un servicio conexión punto en Active Directory, el valor de **HostedCacheScpRegistrationEnabled** es **True **.  
  

