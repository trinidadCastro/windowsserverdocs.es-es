---
title: Implementación de servidores de caché hospedada (opcional)
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b19680e933e7a33871816578b63c5a141db0ce00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826216"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Implementación de servidores de caché hospedada (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar y configurar los servidores de caché hospedada de BranchCache que se encuentran en las sucursales donde desea implementar el modo de caché hospedada de BranchCache. Con BranchCache en Windows Server 2016, puede implementar varios servidores de caché hospedada en una sucursal.  
  
> [!IMPORTANT]  
> Este paso es opcional porque el modo caché distribuida no requiere un equipo de servidor de caché hospedada en las sucursales. Si no planea implementar el modo de caché en las sucursales hospedada, deberá implementar un servidor de caché hospedada y no es necesario realizar los pasos de este procedimiento.  
  
Debe ser miembro del **administradores**, o equivalente para realizar este procedimiento.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Para instalar y configurar un servidor de caché hospedada  
  
1.  En el equipo que desea configurar como servidor de caché hospedada, ejecute el siguiente comando en un símbolo del sistema de Windows PowerShell para instalar la característica BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configurar el equipo como servidor de caché hospedada mediante uno de los siguientes comandos:  
  
    -   Para configurar un equipo unido a que no sea de dominio como un servidor de caché hospedada, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
  
        `Enable-BCHostedServer`  
  
    -   Para configurar un equipo unido a un dominio como servidor de caché hospedada y registrar un punto de conexión de servicio en Active Directory para la detección automática de caché hospedada de servidor por los equipos cliente, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Para comprobar la configuración correcta del servidor de caché hospedada, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Después de ejecutar este comando, en la sección **HostedCacheServerConfiguration**, el valor de **HostedCacheServerIsEnabled** es **True**. Si ha configurado un servidor de caché hospedada unido a dominio para registrar un punto de conexión de servicio (SCP) en Active Directory, el valor de **HostedCacheScpRegistrationEnabled** es **True**.  
  

