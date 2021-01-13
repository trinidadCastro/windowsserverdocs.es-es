---
title: Escenarios de inicio de sesión de Windows
description: Obtenga información sobre los escenarios comunes de inicio de sesión e inicio de sesión de Windows.
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 34d1d70c88395e109a45a1f4a57d2fafb09c07ce
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177554"
---
# <a name="windows-logon-scenarios"></a>Escenarios de inicio de sesión de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de referencia para profesionales de TI se resumen los escenarios comunes de inicio de sesión e inicio de sesión de Windows.

Los sistemas operativos Windows requieren que todos los usuarios inicien sesión en el equipo con una cuenta válida para tener acceso a los recursos locales y de red. Los equipos basados en Windows protegen los recursos implementando el proceso de inicio de sesión, en el que se autentican los usuarios. Una vez autenticado un usuario, las tecnologías de autorización y control de acceso implementan la segunda fase de protección de recursos: determinar si el usuario autenticado está autorizado para tener acceso a un recurso.

El contenido de este tema se aplica a las versiones de Windows designadas en la lista **se aplica a que se** encuentra al principio de este tema.

Además, las aplicaciones y los servicios pueden requerir a los usuarios que inicien sesión para tener acceso a los recursos que ofrece la aplicación o el servicio. El proceso de inicio de sesión es similar al proceso de inicio de sesión, en el que se requieren una cuenta válida y las credenciales correctas, pero la información de inicio de sesión se almacena en la base de datos del administrador de cuentas de seguridad (SAM) en el equipo local y en Active Directory cuando proceda. La aplicación o el servicio administra la información de las credenciales y la cuenta de inicio de sesión y, opcionalmente, se puede almacenar localmente en el bloqueo de credenciales.

Para entender cómo funciona la autenticación, consulte [conceptos de autenticación de Windows](windows-authentication-concepts.md).

En este tema se describen los siguientes escenarios:

