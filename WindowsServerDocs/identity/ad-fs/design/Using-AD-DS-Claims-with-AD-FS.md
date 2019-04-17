---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: "Consideraciones de la topología de implementación de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Uso de AD DS dice con AD FS
  
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
Puedes habilitar el control de acceso más enriquecido para aplicaciones federados mediante el uso de usuario de los servicios de dominio de Active Directory \(AD DS\)\-issued y de dispositivo junto con los servicios de federación de Active Directory \(AD FS\).  
  
## <a name="about-dynamic-access-control"></a>Acerca del Control de acceso dinámico  
En Windows Server® 2012, la característica de Control de acceso dinámico permite a las organizaciones conceder acceso a archivos en función de notificaciones de usuario \ (que se originados por attributes\ de cuenta de usuario) y de dispositivo \ (que se originados por attributes\ de cuenta de equipo) que se emiten por \(AD DS\) los servicios de dominio de Active Directory. AD DS emitidos reclamaciones están integrados en la autenticación integrada de Windows a través del protocolo de autenticación de Kerberos.  
  
Para obtener más información sobre el Control de acceso dinámico, consulta [plan de contenido de Control de acceso dinámico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>¿Novedades en AD FS?  
Como una extensión para el escenario de Control de acceso dinámico, AD FS en Windows Server 2012 ahora pueden:  
  
-   Acceder a atributos de cuenta de equipo además de atributos de cuenta de usuario desde AD DS. En versiones anteriores de AD FS, el servicio de federación podría no tener acceso a atributos de la cuenta de equipo en absoluto de AD DS.  
  
-   Consumir AD DS emitido reclamaciones de usuario o dispositivo que residen en un vale de autenticación de Kerberos. En versiones anteriores de AD FS, el motor de reclamaciones podía lee el usuario y grupo seguridad identificadores \(SIDs\) de Kerberos, pero no se puede leer cualquiera dentro de un vale Kerberos de información de notificaciones.  
  
-   Transformar AD DS que emitió el usuario o dispositivo reclamaciones en tokens SAML que las aplicaciones de confianza pueden utilizar para realizar el control de acceso más enriquecido.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Ventajas de usar AD DS reclamaciones con AD FS  
Estos emitidos reclamaciones de AD DS puede insertarse vales de autenticación de Kerberos y usarse con AD FS para proporcionar las siguientes ventajas:  
  
-   Las organizaciones que requieren directivas de control de acceso más enriquecidas pueden habilitar el acceso basado en claims\ a aplicaciones y los recursos mediante emitido notificaciones que se basan en los valores de atributo que se almacene en AD DS para un usuario determinado o una cuenta de equipo de AD DS. Esto puede ayudar a los administradores para reducir la sobrecarga adicional asociada con la creación y administración:  
  
    -   AD DS grupos de seguridad en caso contrario, se usaría para controlar el acceso a los recursos que están disponibles a través de la autenticación integrada de Windows y aplicaciones.  
  
    -   Bosque de confianza que de lo contrario, se usaría para controlar el acceso a Business\-to\-Business \(B2B\) \ / aplicaciones accesibles a través de Internet y de recursos.  
  
-   Las organizaciones ahora pueden impedir el acceso no autorizado a los recursos de red de los equipos cliente en función de si una cuenta de equipo específicos almacenado en AD DS de valor del atributo \ (por ejemplo, de un equipo DNS equipo\) coincide con la directiva de control de acceso del recurso \ (por ejemplo, un servidor de archivos que se encuentra con claims\) o la directiva de terceros de confianza \ (por ejemplo, application\ Web con reconocimiento de claims\). Esto puede ayudar a los administradores definir directivas de control de acceso más preciso de los recursos o aplicaciones que son:  
  
    -   Solo es accesible a través de la autenticación integrada de Windows.  
  
    -   Internet accesible a través de mecanismos de autenticación de AD FS. AD FS puede usarse para transformar emitido reclamaciones de dispositivo a las notificaciones de AD FS pueden encapsularse en tokens SAML que pueden consumir un recurso accesible de Internet o una aplicación de terceros de confianza de AD DS.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Diferencias entre AD DS y AD FS emitido reclamaciones  
Existen dos factores diferenciadoras que son importantes para comprender acerca de las notificaciones que se emiten desde AD DS vs. AD FS. Estas diferencias incluyen:  
  
-   AD DS solo puede emitir notificaciones que están encapsuladas en vales Kerberos, no los tokens SAML. Para obtener más información acerca de cómo los problemas de notificaciones de AD DS, consulta [plan de contenido de Control de acceso dinámico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS solo puede emitir notificaciones que están encapsuladas en tokens SAML, no los vales Kerberos. Para obtener más información acerca de cómo AD FS problemas de notificaciones, consulta [el rol del motor reclamaciones](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Cómo AD DS emite el trabajo de Reclamación con AD FS  
Emitido reclamaciones de AD DS puede usarse con AD FS para acceder a las reclamaciones de usuarios y dispositivos directamente desde contexto de autenticación, en el usuario lugar de realizar una llamada independiente de LDAP a Active Directory. La siguiente ilustración y los pasos correspondientes se describe cómo funciona este proceso con más detalle para habilitar el control de acceso basado en claims\ para el escenario de Control de acceso dinámico.  
  
![uso de notificaciones](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un administrador de AD DS, usa la consola de centro de administración de Active Directory o los cmdlets de PowerShell para objetos de tipo de notificación específico permite en el esquema de AD DS.  
  
2.  Un administrador de AD FS usa la consola de administración de AD FS para crear y configurar el proveedor de notificaciones y el usuario de confianza confianzas con cualquiera pass\ mediante o reglas de notificaciones de transformación.  
  
3.  Un cliente de Windows intenta acceder a la red. Como parte del proceso de autenticación Kerberos, el cliente presenta al usuario y equipo ticket\ concesión de vales de \(TGT\) que no aún contener cualquier reclamación al controlador de dominio. El controlador de dominio, a continuación, busca los tipos de notificación habilitado en AD DS e incluye cualquier reclamación resultante en la solicitud de Kerberos devuelta.  
  
4.  Cuando el cliente de user\ intenta obtener acceso a un recurso de archivo que se encuentra requerir las notificaciones, pueden acceder a los recursos porque el identificador de compuesta que se expone de Kerberos tiene estas notificaciones.  
  
5.  Cuando el mismo cliente intente acceder a una sitio Web o aplicación Web que está configurada para la autenticación de AD FS, se redirige al usuario a un servidor de federación de AD FS que está configurado para la autenticación integrada de Windows. El cliente envía una solicitud al controlador de dominio con Kerberos. El controlador de dominio emite un vale Kerberos que contiene las notificaciones solicitadas que, a continuación, puede presentar el cliente al servidor de federación.  
  
6.  En función de la manera en que se han configurado las reglas de notificaciones en el proveedor de notificaciones y confiando confianzas de terceros que el administrador ha configurado anteriormente, AD FS lee las notificaciones desde el vale Kerberos e incluye en un token SAML que emite para el cliente.  
  
7.  El cliente recibe el token de SAML que contiene las notificaciones correctas y, a continuación, se redirige al sitio Web.  
  
Para obtener más información sobre cómo crear las reglas de notificación necesarias para notificaciones de AD DS emitido para trabajar con AD FS, consulta [crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
