---
title: Información general sobre contraseñas
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869606"
---
# <a name="passwords-overview"></a>Información general sobre contraseñas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI describe las contraseñas que se usa en los sistemas operativos de Windows y vínculos a documentación y las discusiones sobre el uso de contraseñas en una estrategia de administración de credenciales.

## <a name="BKMK_OVER"></a>Descripción de la característica
Sistemas operativos y aplicaciones de hoy en día están diseñadas en torno a las contraseñas e incluso si usa tarjetas inteligentes o sistemas biométricos, todas las cuentas aún tener contraseñas y todavía se puede usar en algunas circunstancias. Algunas cuentas, en particular las cuentas utilizadas para ejecutar servicios, no pueden incluso usar tarjetas inteligentes y testigos biométricas y, por tanto, deben usar una contraseña para autenticarse. Windows protege las contraseñas mediante algoritmos hash criptográficos.

Para obtener más información acerca de las contraseñas de Windows, consulte [información técnica sobre contraseñas](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Aplicaciones prácticas
En Windows y muchos otros sistemas operativos, el método más común para autenticar la identidad del usuario es usar una frase de contraseña secreta o contraseña. Proteger el entorno de red requiere que todos los usuarios utilicen contraseñas seguras. Esto ayuda a evitar la amenaza de usuarios malintencionados que adivinen una contraseña poco segura, ya sea a través de métodos manuales o mediante el uso de herramientas, para adquirir las credenciales de una cuenta de usuario en peligro. Esto es especialmente cierto para las cuentas administrativas. Cuando se cambia una contraseña compleja con regularidad, reduce la probabilidad de un ataque de contraseña poner en peligro de esa cuenta.

## <a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
En Windows Server 2012 y Windows 8, las contraseñas de imagen son nuevas. Las contraseñas de imagen son una combinación de una imagen de usuario seleccionada junto con una serie de gestos. Funcionalidad de contraseña de imagen está deshabilitada en el dominio\-equipos unidos a un. Vínculos para obtener más información acerca de las contraseñas de imagen se muestran en [Vea también](#BKMK_LINKS) a continuación.

No ha habido ningún cambio en la funcionalidad de contraseña de Windows Server 2012 y Windows 8. Se han agregado ninguna nueva configuración de directiva de grupo. Sin embargo, se han realizado mejoras en credencial \(y la contraseña\) administración, como con las contraseñas de imagen, caja de seguridad e iniciar sesión en Windows 8 con una cuenta de Microsoft, anteriormente conocido como Windows Live ID .

## <a name="BKMK_DEP"></a>Funcionalidad desusada
Se quedó obsoleta ninguna funcionalidad de contraseña en Windows Server 2012 y Windows 8.

## <a name="BKMK_SOFT"></a>Requisitos de software
En entornos empresariales, normalmente se administran las contraseñas con los servicios de dominio de Active Directory. También se pueden administrar las contraseñas en el equipo local mediante la configuración de directiva de contraseñas local de configuración de seguridad, directivas de cuenta.

## <a name="BKMK_LINKS"></a>Vea también
Esta tabla muestra recursos adicionales para las características de la contraseña, administración de tecnología y las credenciales.

|Tipo de contenido|Referencias|
|--------|-------|
|**Documentación del escenario**|[Proteger la identidad digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operaciones**|[Los equipos y usuarios de active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Solución de problemas**|[Descubra cuándo expira la contraseña \- PowerShell Blog de Active Directory](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Seguridad**| Windows Server 2008 R2 y Windows 7 [Guía de amenazas y contramedidas: Directivas de cuenta](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Guía para [cambiar y crear contraseñas seguras](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Herramientas y configuración**|[Referencia de configuración de directiva de grupo para Windows y Windows Server en el centro de descarga de Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos de la comunidad**|[Proteger la identidad digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Inicio de sesión en Windows 8 con un Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Inicio de sesión con una contraseña de imagen](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Optimizar la seguridad de la contraseña de imagen](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


