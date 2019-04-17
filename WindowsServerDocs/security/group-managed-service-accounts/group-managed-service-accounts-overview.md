---
title: Introducción a las cuentas servicio administrado grupo
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
ms.openlocfilehash: 4912ae273e603b4a3362c4984da710f780c3e8b3
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2018
---
# <a name="group-managed-service-accounts-overview"></a>Introducción a las cuentas servicio administrado grupo

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI ofrece la cuenta del servicio administrado de grupo con la descripción de las aplicaciones prácticas, los cambios en la implementación de Microsoft y los requisitos de hardware y software.


## <a name="BKMK_OVER"></a>Descripción de la característica
Independiente cuentas de servicio administradas, que se introdujeron en Windows Server 2008 R2 y Windows 7, son cuentas de dominio administrado que proporcionan administración automática de contraseñas y administración simplificada de SPN, como la delegación de administración a otros administradores.

El grupo de cuenta de servicio administrada proporciona la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad a través de varios servidores. Cuando se conecta a un servicio hospedado en una granja de servidores, como Equilibrio de carga de red, los protocolos de autenticación que admite la autenticación mutua requieren que todas las instancias de los servicios de usan al mismo principal. Cuando se usan cuentas de servicio administrado de grupo como entidades de servicio, el sistema operativo Windows administra la contraseña de la cuenta en lugar de usar el administrador para administrar la contraseña.

El servicio de distribución de claves de Microsoft de \(kdssvc.dll\) proporciona el mecanismo para obtener de forma segura la clave más reciente o una clave con un identificador de clave específica de una cuenta de Active Directory. El servicio de distribución de claves comparte una clave secreta que se usa para crear claves para la cuenta. Estas claves se cambian periódicamente. Para un cuenta de servicio administrado de grupo del controlador de dominio calcula la contraseña de la clave proporcionada por los servicios de distribución de claves, además de otros atributos de la cuenta de servicio administrado de grupo.  Hosts miembros pueden obtener los valores de la contraseña actuales y anteriores en contacto con un controlador de dominio.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Cuentas de servicio administradas grupo proporcionan una solución de identidad única para servicios que se ejecutan en una granja de servidores o en sistemas de equilibrio de carga de red. Al proporcionar una solución MSA de grupo, servicios pueden configurarse para el nuevo grupo MSA principal y se controla la administración de contraseñas por Windows.

Usar un grupo de cuenta de servicio administrada, servicios o los administradores de servicio no es necesario administrar la sincronización de contraseñas entre instancias de servicio. El grupo de cuenta de servicio administrada admite hosts que se conservan sin conexión durante un período de tiempo prolongado y la administración de hosts de miembros para todas las instancias de un servicio. Esto significa que puedes implementar una granja de servidores que admite una única identidad a la que puedan autenticar los equipos cliente existentes sin necesidad de conocer la instancia del servicio al que se conectan.

Clústeres de conmutación por error no se admiten gMSAs. Sin embargo, los servicios que se ejecutan en el servicio de clúster usar un gMSA o un sMSA si son un servicio de Windows, un grupo de la aplicación, una tarea programada o admiten de forma nativa gMSA o sMSA.

## <a name="BKMK_SOFT"></a>Requisitos de software

Una arquitectura 64\ bits se necesita para ejecutar los comandos de Windows PowerShell que se usan para administrar las cuentas de servicio administradas de grupo.

Una cuenta de servicio administrada depende de los tipos de cifrado Kerberos compatibles. Cuando un equipo cliente se autentica a un servidor con Kerberos crea el controlador de dominio un vale de servicio Kerberos protegida con cifrado admite DC y el servidor. El controlador de dominio utiliza el atributo de msDS\ SupportedEncryptionTypes de la cuenta para determinar qué es compatible con el servidor de cifrado y, si no hay ningún atributo, se supone que el equipo cliente no admite tipos de cifrado más sólidos. Si el host está configurado para no admitir el cifrado RC4, autenticación siempre funcionará. Por este motivo, los AES siempre debe configurarse explícitamente para MSA.

> [!NOTE]
> A partir de Windows Server 2008 R2, DES está deshabilitado de manera predeterminada. Para obtener más información acerca de los tipos de cifrado compatibles, consulta [cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Cuentas de servicio administradas de grupo no son aplicables a sistemas operativos Windows anteriores a Windows Server 2012.

## <a name="server-manager-information"></a>Información del administrador del servidor
No hay ningún paso de configuración necesarios para implementar MSA y MSA con el administrador del servidor o el cmdlet WindowsFeature Install\ de grupo.

## <a name="BKMK_LINKS"></a>Consulta también
La siguiente tabla proporciona vínculos a recursos adicionales relacionados con cuentas de servicio administradas y cuentas de servicio administradas de grupo.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Novedades para cuentas de servicio administradas](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentación de Windows 7 y Windows Server 2008 R2 de cuentas de servicio administrada](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Guía paso a Step\-by\ de cuentas de servicio](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planeación**|Aún no está disponible|
|**Implementación**|Aún no está disponible|
|**Operaciones**|[Administra las cuentas de servicio en Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Solución de problemas**|Aún no está disponible|
|**Evaluación**|[Introducción a grupo administra las cuentas de servicio](getting-started-with-group-managed-service-accounts.md)|
|**Herramientas y opciones de configuración**|[Administra las cuentas de servicio en servicios de dominio de Active Directory](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Recursos de la Comunidad**|[Descripción de las cuentas de servicio administradas:, Implementar, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologías relacionadas**|[Información general de servicios de dominio de Active Directory](active-directory-domain-services-overview.md)|


