---
title: Información general sobre contraseñas
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 1b917691f931836605cfe044c725f5200925d2ca
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621887"
---
# <a name="passwords-overview"></a>Información general sobre contraseñas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describen las contraseñas que se usan en los sistemas operativos Windows y se incluyen vínculos a documentación y debates sobre el uso de contraseñas en una estrategia de administración de credenciales.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
Los sistemas operativos y las aplicaciones de hoy en día se diseñan en torno a las contraseñas y, incluso si usa tarjetas inteligentes o sistemas biométricos, todas las cuentas todavía tienen contraseñas y todavía se pueden usar en algunas circunstancias. Algunas cuentas, especialmente las que se usan para ejecutar servicios, no pueden usar tarjetas inteligentes ni tokens biométricos y, por tanto, deben usar una contraseña para autenticarse. Windows protege las contraseñas mediante hashes criptográficos.

Para obtener más información acerca de las contraseñas de Windows, consulte [Introducción técnica a las contraseñas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994558(v=ws.10)).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
En Windows y muchos otros sistemas operativos, el método más común para autenticar la identidad de un usuario es usar una contraseña o frase de contraseña secreta. Para proteger el entorno de red, todos los usuarios deben usar contraseñas seguras. Esto ayuda a evitar la amenaza de que un usuario malintencionado adivine una contraseña no segura, ya sea a través de métodos manuales o mediante herramientas, para adquirir las credenciales de una cuenta de usuario en peligro. Esto es especialmente cierto para las cuentas administrativas. Cuando se cambia una contraseña compleja con regularidad, se reduce la probabilidad de que un ataque de contraseña ponga en peligro esa cuenta.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
En Windows Server 2012 y Windows 8, las contraseñas de imagen son nuevas. Las contraseñas de imagen son una combinación de una imagen seleccionada por el usuario junto con una serie de gestos. La funcionalidad de contraseña de imagen está deshabilitada en equipos Unidos a un dominio \- . Los vínculos a más información acerca de las contraseñas de imagen se [muestran en la lista que aparece a](#BKMK_LINKS) continuación.

No se ha producido ningún cambio en la funcionalidad de contraseña en Windows Server 2012 y Windows 8. No se ha agregado ninguna nueva configuración de directiva de grupo. Sin embargo, se han realizado mejoras y mejoras en la administración de credenciales \( y contraseñas \) , como con contraseñas de imagen, caja de control de credenciales e iniciar sesión en Windows 8 con un cuenta de Microsoft, anteriormente conocido como Windows Live ID.

## <a name="deprecated-functionality"></a><a name="BKMK_DEP"></a>Funciones obsoletas
No se ha dejado de usar ninguna funcionalidad de contraseña en Windows Server 2012 y Windows 8.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
En entornos empresariales, las contraseñas se administran normalmente con Active Directory Domain Services. Las contraseñas también se pueden administrar en el equipo local con las opciones de configuración de seguridad local, directivas de cuenta y Directiva de contraseñas.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vea también
En esta tabla se enumeran recursos adicionales para las características de contraseña, la tecnología y la administración de credenciales.

|Tipo de contenido|Referencias|
|--------|-------|
|**Documentación del escenario**|[Proteger la identidad digital](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operaciones**|[Usuarios y equipos de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754217(v=ws.11))|
|**Solución de problemas**|[Averigüe cuándo expira la contraseña \- Active Directory blog de PowerShell](https://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Seguridad**| [Guía de amenazas y contramedidas de](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125920(v=ws.10)) windows Server 2008 R2 y Windows 7: directivas de cuenta<p>Instrucciones para [cambiar y crear contraseñas seguras](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Herramientas y configuración**|[Referencia de la configuración de directiva de grupo para Windows y Windows Server en el centro de descarga de Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos de la comunidad**|[Proteger la identidad digital](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<p>[Iniciar sesión en Windows 8 con un Windows Live ID](https://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<p>[Iniciar sesión con una contraseña de imagen](/archive/blogs/b8/signing-in-with-a-picture-password)<p>[Optimización de la seguridad de la contraseña de imagen](/archive/blogs/b8/optimizing-picture-password-security)|