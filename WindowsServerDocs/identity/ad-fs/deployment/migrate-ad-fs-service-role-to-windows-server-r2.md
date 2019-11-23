---
title: Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2
description: Proporciona instrucciones para migrar el servicio AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2a3cf6cd523f5cfd69785104fed7aa3938d79525
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359391"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2
 En este documento se proporcionan instrucciones para migrar los siguientes servicios de rol a Servicios de federación de Active Directory (AD FS) (AD FS) que se instala con Windows Server 2012 R2:  
  
-   AD FS servidor de Federación 2,0 instalado en Windows Server 2008 o Windows Server 2008 R2  
  
-   AD FS servidor de Federación instalado en Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Escenarios de migración compatibles  
 Las instrucciones de migración de esta guía constan de las siguientes tareas:  
  
- Exportar los datos de configuración de AD FS 2,0 desde el servidor que ejecuta Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012  
  
- Realización de una actualización local del sistema operativo de este servidor de Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012 R2. 
  
- Volver a crear la configuración de AD FS original y restaurar la configuración del servicio AD FS restante en este servidor, que ahora ejecuta el rol de servidor AD FS que se instala con Windows Server 2012 R2.  
  
  Esta guía no incluye instrucciones para migrar un servidor que ejecute varios roles. Si el servidor ejecuta varios roles, se recomienda que diseñe un proceso de migración personalizado específico para el entorno del servidor, de acuerdo con la información que se proporciona en otras guías de migración de roles. Podrá encontrar guías sobre migración para roles adicionales en el [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemas operativos compatibles  
 Sistema operativo del servidor de destino:  
  
 Windows Server 2012 R2 (opciones de instalación completa y Server Core)  
  
 Procesador del servidor de destino:  
  
 Basado en x64  
  
|Procesador del servidor de origen|Sistema operativo del servidor de origen|  
|-----------------------------|------------------------------------|  
|Basado en x86 o en x64| Windows Server 2008, opciones de instalación completa y Server Core|  
|Basado en x64|Windows Server 2008 R2|  
|Basado en x64|Opción de instalación Server Core de Windows Server 2008 R2|  
|Basado en x64|Opciones de instalación completa y Server Core de Windows Server 2012|  
  
> [!NOTE]
> - Las versiones de los sistemas operativos que se muestran en la tabla anterior corresponden a las combinaciones de sistemas operativos y Service Packs más antiguos que se admiten.  
>   -   Las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server se admiten como servidor de origen o de destino.  
>   -   Se admiten las migraciones entre sistemas operativos físicos y virtuales.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Características y servicios de rol de AD FS admitidos  
 En la tabla siguiente se describen los escenarios de migración de los servicios de rol de AD FS y su configuración respectiva que se describen en esta guía.  
  
|De|Para AD FS instala con Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|AD FS servidor de Federación 2,0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para más información, consulta lo siguiente:<br /><br /> [Preparación de la migración del servidor de Federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md)|  
|AD FS servidor de Federación instalado en Windows Server 2012|Se admite la migración en el mismo servidor.  Para obtener más información, consulta:<br /><br /> [Preparación de la migración del servidor de Federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparando la migración del servidor de Federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migración del servidor proxy de AD FS Federation](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)