---
title: Migrar al agente web de AD FS
description: Proporciona información sobre el agente web de AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877596"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar al agente web de AD FS

Migración de AD FS 1.1 agente basado en tokens de Windows o AD FS 1.1 agente para notificaciones que se instala con Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012, realizar una actualización en contexto del sistema operativo del equipo que hospeda cualquier agente a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). No es necesario realizar ninguna otra configuración.  
  
> [!IMPORTANT]
>  El agente basado en tokens migrado de AD FS 1.1 de Windows solo funciona con un Servicio de federación de AD FS 1.1, que se instala con Windows Server 2008 R2 o Windows Server 2008. Para obtener más información, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
>   
>  El agente web para notificaciones migrado de AD FS 1.1 funcionará con lo siguiente:  
>   
>  -   El Servicio de federación de AD FS 1.1 instalado con Windows Server 2008 R2 o Windows Server 2008  
> -   El Servicio de federación de AD FS 2.0 instalado en Windows Server 2008 R2 o Windows Server 2008  
> -   Servicio de federación de AD FS instalado con Windows Server 2012  
>   
>  Para obtener más información, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS 1.1 de Web](migrate-the-ad-fs-web-agent.md)