-   [Inicio de sesión interactivo](#BKMK_InteractiveLogon)

-   [Inicio de sesión de red](#BKMK_NetworkLogon)

-   [Inicio de sesión con tarjeta inteligente](#BKMK_SmartCardLogon)

-   [Inicio de sesión biométrico](#BKMK_BioLogon)

## <a name="interactive-logon"></a><a name="BKMK_InteractiveLogon"></a>Inicio de sesión interactivo
El proceso de inicio de sesión comienza cuando un usuario escribe las credenciales en el cuadro de diálogo entrada de credenciales, o cuando el usuario inserta una tarjeta inteligente en el lector de tarjeta inteligente, o cuando el usuario interactúa con un dispositivo biométrico. Los usuarios pueden realizar un inicio de sesión interactivo mediante una cuenta de usuario local o una cuenta de dominio para iniciar sesión en un equipo.

El siguiente diagrama muestra los elementos de inicio de sesión interactivos y el proceso de inicio de sesión.

![Diagrama que muestra los elementos de inicio de sesión interactivo y el proceso de inicio de sesión](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows Client Authentication Architecture**

### <a name="local-and-domain-logon"></a><a name="BKMK_LocaDomainLogon"></a>Inicio de sesión local y de dominio
Las credenciales que el usuario presenta para un inicio de sesión de dominio contienen todos los elementos necesarios para un inicio de sesión local, como el nombre de cuenta, la contraseña o el certificado, y Active Directory información del dominio. El proceso confirma la identificación del usuario en la base de datos de seguridad del equipo local del usuario o en un dominio de Active Directory. Este proceso de inicio de sesión obligatorio no se puede desactivar para los usuarios de un dominio.

Los usuarios pueden realizar un inicio de sesión interactivo en un equipo de dos maneras:

-   Localmente, cuando el usuario tiene acceso físico directo al equipo o cuando el equipo forma parte de una red de equipos.

    Un inicio de sesión local concede a un usuario permiso para obtener acceso a los recursos de Windows en el equipo local. Un inicio de sesión local requiere que el usuario tenga una cuenta de usuario en el administrador de cuentas de seguridad (SAM) del equipo local. El SAM protege y administra la información de usuarios y grupos en forma de cuentas de seguridad almacenadas en el registro del equipo local. El equipo puede tener acceso a la red, pero no es necesario. La información de la cuenta de usuario local y la pertenencia a grupos se utiliza para administrar el acceso a los recursos locales.

    Un inicio de sesión de red concede a un usuario permiso para obtener acceso a los recursos de Windows en el equipo local, además de los recursos de los equipos conectados en red, tal como se define en el token de acceso de la credencial. Tanto un inicio de sesión local como un inicio de sesión de red requieren que el usuario tenga una cuenta de usuario en el administrador de cuentas de seguridad (SAM) del equipo local. La información de la cuenta de usuario local y la pertenencia a grupos se utiliza para administrar el acceso a los recursos locales y el token de acceso para el usuario define a qué recursos se puede tener acceso en los equipos conectados en red.

    Un inicio de sesión local y un inicio de sesión de red no son suficientes para conceder permiso al usuario y al equipo para el acceso y el uso de los recursos del dominio.

-   De forma remota, a través de Terminal Services o Servicios de Escritorio remoto (RDS), en cuyo caso el inicio de sesión se califica mejor como remoto interactivo.

Después de un inicio de sesión interactivo, Windows ejecuta aplicaciones en nombre del usuario y el usuario puede interactuar con esas aplicaciones.

Un inicio de sesión local concede a un usuario permiso para obtener acceso a los recursos del equipo local o los recursos de los equipos conectados en red. Si el equipo está unido a un dominio, la funcionalidad de winlogon intenta iniciar sesión en ese dominio.

Un inicio de sesión de dominio concede a un usuario permiso para obtener acceso a recursos locales y de dominio. Un inicio de sesión de dominio requiere que el usuario tenga una cuenta de usuario en Active Directory. El equipo debe tener una cuenta en el dominio de Active Directory y estar físicamente conectado a la red. Los usuarios también deben tener derechos de usuario para iniciar sesión en un equipo local o en un dominio. La información de la cuenta de usuario de dominio y la información de pertenencia a grupos se utilizan para administrar el acceso a los recursos locales y de dominio.

### <a name="remote-logon"></a><a name="BKMK_RemoteLogon"></a>Inicio de sesión remoto
En Windows, el acceso a otro equipo a través del inicio de sesión remoto se basa en el Protocolo de escritorio remoto (RDP). Debido a que el usuario ya debe haber iniciado sesión correctamente en el equipo cliente antes de intentar una conexión remota, los procesos de inicio de sesión interactivos han finalizado correctamente.

RDP administra las credenciales que el usuario especifica mediante el Escritorio remoto cliente. Dichas credenciales están pensadas para el equipo de destino y el usuario debe tener una cuenta en ese equipo de destino. Además, el equipo de destino debe estar configurado para aceptar una conexión remota. Las credenciales del equipo de destino se envían para intentar realizar el proceso de autenticación. Si la autenticación es correcta, el usuario se conecta a recursos locales y de red a los que se puede tener acceso mediante las credenciales proporcionadas.

## <a name="network-logon"></a><a name="BKMK_NetworkLogon"></a>Inicio de sesión de red
Solo se puede usar un inicio de sesión de red después de que se haya realizado la autenticación del usuario, el servicio o el equipo. Durante el inicio de sesión de red, el proceso no utiliza los cuadros de diálogo de entrada de credenciales para recopilar datos. En su lugar, se usan credenciales establecidas previamente u otro método para recopilar credenciales. Este proceso confirma la identidad del usuario para cualquier servicio de red al que el usuario está intentando obtener acceso. Este proceso suele ser invisible para el usuario a menos que se deban proporcionar credenciales alternativas.

Para proporcionar este tipo de autenticación, el sistema de seguridad incluye estos mecanismos de autenticación:

-   Protocolo Kerberos versión 5

-   Certificados de clave pública

-   Capa de sockets seguros/Seguridad de la capa de transporte (SSL/TLS)

-   Digest

-   NTLM, para la compatibilidad con sistemas basados en Microsoft Windows NT 4,0

Para obtener información sobre los elementos y los procesos, consulte el diagrama de inicio de sesión interactivo anterior.

## <a name="smart-card-logon"></a><a name="BKMK_SmartCardLogon"></a>Inicio de sesión con tarjeta inteligente
Las tarjetas inteligentes solo pueden usarse para iniciar sesión en cuentas de dominio, no en cuentas locales. La autenticación mediante tarjeta inteligente requiere el uso del Protocolo de autenticación Kerberos. Introducido en el servidor de Windows 2000, en los sistemas operativos basados en Windows, se implementa una extensión de clave pública para la solicitud de autenticación inicial del protocolo Kerberos. A diferencia de la criptografía de clave secreta compartida, la criptografía de clave pública es asimétrica, es decir, se necesitan dos claves diferentes: una para cifrar, otra para descifrar. Juntas, las claves necesarias para realizar ambas operaciones componen un par de claves privada y pública.

Para iniciar una sesión de inicio de sesión típica, el usuario debe demostrar su identidad proporcionando información que solo el usuario y la infraestructura de protocolo Kerberos subyacente conocen. La información secreta es una clave compartida criptográfica derivada de la contraseña del usuario. Una clave secreta compartida es simétrica, lo que significa que se utiliza la misma clave para el cifrado y el descifrado.

En el diagrama siguiente se muestran los elementos y procesos necesarios para el inicio de sesión con tarjeta inteligente.

![Diagrama que muestra los elementos y procesos necesarios para el inicio de sesión con tarjeta inteligente](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Arquitectura del proveedor de credenciales de tarjetas inteligentes**

Cuando se usa una tarjeta inteligente en lugar de una contraseña, un par de claves privada y pública almacenado en la tarjeta inteligente del usuario se sustituye por la clave secreta compartida, que se deriva de la contraseña del usuario. La clave privada solo se almacena en la tarjeta inteligente. La clave pública puede ponerse a disposición de cualquier persona con la que el propietario quiera intercambiar información confidencial.

Para obtener más información acerca del proceso de inicio de sesión con tarjeta inteligente en Windows, consulte Cómo funciona el inicio de [sesión con tarjeta inteligente en Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff404285(v=ws.10)).

## <a name="biometric-logon"></a><a name="BKMK_BioLogon"></a>Inicio de sesión biométrico
Un dispositivo se usa para capturar y compilar una característica digital de un artefacto, como una huella digital. Esta representación digital se compara a continuación con una muestra del mismo artefacto y, cuando las dos se comparan correctamente, se puede producir la autenticación. Los equipos que ejecutan cualquiera de los sistemas operativos designados en la lista **se aplica a** al principio de este tema se pueden configurar para aceptar esta forma de inicio de sesión. Sin embargo, si el inicio de sesión biométrico solo está configurado para el inicio de sesión local, el usuario debe presentar credenciales de dominio al obtener acceso a un dominio de Active Directory.

## <a name="additional-resources"></a>Recursos adicionales
Para obtener información acerca de cómo Windows administra las credenciales enviadas durante el proceso de inicio de sesión, vea [Administración de credenciales en la autenticación de Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169014(v=ws.10)).

[Información técnica sobre inicio de sesión y autenticación en Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169029(v=ws.10))