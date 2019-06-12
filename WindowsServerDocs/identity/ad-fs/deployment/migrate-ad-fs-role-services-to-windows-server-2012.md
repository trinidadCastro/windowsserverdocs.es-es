---
title: Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012
description: Proporciona instrucciones para migrar el servicio AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 63dc9825b9c9b8a06d0946859e08a536589eb044
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445701"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012

El siguiente proporciona instrucciones sobre cómo migrar los siguientes servicios de rol Servicios de federación de Active Directory (AD FS) en Windows Server 2012:  
  
-   Agente basado en tokens de AD FS 1.1 Windows y AD FS 1.1 de agente para notificaciones instalado con Windows Server 2008 o Windows Server 2008 R2  
  
-   Servidor de federación de AD FS 2.0 y proxy de AD FS 2.0 federation server instalado en Windows Server 2008 o Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Escenarios de migración compatibles  
 Las instrucciones de migración contiene las siguientes tareas:  
  
- Exportar los datos de AD FS 2.0 configuración desde el servidor que ejecuta Windows Server 2008 o Windows Server 2008 R2  
  
- Realizar una actualización en contexto del sistema operativo de este servidor de Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2012.
  
- Volver a crear la configuración de AD FS original y restaurar el resto de AD FS del servicio configuración en este servidor, que ahora se está ejecutando el rol de servidor de AD FS que se instala con Windows Server 2012.  
  
  Esta guía no incluye instrucciones para migrar un servidor que ejecute varios roles. Si el servidor ejecuta varios roles, se recomienda que diseñe un proceso de migración personalizado específico para el entorno del servidor, de acuerdo con la información que se proporciona en otras guías de migración de roles. Podrá encontrar guías sobre migración para roles adicionales en el [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemas operativos compatibles  
 **Sistema de operativo del servidor de destino:**  
  

- Windows Server 2012 o Windows Server 2008 R2 (opciones de instalación completa y Server Core)  
  
  **Procesador del servidor de destino:**  
  

- Basado en x64  
  
|Procesador del servidor de origen|Sistema operativo del servidor de origen|  
|-----|-----|  
|Basado en x86 o en x64|Windows Server 2003 con Service Pack 2|  
|Basado en x86 o en x64|Windows Server 2003 R2|  
|Basado en x86 o en x64|Windows Server 2008, completo y las opciones de instalación de Server Core|  
|Basado en x64|Windows Server 2008 R2|  
|Basado en x64|Opción de instalación de Server Core de Windows Server 2008 R2|  
|Basado en x64|Opciones de instalación completa de Windows Server 2012 y Server Core|  
  
> [!NOTE]
> - Las versiones de los sistemas operativos que se muestran en la tabla anterior corresponden a las combinaciones de sistemas operativos y Service Packs más antiguos que se admiten.  
>   -   Se admiten las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server como el origen o el servidor de destino.  
>   -   Se admiten las migraciones entre sistemas operativos físicos y virtuales.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Los servicios de rol de AD FS y las características compatibles  
 En la tabla siguiente se describe los escenarios de migración de los servicios de rol de AD FS y su configuración respectiva descritos en esta guía.  
  
|De|Para AD FS instalado con Windows Server 2012|  
|----------|-----|  
|Servidor de federación de AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|  
|Proxy de servidor de federación AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|  
|AD FS 1.0 Windows basada en token agente instalado con Windows Server 2003 R2|No se admite la migración|  
|AD FS 1.0 notificaciones agente instalado con Windows Server 2003 R2)|No se admite la migración|  
|Servidor de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración|  
|Proxy de servidor de federación AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración|  
|AD FS 1.1 Windows basada en token agente instalado con Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor, pero el agente basado en tokens de Windows de AD FS migrado solo funcionará con un servicio de federación 1.1 de AD FS instalado con Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, vea:<br /><br /> [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperación con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 notificaciones agente instalado con Windows Server 2008 o Windows Server 2008 R2)|Se admite la migración en el mismo servidor. El agente web 1.1 de notificaciones de AD FS migrado funcionará con lo siguiente:<br /><br /> Servicio de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servicio de federación 2.0 de AD FS instalado en Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servicio de federación de AD FS instalado con Windows Server 2012<br /><br /> Para obtener más información, vea:<br /><br /> [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperación con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para obtener más información, vea:<br /><br /> [Preparar la migración del servidor de federación de AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrar el servidor de federación de AD FS 2.0](migrate-the-ad-fs-fed-server.md)|  
|Proxy de servidor de federación AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor.  Para obtener más información, consulta:<br /><br /> [Preparar la migración del servidor proxy de federación de AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrar el servidor proxy de federación de AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Vea también  
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)