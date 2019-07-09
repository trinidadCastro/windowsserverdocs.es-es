---
title: Migración de roles y características
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 486c11ebd46c6fd23b3bd16cd90463f8d607287e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443540"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrar roles y características en Windows Server

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta página contiene vínculos a información y herramientas que te guiarán por el proceso de la migración de roles y características a Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016. Muchos roles y características se pueden migrar mediante el uso de Herramientas de migración de Windows Server, un conjunto de cinco cmdlets de Windows PowerShell que se introdujo en Windows Server 2008 R2 para migrar fácilmente elementos y datos de roles y características.

La guías de migración ayudan a las migraciones de roles y características especificados desde un servidor a otro (no en actualizaciones en contexto). Si no se indica algo distinto en las guías, se admiten las migraciones entre equipos físicos y virtuales y entre las opciones de instalación completa de Windows Server y servidores que ejecuten la opción de instalación básica.  

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar a migrar roles y características, comprueba que los servidores de origen y destino ejecuten los Service Packs más recientes disponibles para sus sistemas operativos.
Ahora se encuentra disponible un libro electrónico con las guías de migración de Windows Server 2012 y Windows Server 2012 R2. Para más información y descargar el libro electrónico, consulta la [E-Book Gallery for Microsoft Technologies](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles) (Galería de libros electrónicos de Microsoft Technologies). 

>[!NOTE]
>Siempre que se migra o se actualiza a cualquier versión de Windows Server, es necesario revisar y comprender la [directiva de ciclos de vida de Microsoft](https://support.microsoft.com/lifecycle) y el período para esa versión, y planificar en consecuencia. Puedes [buscar información sobre el ciclo de vida](https://support.microsoft.com/lifecycle) referente a la versión concreta de Windows Server que te interese.
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>Guías de migración
Se están desarrollando guías de migración actualizada para Windows Server 2016. Vuelve a consultar en esta ubicación para obtener las actualizaciones en cuanto estén disponibles. En muchos casos, los pasos de las guías de migración de Windows Server 2012 R2 son aún válidos para Windows Server 2016.

- [Servicios de Escritorio remoto](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Servidor web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>Guías de migración
Sigue los pasos que aparecen en estas guías para migrar roles y características desde servidores que ejecuten Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o Windows Server 2012 R2 a Windows Server 2012 R2. Herramientas de migración de Windows Server en Windows Server 2012 R2 admite las migraciones entre subredes.

- [Instalar, usar y quitar herramientas de migración de Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guía de migración de Servicios de certificados de Active Directory para Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migración del servicio de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guía de migración y actualización de Active Directory Rights Management Services](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrar Servicios de archivos y almacenamiento a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migrar Hyper-V a Windows Server 2012 R2 desde Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrar Servidor de directivas de redes a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar Servicios de Escritorio remoto a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migración de Windows Server Update Services a Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrar roles de clúster a Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrar servidores DHCP a Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>Guías de migración
Sigue los pasos que aparecen en estas guías para migrar roles y características desde servidores que ejecuten Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012. Herramientas de migración de Windows Server en Windows Server 2012 admite las migraciones entre subredes.

- [Instalar, usar y quitar herramientas de migración de Windows Server](https://technet.microsoft.com/library/jj134202)
- [Migrar los servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Migrar Autoridad de registro de mantenimiento a Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Migrar Hyper-V a Windows Server 2012 R2 desde Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migración de la configuración de IP a Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Migrar Servidor de directivas de redes a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar Servicios de impresión y documentos a Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Migrate Remote Access to Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Migración de Windows Server Update Services a Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Actualizar los controladores de Dominio de Active Directory a Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migración de aplicaciones y servicios en clúster a Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Para más recursos de migración, visita [Migrar roles y características a Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>Guías de migración
Sigue los pasos que aparecen en estas guías para migrar roles y características desde servidores que ejecuten Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008 R2. Herramientas de migración de Windows Server en Windows Server 2008 R2 no admite las migraciones entre subredes.

- [Instalación, acceso y eliminación de las herramientas de migración de Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guía de migración de los Servicios de certificados de Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guía de migración del servidor de Active Directory Domain Services y Sistema de nombres de dominio (DNS)](https://technet.microsoft.com/library/dd379558) (puede estar en inglés)
- [Guía de migración de BranchCache](https://technet.microsoft.com/library/dd548365).
- [Guía de migración del servidor DHCP (Protocolo de configuración dinámica de host)](https://technet.microsoft.com/library/dd379535) (puede estar en inglés)
- [Guía de migración de Servicios de archivo](https://technet.microsoft.com/library/dd379487) (puede estar en inglés)
- [Guía de migración de HRA](https://technet.microsoft.com/library/ee791829)
- [Guía de migración de Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guía de migración de configuración de IP](https://technet.microsoft.com/library/dd379537)
- [Guía de migración de usuarios y grupos locales](https://technet.microsoft.com/library/dd379531) (puede estar en inglés)
- [Guía de migración de NPS](https://technet.microsoft.com/library/ee791849)
- [Guía de migración de Servicios de impresión](https://technet.microsoft.com/library/dd379488)
- [Guía de migración de Servicios de Escritorio remoto](https://technet.microsoft.com/library/ff849223)
- [Guía de migración de RRAS](https://technet.microsoft.com/library/ee822825)
- [Tareas comunes e información sobre la migración de Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guía de migración de Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826) (puede estar en inglés)
 
Para más recursos de migración, visita [Migrate Roles and Features to Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353) (Migrar roles y características a Windows Server 2008 R2).
