---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Consideraciones sobre la topología de implementación de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838386"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Usar notificaciones de AD DS con AD FS
  
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
Puede permitir un mayor control de acceso para las aplicaciones federadas mediante el uso de servicios de dominio de Active Directory \(AD DS\)\-emite notificaciones de usuario y dispositivo junto con los servicios de federación de Active Directory \(AD FS \).  
  
## <a name="about-dynamic-access-control"></a>Acerca del Control de acceso dinámico  
En Windows Server® 2012, la característica de Control de acceso dinámico permite a las organizaciones conceder acceso a los archivos según las notificaciones de usuario \(que se obtienen los atributos de cuenta de usuario\) y notificaciones de dispositivo \(que se obtienen por atributos de la cuenta de equipo\) que son emitidos por los servicios de dominio de Active Directory \(AD DS\). Emitidas notificaciones de AD DS se integran en la autenticación integrada de Windows a través del protocolo de autenticación Kerberos.  
  
Para obtener más información acerca del Control de acceso dinámico, consulte [Guía de contenido de Control de acceso dinámico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>Novedades en AD FS  
Como una extensión para el escenario de Control de acceso dinámico, AD FS en Windows Server 2012 ahora pueden:  
  
-   Atributos de cuenta de equipo de acceso además de los atributos de cuenta de usuario desde dentro de AD DS. En versiones anteriores de AD FS, el servicio de federación no pudo acceder los atributos de cuenta de equipo en absoluto de AD DS.  
  
-   Usar AD DS emitidas notificaciones de usuario o dispositivo que residan en un vale de autenticación de Kerberos. En versiones anteriores de AD FS, el motor de notificaciones era capaz de leer los identificadores de usuario y grupo de seguridad \(SID\) de Kerberos, pero no era capaz de leer cualquier información contenida en un vale de Kerberos de notificaciones.  
  
-   Transformación de AD DS que emite las notificaciones de dispositivo o usuario en los tokens SAML que las aplicaciones de usuario de confianza pueden usar para realizar un mayor control de acceso.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Ventajas del uso de AD DS notificaciones con AD FS  
Estos emitidas notificaciones de AD DS se puede insertar en los vales de autenticación de Kerberos y usar con AD FS para proporcionar las siguientes ventajas:  
  
-   Pueden permitir que las organizaciones que requieren las directivas de control de acceso más ricas notificaciones\-basada en acceso a aplicaciones y recursos con AD DS emite las notificaciones que se basan en los valores de atributo que se almacenan en AD DS para una cuenta de usuario o equipo determinada. Esto puede ayudar a los administradores a reducir la sobrecarga adicional asociada con la creación y administración:  
  
    -   Grupos de seguridad de AD DS que de lo contrario se utilizaría para controlar el acceso a aplicaciones y recursos que son accesibles a través de la autenticación integrada de Windows.  
  
    -   Bosque de confianza que de lo contrario se utilizaría para controlar el acceso a los negocios\-a\-Business \(B2B\) \/ aplicaciones accesibles de Internet y los recursos.  
  
-   Las organizaciones ahora pueden impedir el acceso no autorizado a recursos de red de los equipos cliente en función de si una cuenta de equipo específica almacenada en AD DS de valor del atributo \(por ejemplo, el nombre DNS de un equipo\) coincide con el control de acceso directiva del recurso \(por ejemplo, un servidor de archivos que se encuentra con notificaciones\) o la directiva de usuario de confianza \(por ejemplo, un notificaciones\-aplicación Web compatible con\). Esto puede ayudar a los administradores establecer directivas de control de acceso más precisas de los recursos o las aplicaciones que son:  
  
    -   Solo es accesible a través de la autenticación integrada de Windows.  
  
    -   Internet accesible a través de mecanismos de autenticación de AD FS. AD FS se puede usar para transformar emitidas notificaciones de dispositivo en las notificaciones de AD FS que se pueden encapsular en tokens SAML que pueden consumir un recurso accesible de Internet o la aplicación del usuario autenticado de AD DS.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Diferencias entre AD DS y AD FS emite notificaciones  
Hay dos factores diferenciadores que son importantes para comprender acerca de las notificaciones que se emiten desde AD DS vs. AD FS. Estas diferencias incluyen:  
  
-   AD DS solo pueden emitir notificaciones que se encapsulan en vales de Kerberos, no se consideran tokens SAML. Para obtener más información sobre cómo AD DS emite notificaciones, consulte [Guía de contenido de Control de acceso dinámico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS sólo puede emitir notificaciones que se encapsulan en los tokens SAML, no los vales de Kerberos. Para obtener más información sobre cómo AD FS emite notificaciones, consulte [The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Cómo funcionan las notificaciones emitidas por AD DS con AD FS  
Emitidas notificaciones de AD DS se puede usar con AD FS para tener acceso a las notificaciones de usuario y dispositivo directamente desde contexto de autenticación, en el usuario lugar de realizar una llamada independiente de LDAP a Active Directory. La siguiente ilustración y los pasos correspondientes se describe cómo funciona este proceso con más detalle para habilitar notificaciones\-en función de control de acceso para el escenario de Control de acceso dinámico.  
  
![uso de notificaciones](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un administrador de AD DS usa la consola de centro de administración de Active Directory o los cmdlets de PowerShell para objetos de tipo de notificación concreta habilita en el esquema de AD DS.  
  
2.  Un administrador de AD FS usa la consola de administración de AD FS para crear y configurar el proveedor de notificaciones y de confianza confianzas con pass\-a través o las reglas de notificación de transformación.  
  
3.  Un cliente de Windows intenta tener acceso a la red. Como parte del proceso de autenticación Kerberos, el cliente presenta su vale de usuario y equipo\-concesión de vales \(TGT\) que aún no contiene las notificaciones para el controlador de dominio. El controlador de dominio incluye las notificaciones resultantes en el vale de Kerberos devuelto y, a continuación, busca los tipos de notificación habilitada en AD DS.  
  
4.  Cuando el usuario\/cliente intenta obtener acceso a un recurso de archivo que se encuentra para solicitar las notificaciones, pueden tener acceso al recurso porque el identificador compuesto que aparecía de Kerberos tiene estas notificaciones.  
  
5.  Cuando el mismo cliente intenta tener acceso a un sitio Web o aplicación Web que está configurada para la autenticación de AD FS, se redirige al usuario a un servidor de federación de AD FS que está configurado para la autenticación integrada de Windows. El cliente envía una solicitud al controlador de dominio con Kerberos. El controlador de dominio emite un vale de Kerberos que contiene las notificaciones solicitadas que el cliente, a continuación, puede presentar al servidor de federación.  
  
6.  Según la manera en que se han configurado las reglas de notificaciones en el proveedor de notificaciones y autenticado que el administrador configurado previamente, AD FS lee las notificaciones del vale Kerberos y los incluye en un token SAML que emite para el cliente.  
  
7.  El cliente recibe el token SAML que contiene las notificaciones correctas y, a continuación, se redirige al sitio Web.  
  
Para obtener más información sobre cómo crear las reglas de notificación necesarias para AD DS emite notificaciones para que funcione con AD FS, consulte [crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
