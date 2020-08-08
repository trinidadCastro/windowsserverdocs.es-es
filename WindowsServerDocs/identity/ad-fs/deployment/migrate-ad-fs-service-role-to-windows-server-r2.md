---
title: Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2
description: Proporciona instrucciones para migrar el servicio AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.openlocfilehash: 49c75fadcbc1284f23b490eb6ee48813ef40f781
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970912"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2
 En este documento se proporcionan instrucciones para migrar los siguientes servicios de rol a Servicios de federación de Active Directory (AD FS) (AD FS) que se instala con Windows Server 2012 R2:

-   Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2

-   Servidor de federación de AD FS instalado en Windows Server 2012

## <a name="supported-migration-scenarios"></a>Escenarios de migración admitidos
 Las instrucciones de migración de esta guía constan de las siguientes tareas:

- Exportar los datos de configuración de AD FS 2.0 desde el servidor que ejecuta Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012

- Realización de una actualización local del sistema operativo de este servidor de Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012 R2.

- Volver a crear la configuración de AD FS original y restaurar la configuración del servicio AD FS restante en este servidor, que ahora ejecuta el rol de servidor AD FS que se instala con Windows Server 2012 R2.

  Esta guía no incluye instrucciones para migrar un servidor que ejecute varios roles. Si el servidor ejecuta varios roles, se recomienda que diseñe un proceso de migración personalizado específico para el entorno del servidor, de acuerdo con la información que se proporciona en otras guías de migración de roles. Puedes encontrar guías sobre migración para roles adicionales en el [Portal de migración de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).

### <a name="supported-operating-systems"></a>Sistemas operativos admitidos
 Sistema operativo del servidor de destino:

 Windows Server 2012 R2 (opciones de instalación completa y Server Core)

 Procesador del servidor de destino:

 Basado en x64

|Procesador del servidor de origen|Sistema operativo del servidor de origen|
|-----------------------------|------------------------------------|
|Basado en x86 o en x64| Windows Server 2008, opciones de instalación completa y Server Core|
|Basado en x64|Windows Server 2008 R2|
|Basado en x64|Opción de instalación Server Core de Windows Server 2008 R2|
|Basado en x64|Opciones de instalación completa y Server Core de Windows Server 2012|

> [!NOTE]
> - Las versiones de los sistemas operativos que se muestran en la tabla anterior corresponden a las combinaciones de sistemas operativos y Service Packs más antiguos que se admiten.
>   -   Las ediciones Foundation, Standard, Enterprise y Datacenter del sistema operativo Windows Server se admiten como servidor de origen o de destino.
>   -   Se admiten las migraciones entre sistemas operativos físicos y virtuales.

### <a name="supported-ad-fs-role-services-and-features"></a>Servicios de rol y características de AD FS compatibles
 En la tabla siguiente se describen los escenarios de migración de los servicios de rol de AD FS y su configuración respectiva descritos en esta guía.

|De|Para AD FS instala con Windows Server 2012 R2|
|----------|----------------------------------------------------------------------------------------------|
|Servidor de federación de AD FS 2.0 instalado en Windows Server 2008 o Windows Server 2008 R2|Se admite la migración en el mismo servidor. Para obtener más información, consulte:<p> [Preparación de la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<p> [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)|
|Servidor de federación de AD FS instalado en Windows Server 2012|Se admite la migración en el mismo servidor.  Para obtener más información, consulte:<p> [Preparación de la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)<p> [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)|

## <a name="next-steps"></a>Pasos a seguir
 [Preparación de la migración del servidor de Federación de AD FS](prepare-migrate-ad-fs-server-r2.md) [migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md) [migración del servidor proxy de Federación de AD FS](migrate-fed-server-proxy-r2.md) [comprobación de la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)