---
title: Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012
description: Proporciona instrucciones para migrar el servicio AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: abbd51daea374eb4684f5416bed45fb0aaf315d4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938215"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012

A continuación se proporcionan instrucciones para migrar los siguientes servicios de rol a Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012:

-   Agente basado en tokens de Windows de AD FS 1.1 y agente para notificaciones de AD FS 1.1 con Windows Server 2008 o Windows Server 2008 R2

-   Un servidor de federación de AD FS 2.0 y un proxy de servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2

## <a name="supported-migration-scenarios"></a>Escenarios de migración admitidos
 Las instrucciones de migración contienen las siguientes tareas:

- Exportar los datos de configuración de AD FS 2.0 desde el servidor que ejecuta Windows Server 2008 o Windows Server 2008 R2.

- Realización de una actualización local del sistema operativo de este servidor de Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2012.

- Volver a crear la configuración de AD FS original y restaurar la configuración del servicio AD FS restante en este servidor, que ahora ejecuta el rol de servidor AD FS que se instala con Windows Server 2012.

  Esta guía no incluye instrucciones para migrar un servidor que ejecute varios roles. Si el servidor ejecuta varios roles, se recomienda que diseñe un proceso de migración personalizado específico para el entorno del servidor, de acuerdo con la información que se proporciona en otras guías de migración de roles. Puedes encontrar guías sobre migración para roles adicionales en el [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos
 **Sistema operativo del servidor de destino:**


- Windows Server 2012 o Windows Server 2008 R2 (opciones de instalación completa y Server Core)

  **Procesador del servidor de destino:**


- Basado en x64

|Procesador del servidor de origen|Sistema operativo del servidor de origen|
|-----|-----|
|Basado en x86 o en x64|Windows Server 2003 con Service Pack 2|
|Basado en x86 o en x64|Windows Server 2003 R2|
|Basado en x86 o en x64|Windows Server 2008, opciones de instalación completa y Server Core|
|Basado en x64|Windows Server 2008 R2|
|Basado en x64|Opción de instalación Server Core de Windows Server 2008 R2|
|Basado en x64|Opciones de instalación completa y Server Core de Windows Server 2012|

> [!NOTE]
> - Las versiones de los sistemas operativos que se muestran en la tabla anterior corresponden a las combinaciones de sistemas operativos y Service Packs más antiguos que se admiten.
>   -   Las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server se admiten como servidor de origen o de destino.
>   -   Se admiten las migraciones entre sistemas operativos físicos y virtuales.

### <a name="supported-ad-fs-role-services-and-features"></a>Servicios de rol y características de AD FS compatibles
 En la tabla siguiente se describen los escenarios de migración de los servicios de rol de AD FS y su configuración respectiva descritos en esta guía.

|De|Para AD FS instala con Windows Server 2012|
|----------|-----|
|Servidor de federación de AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|
|Servidor proxy de federación de AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|
|Agente basado en tokens de Windows de AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|
|Agente para notificaciones de AD FS 1.0 instalado con Windows Server 2003 R2|No se admite la migración|
|Servidor de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración|
|Servidor proxy de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración|
|Agente basado en tokens de Windows de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|No se admite la migración en el mismo servidor, pero el agente basado en tokens de Windows de AD FS migrado solo funcionará con un servicio de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulte:<p> [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)<p> [Interoperación con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|
|Agente para notificaciones de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. El agente web para notificaciones de AD FS 1.1 migrado funcionará con lo siguiente:<p> Servicio de federación de AD FS 1.1 instalado con Windows Server 2008 o Windows Server 2008 R2<p> Servicio de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2<p> AD FS servicio de Federación instalado con Windows Server 2012<p> Para obtener más información, consulte:<p> [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)<p> [Interoperación con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|
|Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para obtener más información, consulte:<p> [Preparar la migración del servidor de federación de AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<p> [Migrar el servidor de federación de AD FS 2.0](migrate-the-ad-fs-fed-server.md)|
|Servidor proxy de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor.  Para obtener más información, consulte:<p> [Preparar la migración del servidor proxy de federación de AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<p> [Migrar el servidor proxy de federación de AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|

## <a name="see-also"></a>Consulte también
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparación para migrar el servidor proxy de federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar el servidor de Federación de AD FS 2,0](migrate-the-ad-fs-fed-server.md) [migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) [migrar los agentes Web de AD FS 1,1](migrate-the-ad-fs-web-agent.md)