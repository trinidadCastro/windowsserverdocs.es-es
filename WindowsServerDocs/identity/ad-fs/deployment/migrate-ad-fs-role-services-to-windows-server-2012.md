---
title: "Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012"
description: Proporciona instrucciones para migrar el servicio de AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012

El siguiente proporciona instrucciones sobre cómo migrar los siguientes servicios de rol a servicios de federación de Active Directory (AD FS) en Windows Server 2012:  
  
-   Agente de AD FS 1.1 Windows basada en tokens y AD FS 1.1 agente notificaciones instalado con Windows Server 2008 o Windows Server 2008 R2  
  
-   Servidor de federación de AD FS 2.0 y proxy del servidor de federación 2.0 de AD FS instalada en Windows Server 2008 o Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Escenarios de migración admitidos  
 Las instrucciones de migración contiene las siguientes tareas:  
  
-   Exportar los datos de configuración 2.0 de AD FS desde el servidor que ejecuta Windows Server 2008 o Windows Server 2008 R2  
  
-   Realizar una actualización en contexto del sistema operativo de este servidor de Windows Server 2008 o Windows Server 2008 R2 para Windows Server 2012.
  
-   Volver a crear la configuración de AD FS original y restaurar el resto AD FS servicio configuración en este servidor, que se está ejecutando el rol de servidor de AD FS que se instala con Windows Server 2012.  
  
 Esta guía no incluye instrucciones para migrar un servidor que ejecuta varios roles. Si tu servidor tiene varios roles, te recomendamos que diseñes un proceso de migración personalizados específico al entorno de servidor, en función de la información proporcionada en otras guías de migración de rol. Guías de migración de roles adicionales están disponibles en la [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemas operativos compatibles  
 **Sistema operativo de servidor de destino:**  
  

-  Windows Server 2012 o Windows Server 2008 R2 (Server Core y opciones de instalación completo)  
  
 **Procesador de servidor de destino:**  
  

-  x64 64  
  
|Procesador de servidor de origen|Sistema operativo de servidor de origen|  
|-----|-----|  
|x86-o x 64-según|Windows Server 2003 con Service Pack 2|  
|x86-o x 64-según|Windows Server 2003 R2|  
|x86-o x 64-según|Windows Server 2008, completa y opciones de instalación Server Core|  
|x64 64|Windows Server 2008 R2|  
|x64 64|Opción de instalación Server Core de Windows Server 2008 R2|  
|x64 64|Opciones de instalación completa de Windows Server 2012 y Server Core|  
  
> [!NOTE]
>  -   Las versiones de sistemas operativos que se enumeran en la tabla anterior son las más antiguas combinaciones de sistemas operativos y service Pack que es compatibles.  
> -   Las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server se admiten como el origen o el servidor de destino.  
> -   Se admiten migraciones entre sistemas operativos físicos y sistemas operativos virtuales.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Las características y los servicios de rol de AD FS compatibles  
 La siguiente tabla describe los escenarios de migración de los servicios de rol de AD FS y sus respectivas opciones de configuración que se describen en esta guía.  
  
|De|Para AD FS instalada con Windows Server 2012|  
|----------|-----|  
|Servidor de federación de AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|  
|Proxy de servidor de federación AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|  
|AD FS 1.0 Windows basada en tokens agente instalado con Windows Server 2003 R2|No se admite la migración|  
|AD FS 1.0 notificaciones agente instalado con Windows Server 2003 R2)|No se admite la migración|  
|Servidor de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración|  
|Proxy de servidor de federación AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración|  
|AD FS 1.1 Windows basada en tokens agente instalado con Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor, pero el agente basada en tokens de AD FS Windows migrado funcionará solo con un servicio de federación 1.1 AD FS instalado con Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulta:<br /><br /> [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperar con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 notificaciones agente instalado con Windows Server 2008 o Windows Server 2008 R2)|Se admite la migración en el mismo servidor. El agente de web notificaciones 1.1 AD FS migrado funcionarán con lo siguiente:<br /><br /> Servicios de federación de AD FS 1.1 instalan con Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servicios de federación 2.0 de AD FS instalada en Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servicios de federación de AD FS instalan con Windows Server 2012<br /><br /> Para obtener más información, consulta:<br /><br /> [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperar con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para obtener más información, consulta:<br /><br /> [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)|  
|Proxy de servidor de federación AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor.  Para obtener más información, consulta:<br /><br /> [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Consulta también  
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)