---
title: Group Managed Service Accounts Overview
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 09405b940e9fd862372fe80c4a5194caa205e5ea
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991496"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se presenta la cuenta de servicio administrada de grupo mediante la descripción de las aplicaciones prácticas, los cambios en la implementación de Microsoft y los requisitos de hardware y software.


## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
Una cuenta de servicio administrada independiente (sMSA) es una cuenta de dominio administrada que proporciona administración automática de contraseñas, administración de nombres de entidad de seguridad de servicio (SPN) simplificada y la capacidad de delegar la administración a otros administradores. Este tipo de cuenta de servicio administrada (MSA) se incorporó por primera vez en Windows Server 2008 R2 y Windows 7.

La cuenta de servicio administrada de grupo (gMSA) proporciona la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad en varios servidores. Al conectarse a un servicio hospedado en una granja de servidores, como la solución de equilibrio de carga de red, los protocolos de autenticación que admiten la autenticación mutua requieren que todas las instancias de los servicios usen la misma entidad de seguridad. Cuando se usa un gMSA como entidad de servicio, el sistema operativo Windows administra la contraseña de la cuenta en lugar de confiar en el administrador para administrar la contraseña.

El servicio de distribución de claves de Microsoft \(kdssvc.dll\) proporciona el mecanismo para obtener de manera segura la clave más reciente o una clave específica con un identificador de clave para una cuenta de Active Directory. El servicio de distribución de claves comparte un secreto que se usa para crear claves para la cuenta. Estas claves se cambian de forma periódica. En el caso de una gMSA, el controlador de dominio calcula la contraseña en la clave proporcionada por los servicios de distribución de claves, además de otros atributos de gMSA.  Los hosts miembros pueden obtener los valores de contraseña actuales y anteriores poniéndose en contacto con un controlador de dominio.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
GMSA proporcionan una solución de identidad única para los servicios que se ejecutan en una granja de servidores o en sistemas que están detrás de Load Balancer de red. Al proporcionar una solución de gMSA, los servicios se pueden configurar para la nueva entidad de seguridad de gMSA y Windows controla la administración de contraseñas.

El uso de gMSA, los servicios o los administradores de servicios no necesitan administrar la sincronización de contraseñas entre las instancias de servicio. GMSA admite hosts que se mantienen sin conexión durante un período de tiempo prolongado y la administración de hosts miembros para todas las instancias de un servicio. Esto significa que es posible implementar una granja de servidores que admite una sola identidad con la que pueden autenticarse los equipos cliente actuales sin saber a qué instancia del servicio se conectan.

Los clústeres de conmutación por error no admiten las cuentas de servicio administradas de grupo (gMSA). Sin embargo, los servicios que se ejecutan sobre el Servicio de clúster pueden utilizar una gMSA o una cuenta de servicio administrada independiente (sMSA) si son servicios de Windows, grupos de aplicaciones o tareas programadas, o si admiten gSMA o sMSA de forma nativa.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software

\-Se necesita una arquitectura de 64 bits para ejecutar los comandos de Windows PowerShell que se usan para administrar GMSA.

Una cuenta de servicio administrada depende de los tipos de cifrado compatibles con Kerberos. Cuando un equipo cliente se autentica en un servidor con Kerberos, el controlador de dominio crea un vale de servicio Kerberos protegido con un cifrado que admiten tanto el controlador de dominio como el servidor. El controlador de dominio usa el atributo msDS SupportedEncryptionTypes de la cuenta \- para determinar el cifrado que el servidor admite y, si no hay ningún atributo, supone que el equipo cliente no admite tipos de cifrado más seguros. Si el host está configurado para no admitir RC4, la autenticación siempre producirá un error. Es por esto que el AES siempre se debe configurar expresamente para MSA.

> [!NOTE]
> Desde Windows Server 2008 R2, el DES está deshabilitado de forma predeterminada. Para obtener más información sobre los tipos de cifrado compatibles, consulte [Cambios en la autenticación Kerberos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10)).

GMSA no se aplican a los sistemas operativos Windows anteriores a Windows Server 2012.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor
No hay ningún paso de configuración necesario para implementar MSA y gMSA con Administrador del servidor o el \- cmdlet install WindowsFeature.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente se proporcionan vínculos a recursos adicionales relacionados con las cuentas de servicio administradas y las cuentas de servicio administradas de grupo.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[What's New for Managed Service Accounts](what-s-new-for-managed-service-accounts.md)<p>[Documentación de cuentas de servicio administradas para Windows 7 y Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641731(v=ws.10))<p>[\-Guía paso a paso de las cuentas de servicio \-](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548356(v=ws.10))|
|**Planeamiento**|No disponible todavía|
|**Implementación**|No disponible todavía|
|**Operaciones**|[Cuentas de servicio administradas en Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378925(v=ws.10))|
|**Solución de problemas**|No disponible todavía|
|**Evaluación**|[Introducción con cuentas de servicio administradas de grupo](getting-started-with-group-managed-service-accounts.md)|
|**Herramientas y configuración**|[Cuentas de servicio administradas en Active Directory Domain Services](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378925(v=ws.10))|
|**Recursos de la comunidad**|[Cuentas de servicio administradas: Comprensión, implementación, procedimientos recomendados y solución de problemas](/archive/blogs/askds/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting)|
|**Tecnologías relacionadas**|[Introducción a Active Directory Domain Services](active-directory-domain-services-overview.md)|