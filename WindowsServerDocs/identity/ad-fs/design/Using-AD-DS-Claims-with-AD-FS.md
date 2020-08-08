---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Usar notificaciones de AD DS con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 942b7d88196fa32dd70fd554d76547cc5feadb26
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967552"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Usar notificaciones de AD DS con AD FS


Puede habilitar el control de acceso más completo para aplicaciones federadas mediante Active Directory Domain Services \( AD DS \) \- notificaciones de usuario y de dispositivo emitidas junto con servicios de Federación de Active Directory (AD FS) \( AD FS \) .

## <a name="about-dynamic-access-control"></a>Acerca de Access Control dinámico
En Windows Server &reg; 2012, la característica de Access Control dinámica permite a las organizaciones conceder acceso a los archivos en función de las notificaciones de usuario \( que provienen de los atributos de la cuenta de usuario \) y las notificaciones de dispositivo \( que tienen como origen los atributos de cuenta de equipo \) que emite Active Directory Domain Services \( AD DS \) . AD DS las notificaciones emitidas se integran en la autenticación integrada de Windows a través del Protocolo de autenticación Kerberos.

Para obtener más información sobre el Access Control dinámico, vea [mapa de ruta de contenido de Access control dinámico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).

### <a name="whats-new-in-ad-fs"></a>Novedades de AD FS
Como extensión del escenario de Access Control dinámico, AD FS en Windows Server 2012 ahora pueden:

-   Obtener acceso a los atributos de la cuenta de equipo, además de a los atributos de cuenta de usuario, dentro de AD DS. En versiones anteriores de AD FS, el Servicio de federación no podía tener acceso a los atributos de la cuenta de equipo desde AD DS.

-   Consume AD DS notificaciones de usuario o dispositivo emitidas que residen en un vale de autenticación Kerberos. En versiones anteriores de AD FS, el motor de notificaciones podía leer los SID de los identificadores de seguridad de usuarios y grupos \( \) de Kerberos, pero no podía leer la información de notificaciones contenida dentro de un vale de Kerberos.

-   Transforme AD DS notificaciones de usuario o dispositivo emitidas en tokens SAML que las aplicaciones de confianza pueden usar para realizar un control de acceso más completo.

## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Ventajas del uso de notificaciones de AD DS con AD FS
Estas AD DS notificaciones emitidas se pueden insertar en vales de autenticación Kerberos y usarse con AD FS para proporcionar las siguientes ventajas:

-   Las organizaciones que requieren directivas de control de acceso más enriquecidas pueden habilitar \- el acceso basado en notificaciones a aplicaciones y recursos mediante AD DS notificaciones emitidas que se basan en los valores de atributo almacenados en AD DS para una cuenta de usuario o de equipo determinada. Esto puede ayudar a los administradores a reducir la sobrecarga adicional asociada a la creación y administración de:

    -   AD DS grupos de seguridad que, de otro modo, se utilizarían para controlar el acceso a las aplicaciones y recursos a los que se puede tener acceso a través de la autenticación integrada de Windows.

    -   Confianzas de bosque que, de lo contrario, se utilizarían para controlar el acceso a \- \- \( \) \/ recursos y aplicaciones accesibles a Internet empresariales de empresa a negocio.

-   Las organizaciones ahora pueden impedir el acceso no autorizado a los recursos de red de los equipos cliente en función de si un valor de atributo de cuenta de equipo específico almacenado en AD DS \( por ejemplo, el nombre DNS de un equipo \) coincide con la Directiva de control de acceso del recurso \( , por ejemplo, un servidor de archivos que se ha incluirse con notificaciones \) o la directiva \( de usuario de confianza, por ejemplo, una \- \) Esto puede ayudar a los administradores a establecer directivas de control de acceso más precisas para los recursos o las aplicaciones que:

    -   Solo accesible a través de la autenticación integrada de Windows.

    -   Internet accesible a través de AD FS mecanismos de autenticación. AD FS puede usarse para transformar AD DS notificaciones de dispositivo emitidas en AD FS notificaciones que se pueden encapsular en tokens de SAML que se pueden usar en un recurso accesible a Internet o en una aplicación de usuario de confianza.

## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Diferencias entre AD DS y AD FS las notificaciones emitidas
Hay dos factores diferenciales que es importante comprender sobre las notificaciones que se emiten desde AD DS frente AD FS. Estas diferencias incluyen:

-   AD DS solo puede emitir notificaciones que se encapsulan en vales Kerberos, no en tokens SAML. Para obtener más información sobre cómo AD DS notificaciones de problemas, consulte el [mapa de ruta de contenido de Access control dinámico](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).

-   AD FS solo puede emitir notificaciones que se encapsulan en tokens SAML, no en vales Kerberos. Para obtener más información sobre cómo AD FS notificaciones de problemas, vea [el rol del motor de notificaciones](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).

## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Cómo funcionan las notificaciones emitidas por AD DS con AD FS
AD DS se pueden usar notificaciones emitidas con AD FS para tener acceso a las notificaciones de usuario y de dispositivo directamente desde el contexto de autenticación del usuario, en lugar de realizar una llamada LDAP independiente a Active Directory. La siguiente ilustración y los pasos correspondientes describen cómo funciona este proceso con más detalle para habilitar \- el control de acceso basado en notificaciones para el escenario de Access control dinámico.

![uso de notificaciones](media/UsingADDSClaimswithADFS.gif)

1.  Un administrador de AD DS usa la consola de Centro de administración de Active Directory o cmdlets de PowerShell para habilitar objetos de tipo de notificaciones específicos en el esquema de AD DS.

2.  Un administrador de AD FS usa la consola de administración de AD FS para crear y configurar el proveedor de notificaciones y las relaciones de confianza para usuario autenticado con reglas de notificación de paso \- a través o de transformación.

3.  Un cliente de Windows intenta tener acceso a la red. Como parte del proceso de autenticación de Kerberos, el cliente presenta el TGT del vale de concesión de vales de usuario y equipo \- \( \) que todavía no contiene notificaciones al controlador de dominio. A continuación, el controlador de dominio busca los tipos de notificación habilitados en AD DS e incluye las notificaciones resultantes en el vale Kerberos devuelto.

4.  Cuando el cliente de usuario \/ intenta obtener acceso a un recurso de archivo que se incluirse para requerir las notificaciones, puede tener acceso al recurso porque el identificador compuesto que se generó de Kerberos tiene estas notificaciones.

5.  Cuando el mismo cliente intenta obtener acceso a un sitio web o aplicación web que está configurado para la autenticación de AD FS, se redirige al usuario a un servidor de Federación de AD FS que está configurado para la autenticación integrada de Windows. El cliente envía una solicitud al controlador de dominio mediante Kerberos. El controlador de dominio emite un vale de Kerberos que contiene las notificaciones solicitadas que el cliente puede presentar en el servidor de Federación.

6.  En función de la forma en que se hayan configurado las reglas de notificaciones en el proveedor de notificaciones y las relaciones de confianza para usuario autenticado que el administrador haya configurado anteriormente, AD FS leer las notificaciones del vale de Kerberos e incluirlas en un token SAML que emite para el cliente.

7.  El cliente recibe el token SAML que contiene las notificaciones correctas y, a continuación, se redirige al sitio Web.

Para obtener más información sobre cómo crear las reglas de notificaciones necesarias para que AD DS las notificaciones emitidas funcionen con AD FS, consulte [creación de una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
