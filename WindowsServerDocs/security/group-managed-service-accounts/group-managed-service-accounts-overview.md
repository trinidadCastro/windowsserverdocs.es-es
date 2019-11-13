---
title: Group Managed Service Accounts Overview
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4e7f46739dd8def6ffc34c6cc50210c0e6999c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403746"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se presenta la cuenta de servicio administrada de grupo mediante la descripción de las aplicaciones prácticas, los cambios en la implementación de Microsoft y los requisitos de hardware y software.


## <a name="BKMK_OVER"></a>Descripción de la característica
Una cuenta de servicio administrada independiente (sMSA) es una cuenta de dominio administrada que proporciona administración automática de contraseñas, administración de nombres de entidad de seguridad de servicio (SPN) simplificada y la capacidad de delegar la administración a otros administradores. Este tipo de cuenta de servicio administrada (MSA) se incorporó en Windows Server 2008 R2 y Windows 7.

La cuenta de servicio administrada de grupo (gMSA) proporciona la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad en varios servidores. Al conectarse a un servicio hospedado en una granja de servidores, como la solución de equilibrio de carga de red, los protocolos de autenticación que admiten la autenticación mutua requieren que todas las instancias de los servicios usen la misma entidad de seguridad. Cuando se usa un gMSA como entidad de servicio, el sistema operativo Windows administra la contraseña de la cuenta en lugar de confiar en el administrador para administrar la contraseña.

El servicio de distribución de claves de Microsoft \(kdssvc. dll\) proporciona el mecanismo para obtener de manera segura la clave más reciente o una clave específica con un identificador de clave para una cuenta de Active Directory. El servicio de distribución de claves comparte un secreto que se usa para crear claves para la cuenta. Estas claves se cambian de forma periódica. En el caso de una gMSA, el controlador de dominio calcula la contraseña en la clave proporcionada por los servicios de distribución de claves, además de otros atributos de gMSA.  Los hosts miembros pueden obtener los valores de contraseña actuales y anteriores poniéndose en contacto con un controlador de dominio.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
GMSA proporcionan una solución de identidad única para los servicios que se ejecutan en una granja de servidores o en sistemas que están detrás de Load Balancer de red. Al proporcionar una solución de gMSA, los servicios se pueden configurar para la nueva entidad de seguridad de gMSA y Windows controla la administración de contraseñas.

El uso de gMSA, los servicios o los administradores de servicios no necesitan administrar la sincronización de contraseñas entre las instancias de servicio. GMSA admite hosts que se mantienen sin conexión durante un período de tiempo prolongado y la administración de hosts miembros para todas las instancias de un servicio. Esto significa que es posible implementar una granja de servidores que admite una sola identidad con la que pueden autenticarse los equipos cliente actuales sin saber a qué instancia del servicio se conectan.

Los clústeres de conmutación por error no admiten las cuentas de servicio administradas de grupo (gMSA). Sin embargo, los servicios que se ejecutan sobre el Servicio de clúster pueden utilizar una gMSA o una cuenta de servicio administrada independiente (sMSA) si son servicios de Windows, grupos de aplicaciones o tareas programadas, o si admiten gSMA o sMSA de forma nativa.

## <a name="BKMK_SOFT"></a>Requisitos de software

Se necesita una arquitectura de\-bits 64 para ejecutar los comandos de Windows PowerShell que se usan para administrar GMSA.

Una cuenta de servicio administrada depende de los tipos de cifrado compatibles con Kerberos. Cuando un equipo cliente se autentica en un servidor con Kerberos, el controlador de dominio crea un vale de servicio Kerberos protegido con un cifrado que admiten tanto el controlador de dominio como el servidor. El controlador de dominio usa el atributo msDS\-SupportedEncryptionTypes de la cuenta para determinar el cifrado que el servidor admite y, si no hay ningún atributo, supone que el equipo cliente no admite tipos de cifrado más seguros. Si el host está configurado para no admitir RC4, la autenticación siempre producirá un error. Es por esto que el AES siempre se debe configurar expresamente para MSA.

> [!NOTE]
> Desde Windows Server 2008 R2, el DES está deshabilitado de forma predeterminada. Para obtener más información sobre los tipos de cifrado compatibles, consulte [Cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

GMSA no se aplican a los sistemas operativos Windows anteriores a Windows Server 2012.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor
No hay pasos de configuración necesarios para implementar MSA y gMSA con Administrador del servidor o el cmdlet install\-WindowsFeature.

## <a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente se proporcionan vínculos a recursos adicionales relacionados con las cuentas de servicio administradas y las cuentas de servicio administradas de grupo.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Novedades de las cuentas de servicio administradas](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentación de cuentas de servicio administradas para Windows 7 y Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Paso\-de cuentas de servicio en la guía de paso de\-](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planeamiento**|No disponible aún|
|**Implementación**|No disponible aún|
|**Operaciones**|[Cuentas de servicio administradas en Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Solución de problemas**|No disponible aún|
|**Evaluación**|[Introducción a las cuentas de servicio administradas de grupo](getting-started-with-group-managed-service-accounts.md)|
|**Herramientas y configuración**|[Cuentas de servicio administradas en Active Directory Domain Services](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Recursos de la comunidad**|[Cuentas de servicio administradas: Descripción, implementación, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologías relacionadas**|[Introducción a Active Directory Domain Services](active-directory-domain-services-overview.md)|


