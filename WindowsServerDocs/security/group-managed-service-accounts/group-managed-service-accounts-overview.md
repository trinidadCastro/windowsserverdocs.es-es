---
title: Group Managed Service Accounts Overview
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 24e3e3c15544de2f3bed4a7ef177b659e8095385
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836976"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema destinado a profesionales de TI, presentan la cuenta de servicio administrada de grupo describen aplicaciones prácticas, cambios en la implementación de Microsoft y los requisitos de hardware y software.


## <a name="BKMK_OVER"></a>Descripción de la característica
Una cuenta de servicio administrada (sMSA) independiente es una cuenta de dominio administrado que proporciona administración automática de contraseñas, administración de nombre de entidad de seguridad (SPN) del servicio simplificado y la capacidad de delegar la administración a otros administradores. Este tipo de cuenta de servicio administrada (MSA) se introdujo en Windows Server 2008 R2 y Windows 7.

El grupo de cuenta de servicio administrada (gMSA) proporciona la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad por medio de varios servidores. Al conectarse a un servicio hospedado en una granja de servidores, como solución de equilibrio de carga de red, los protocolos de autenticación que admiten la autenticación mutua requieren que todas las instancias de los servicios utilizan la misma entidad de seguridad. Cuando se utiliza una gMSA como entidades de servicio, el sistema operativo Windows administra la contraseña de la cuenta en lugar de confiar en el administrador para administrar la contraseña.

El servicio de distribución de claves de Microsoft \(kdssvc.dll\) proporciona el mecanismo para obtener la clave más reciente o una clave específica con un identificador de clave para una cuenta de Active Directory de forma segura. El servicio de distribución de claves comparte un secreto que se usa para crear claves para la cuenta. Estas claves se cambian de forma periódica. Para una gMSA el controlador de dominio procesa la contraseña en la clave proporcionada por el servicio de distribución de claves, además de otros atributos de la gMSA.  Hosts miembros pueden obtener los valores de contraseña actual y anterior poniéndose en un controlador de dominio.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Gmsa proporcionan una solución de identidad única para los servicios que se ejecutan en una granja de servidores o en los sistemas subyacentes del equilibrador de carga de red. Al proporcionar una solución de gMSA, servicios pueden configurarse para la gMSA nueva entidad de seguridad y la administración de contraseñas se controla por Windows.

Con una gMSA, servicios o los administradores de servicios no es necesario administrar la sincronización de contraseñas entre instancias de servicio. La gMSA es compatible con los hosts que se conservan sin conexión durante un período de tiempo prolongado y la administración de hosts miembros para todas las instancias de un servicio. Esto significa que es posible implementar una granja de servidores que admite una sola identidad con la que pueden autenticarse los equipos cliente actuales sin saber a qué instancia del servicio se conectan.

Los clústeres de conmutación por error no admiten las cuentas de servicio administradas de grupo (gMSA). Sin embargo, los servicios que se ejecutan sobre el Servicio de clúster pueden utilizar una gMSA o una cuenta de servicio administrada independiente (sMSA) si son servicios de Windows, grupos de aplicaciones o tareas programadas, o si admiten gSMA o sMSA de forma nativa.

## <a name="BKMK_SOFT"></a>Requisitos de software

Un 64\-arquitectura de bits es necesario para ejecutar los comandos de Windows PowerShell que se usan para administrar Gmsa.

Una cuenta de servicio administrada depende de los tipos de cifrado compatibles con Kerberos. Cuando un equipo cliente se autentica en un servidor con Kerberos, el controlador de dominio crea un vale de servicio Kerberos protegido con un cifrado que admiten tanto el controlador de dominio como el servidor. El controlador de dominio usa msDS de la cuenta\-SupportedEncryptionTypes de atributo para determinar qué se admite el cifrado del servidor y, si no hay ningún atributo, se supone que el equipo cliente no admite los tipos de cifrado más fuertes. Si el host está configurado para no admita RC4, autenticación siempre funcionará. Es por esto que el AES siempre se debe configurar expresamente para MSA.

> [!NOTE]
> Desde Windows Server 2008 R2, el DES está deshabilitado de forma predeterminada. Para obtener más información sobre los tipos de cifrado compatibles, consulte [Cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Gmsa no son aplicables a los sistemas operativos de Windows anteriores a Windows Server 2012.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor
No hay ningún paso de configuración necesarios para implementar MSA y gMSA con el administrador del servidor o la instalación\-cmdlet WindowsFeature.

## <a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente se proporcionan vínculos a recursos adicionales relacionados con las cuentas de servicio administradas y las cuentas de servicio administradas de grupo.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Novedades de cuentas de servicio administradas](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentación de Windows 7 y Windows Server 2008 R2 de cuentas de servicio administradas](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Cuentas de servicio paso\-por\-guía paso](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planeamiento**|No disponible aún|
|**Implementación**|No disponible aún|
|**Operaciones**|[Administra las cuentas de servicio en Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Solución de problemas**|No disponible aún|
|**Evaluación**|[Introducción a grupo de cuentas de servicio administradas](getting-started-with-group-managed-service-accounts.md)|
|**Herramientas y configuración**|[Administra las cuentas de servicio en servicios de dominio de Active Directory](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Recursos de la comunidad**|[Cuentas de servicio administradas: Descripción, implementación, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologías relacionadas**|[Introducción a servicios de dominio de Active Directory](active-directory-domain-services-overview.md)|


