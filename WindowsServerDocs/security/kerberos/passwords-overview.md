---
title: "Introducción a las contraseñas"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="passwords-overview"></a>Introducción a las contraseñas

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema para profesionales de TI describe las contraseñas que se usa en los sistemas operativos Windows y vínculos a documentación y discusiones sobre el uso de contraseñas en una estrategia de administración de credenciales.

## <a name="BKMK_OVER"></a>Descripción de la característica
Sistemas operativos y aplicaciones de hoy en día se diseñado alrededor de las contraseñas e incluso si usas tarjetas inteligentes o sistemas biométricos, aún, todas las cuentas tengan contraseñas y todavía pueden usarse en algunas circunstancias. Algunas cuentas, especialmente las cuentas que se utilizan para ejecutar servicios, incluso no pueden usar tarjetas inteligentes y tokens biométricos y, por tanto, deben usar una contraseña para autenticarse. Windows protege las contraseñas con hash criptográficos.

Para obtener más información acerca de las contraseñas de Windows, consulta [Introducción técnica a las contraseñas](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Aplicaciones prácticas
En Windows y muchos otros sistemas operativos, el método más común para autenticar la identidad de un usuario es usar una frase de contraseña secreta o contraseña. Proteger el entorno de red requiere que todos los usuarios usar contraseñas seguras. Esto ayuda a evitar la amenaza de un usuario malintencionado adivinar una contraseña no segura, ya sea a través de métodos manuales o con herramientas, para adquirir las credenciales de una cuenta de usuario en peligro. Esto es especialmente cierto para las cuentas administrativas. Cuando se cambia una contraseña compleja con regularidad, reduce la probabilidad de que un ataque de contraseña que se ponga en riesgo esa cuenta.

## <a name="BKMK_NEW"></a>Funcionalidades nuevas y modificadas
En Windows Server 2012 y Windows 8, las contraseñas de imagen son nuevas. Las contraseñas de imagen son una combinación de una imagen seleccionada del usuario junto con una serie de gestos. Funcionalidad de la contraseña de imagen está deshabilitada en los equipos unidos a un domain\. Vínculos para obtener más información acerca de las contraseñas de imagen se muestran en [ver también](#BKMK_LINKS) a continuación.

No ha habido ningún cambio en la funcionalidad de la contraseña de Windows Server 2012 y Windows 8. No se han agregado ninguna configuración de directiva de grupo nuevo. Sin embargo, mejoras y se han realizado en administración de \(and password\) de credenciales, como con las contraseñas de imagen, caja de seguridad e inicio de sesión Windows 8 con una cuenta de Microsoft, anteriormente conocido como un Windows Live ID.

## <a name="BKMK_DEP"></a>Funcionalidad en desuso
Ninguna funcionalidad de contraseña ha quedado en desuso en Windows Server 2012 y Windows 8.

## <a name="BKMK_SOFT"></a>Requisitos de software
En entornos empresariales, las contraseñas por lo general se administran con los servicios de dominio de Active Directory. También se pueden administrar las contraseñas en el equipo local con la configuración de directiva de contraseñas de configuración de seguridad, las directivas de cuenta local.

## <a name="BKMK_LINKS"></a>Consulta también
Esta tabla enumeran los recursos adicionales para las características de contraseña, administración de credenciales y tecnología.

|Tipo de contenido|Referencias|
|--------|-------|
|**Documentación de escenario**|[Protección de la identidad digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operaciones**|[Equipos y usuarios de active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Solución de problemas**|[Descubre cuando expira la contraseña \-Blog de PowerShell de Active Directory](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Seguridad**| Windows Server 2008 R2 y Windows 7 [Guía de amenazas y contramedidas: directivas de cuenta](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Orientación para [cambiar y crear contraseñas seguras](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Herramientas y opciones de configuración**|[Referencia de configuración de directiva de grupo para Windows y Windows Server en el centro de descarga de Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos de la Comunidad**|[Protección de la identidad digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Inicio de sesión en Windows 8 con un Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Inicio de sesión con una contraseña de imagen](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Optimizar la seguridad de contraseña de imagen](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


