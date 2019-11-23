---
title: Migrar el agente Web de AD FS
description: Proporciona información sobre el agente Web de AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ddba9668b54e325dae6dfc0cf67d50d3ae5d90be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408197"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar el agente Web de AD FS

Para migrar el agente basado en tokens de Windows de AD FS 1,1 o el agente para notificaciones de AD FS 1,1 que se instala con Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012, realice una actualización local del sistema operativo del equipo que hospeda cualquier agente. a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). No es necesario realizar ninguna otra configuración.  
  
> [!IMPORTANT]
>  El agente basado en tokens migrado de AD FS 1.1 de Windows solo funciona con un Servicio de federación de AD FS 1.1, que se instala con Windows Server 2008 R2 o Windows Server 2008. Para obtener más información, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
> 
>  El agente web para notificaciones migrado de AD FS 1.1 funcionará con lo siguiente:  
> 
> - El Servicio de federación de AD FS 1.1 instalado con Windows Server 2008 R2 o Windows Server 2008  
>   -   El Servicio de federación de AD FS 2.0 instalado en Windows Server 2008 R2 o Windows Server 2008  
>   -   AD FS servicio de Federación instalado con Windows Server 2012  
> 
>   Para obtener más información, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de federación AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor proxy de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de federación AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migre el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)