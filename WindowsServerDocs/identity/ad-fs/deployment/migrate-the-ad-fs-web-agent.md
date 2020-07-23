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
ms.openlocfilehash: 3c18e0a6a2e633c0fad0ce8585296afba8ce444c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966937"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar el agente Web de AD FS

Para migrar el agente basado en tokens de Windows de AD FS 1,1 o el agente para notificaciones de AD FS 1,1 que se instala con Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012, realice una actualización en contexto del sistema operativo del equipo que hospeda cualquier agente en Windows Server 2012. Para obtener más información, consulte el tema sobre la [instalación de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)). No es necesario realizar ninguna otra configuración.  
  
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
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor proxy de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de Federación AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar el servidor proxy de Federación AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)
