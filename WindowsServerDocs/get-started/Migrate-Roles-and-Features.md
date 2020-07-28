---
title: Migración de roles y características
description: Información sobre cómo migrar roles y características a una versión más reciente de Windows Server.
ms.prod: windows-server
ms.date: 08/28/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 99a684cc90d47e1e80dc84ef9c3705a2ed79728b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182031"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrar roles y características en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta página contiene vínculos a información y herramientas que te guiarán por el proceso de la migración de roles y características a una versión más reciente de Windows Server. Puedes migrar servidores de archivos y almacenamiento mediante el [Servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md), mientras que muchos otros roles y características se pueden migrar mediante las Herramientas de migración de Windows Server, un conjunto de cmdlets de PowerShell que se incluyeron en Windows Servidor 2008 R2 para migrar roles y características.

La guías de migración ayudan a las migraciones de roles y características especificados desde un servidor a otro (no en actualizaciones en contexto). Si no se indica algo distinto en las guías, se admiten las migraciones entre equipos físicos y virtuales y entre las opciones de instalación completa de Windows Server y servidores que ejecuten la opción de instalación básica.

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar a migrar roles y características, comprueba que los servidores de origen y destino ejecuten los Service Packs más recientes disponibles para sus sistemas operativos.

> [!NOTE]
> Siempre que se migra o se actualiza a cualquier versión de Windows Server, es necesario revisar y comprender la [directiva de ciclos de vida de Microsoft](https://support.microsoft.com/lifecycle) y el período para esa versión, y planificar en consecuencia. Puedes [buscar información sobre el ciclo de vida](https://support.microsoft.com/lifecycle) referente a la versión concreta de Windows Server que te interese.

## <a name="windows-server-2019"></a>Windows Server 2019

Para migrar servidores de archivos y almacenamiento a Windows Server 2019 o Windows Server 2016, se recomienda usar el [Servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md). Para migrar otros roles, consulta las instrucciones para Windows Server 2016 y Windows Server 2012 R2.

## <a name="windows-server-2016"></a>Windows Server 2016

Estas son las guías de migración de Windows Server 2016. Ten en cuenta que, en muchos casos, también puedes usar las guías de migración de Windows Server 2012 R2.

- [Servicios de Escritorio remoto](../remote/remote-desktop-services/migrate-rds-role-services.md)
- [Servidor web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [MultiPoint Services](../remote/multipoint-services/multipoint-services-migrate.md)

Para migrar servidores de archivos a Windows Server 2019 o Windows Server 2016, se recomienda usar el [Servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md).

## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

Sigue los pasos que aparecen en estas guías para migrar roles y características desde servidores que ejecuten Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o Windows Server 2012 R2 a Windows Server 2012 R2. Herramientas de migración de Windows Server en Windows Server 2012 R2 admite las migraciones entre subredes.

- [Instalar, usar y quitar herramientas de migración de Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134202(v=ws.11))
- [Guía de migración de Servicios de certificados de Active Directory para Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486797(v=ws.11))
- [Migración del servicio de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486815(v=ws.11))
- [Guía de migración y actualización de Active Directory Rights Management Services](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754277(v=ws.10))
- [Migrar Servicios de archivos y almacenamiento a Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn479292(v=ws.11))
- [Migrar Hyper-V a Windows Server 2012 R2 desde Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486799(v=ws.11))
- [Migrar Servidor de directivas de redes a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831652(v=ws.11))
- [Migrar Servicios de Escritorio remoto a Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn479239(v=ws.11))
- [Migración de Windows Server Update Services a Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [Migrar roles de clúster a Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn530779(v=ws.11))
- [Migrar servidores DHCP a Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn495425(v=ws.11))

Ahora se encuentra disponible un libro electrónico con las guías de migración de Windows Server 2012 y Windows Server 2012 R2. Para más información y descargar el libro electrónico, consulta la [E-Book Gallery for Microsoft Technologies](https://download.microsoft.com/download/8/D/3/8D33661A-7E21-4FEE-9AAA-C17C3004B5AA/Windows-Migration-and-Upgrade-Guide.pdf) (Galería de libros electrónicos de Microsoft Technologies).

## <a name="windows-server-2012"></a>Windows Server 2012

Sigue los pasos que aparecen en estas guías para migrar roles y características desde servidores que ejecuten Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012. Herramientas de migración de Windows Server en Windows Server 2012 admite las migraciones entre subredes.

- [Instalar, usar y quitar herramientas de migración de Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134202(v=ws.11))
- [Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012](../identity/ad-fs/deployment/migrate-ad-fs-role-services-to-windows-server-2012.md)
- [Migrar Autoridad de registro de mantenimiento a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831513(v=ws.11))
- [Migrar Hyper-V a Windows Server 2012 R2 desde Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574113(v=ws.11))
- [Migración de la configuración de IP a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574133(v=ws.11))
- [Migrar Servidor de directivas de redes a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831652(v=ws.11))
- [Migrar Servicios de impresión y documentos a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134150(v=ws.11))
- [Migrate Remote Access to Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831423(v=ws.11))
- [Migración de Windows Server Update Services a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [Actualizar los controladores de Dominio de Active Directory a Windows Server 2012](../identity/ad-ds/deploy/upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012.md)
- [Migración de aplicaciones y servicios en clúster a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486790(v=ws.11))


Para más recursos de migración, visita [Migrar roles y características a Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134039(v=ws.11)).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

Sigue los pasos que aparecen en estas guías para migrar roles y características desde servidores que ejecuten Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008 R2. Herramientas de migración de Windows Server en Windows Server 2008 R2 no admite las migraciones entre subredes.

- [Instalación, acceso y eliminación de las herramientas de migración de Windows Server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379545(v=ws.10))
- [Guía de migración de los Servicios de certificados de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))
- [Guía de migración del servidor de Active Directory Domain Services y Sistema de nombres de dominio (DNS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379558(v=ws.10)) (puede estar en inglés)
- [Guía de migración de BranchCache](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548365(v=ws.10)).
- [Guía de migración del servidor DHCP (Protocolo de configuración dinámica de host)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379535(v=ws.10)) (puede estar en inglés)
- [Guía de migración de Servicios de archivo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379487(v=ws.10)) (puede estar en inglés)
- [Guía de migración de HRA](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee791829(v=ws.10))
- [Guía de migración de Hyper-V](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee849855(v=ws.10))
- [Guía de migración de configuración de IP](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379537(v=ws.10))
- [Guía de migración de usuarios y grupos locales](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379531(v=ws.10)) (puede estar en inglés)
- [Guía de migración de NPS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee791849(v=ws.10))
- [Guía de migración de Servicios de impresión](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379488(v=ws.10))
- [Guía de migración de Servicios de Escritorio remoto](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff849223(v=ws.10))
- [Guía de migración de RRAS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee822825(v=ws.10))
- [Tareas comunes e información sobre la migración de Windows Server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff400258(v=ws.10))
- [Guía de migración de Windows Server Update Services 3.0 SP2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee822826(v=ws.10)) (puede estar en inglés)

Para más recursos de migración, visita [Migrate Roles and Features to Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd365353(v=ws.10)) (Migrar roles y características a Windows Server 2008 R2).
