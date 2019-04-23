---
title: Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2
description: Proporciona instrucciones para migrar el servicio AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847956"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2
 Este documento proporciona instrucciones para migrar los siguientes servicios de rol a Active Directory Federation Services (AD FS) que se instala con Windows Server 2012 R2:  
  
-   Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2  
  
-   Servidor de federación de AD FS instalado en Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Escenarios de migración compatibles  
 Las instrucciones de migración de esta guía constan de las siguientes tareas:  
  
-   Exportar los datos de AD FS 2.0 configuración desde el servidor que ejecuta Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012  
  
-   Realizar una actualización en contexto del sistema operativo de este servidor de Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012 R2. 
  
-   Volver a crear la configuración de AD FS original y restaurar el resto de AD FS del servicio configuración en este servidor, que ahora se está ejecutando el rol de servidor de AD FS que se instala con Windows Server 2012 R2.  
  
 Esta guía no incluye instrucciones para migrar un servidor que ejecute varios roles. Si el servidor ejecuta varios roles, se recomienda que diseñe un proceso de migración personalizado específico para el entorno del servidor, de acuerdo con la información que se proporciona en otras guías de migración de roles. Podrá encontrar guías sobre migración para roles adicionales en el [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemas operativos compatibles  
 Sistema de operativo del servidor de destino:  
  
 Windows Server 2012 R2 (opciones de instalación completa y Server Core)  
  
 Procesador del servidor de destino:  
  
 Basado en x64  
  
|Procesador del servidor de origen|Sistema operativo del servidor de origen|  
|-----------------------------|------------------------------------|  
|Basado en x86 o en x64| Windows Server 2008, completo y las opciones de instalación de Server Core|  
|Basado en x64|Windows Server 2008 R2|  
|Basado en x64|Opción de instalación de Server Core de Windows Server 2008 R2|  
|Basado en x64|Opciones de instalación completa de Windows Server 2012 y Server Core|  
  
> [!NOTE]
>  -   Las versiones de los sistemas operativos que se muestran en la tabla anterior corresponden a las combinaciones de sistemas operativos y Service Packs más antiguos que se admiten.  
> -   Se admiten las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server como el origen o el servidor de destino.  
> -   Se admiten las migraciones entre sistemas operativos físicos y virtuales.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Los servicios de rol de AD FS y las características compatibles  
 En la tabla siguiente se describe los escenarios de migración de los servicios de rol de AD FS y su configuración respectiva descritos en esta guía.  
  
|De|Para AD FS instalado con Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para obtener más información, vea:<br /><br /> [Preparar la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Servidor de federación de AD FS instalado en Windows Server 2012|Se admite la migración en el mismo servidor.  Para obtener más información, consulta:<br /><br /> [Preparar la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migración del servidor Proxy de federación de AD FS](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)