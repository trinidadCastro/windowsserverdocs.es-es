---
title: "Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012 R2"
description: Proporciona instrucciones para migrar el servicio de AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012 R2
 Este documento proporciona instrucciones para migrar los siguientes servicios de rol en Active Directory federación Services (AD FS) que se instala con Windows Server 2012 R2:  
  
-   Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2  
  
-   Servidor de federación de AD FS instalado en Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Escenarios de migración admitidos  
 Las instrucciones de migración en esta guía consisten en las siguientes tareas:  
  
-   Exportar los datos de configuración 2.0 de AD FS desde el servidor que ejecuta Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012  
  
-   Realizar una actualización en contexto del sistema operativo de este servidor de Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 para Windows Server 2012 R2. 
  
-   Volver a crear la configuración de AD FS original y restaurar el resto AD FS servicio configuración en este servidor, que se está ejecutando el rol de servidor de AD FS que se instala con Windows Server 2012 R2.  
  
 Esta guía no incluye instrucciones para migrar un servidor que ejecuta varios roles. Si tu servidor tiene varios roles, te recomendamos que diseñes un proceso de migración personalizados específico al entorno de servidor, en función de la información proporcionada en otras guías de migración de rol. Guías de migración de roles adicionales están disponibles en la [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemas operativos compatibles  
 Sistema operativo de servidor de destino:  
  
 Windows Server 2012 R2 (Server Core y opciones de instalación completo)  
  
 Procesador de servidor de destino:  
  
 x64 64  
  
|Procesador de servidor de origen|Sistema operativo de servidor de origen|  
|-----------------------------|------------------------------------|  
|x86-o x 64-según| Windows Server 2008, completa y opciones de instalación Server Core|  
|x64 64|Windows Server 2008 R2|  
|x64 64|Opción de instalación Server Core de Windows Server 2008 R2|  
|x64 64|Opciones de instalación completa de Windows Server 2012 y Server Core|  
  
> [!NOTE]
>  -   Las versiones de sistemas operativos que se enumeran en la tabla anterior son las más antiguas combinaciones de sistemas operativos y service Pack que es compatibles.  
> -   Las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server se admiten como el origen o el servidor de destino.  
> -   Se admiten migraciones entre sistemas operativos físicos y sistemas operativos virtuales.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Las características y los servicios de rol de AD FS compatibles  
 La siguiente tabla describe los escenarios de migración de los servicios de rol de AD FS y sus respectivas opciones de configuración que se describen en esta guía.  
  
|De|Para AD FS instalada con Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para obtener más información, consulta:<br /><br /> [Preparación para migrar el servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Servidor de federación de AD FS instalado en Windows Server 2012|Se admite la migración en el mismo servidor.  Para obtener más información, consulta:<br /><br /> [Preparación para migrar el servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrar al Proxy de servidor de federación de AD FS](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)