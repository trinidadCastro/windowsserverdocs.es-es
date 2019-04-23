---
title: Escenarios de inicio de sesión de Windows
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d6a1d52bee3c2d14ea83ccb794ecea1872868df5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828186"
---
# <a name="windows-logon-scenarios"></a>Escenarios de inicio de sesión de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema de referencia para profesionales de TI resume el inicio de sesión de Windows comunes y escenarios de inicio de sesión.

Los sistemas operativos de Windows requiere todos los usuarios para iniciar sesión en el equipo con una cuenta válida para el acceso local y los recursos de red. Los equipos basados en Windows proteger los recursos al implementar el proceso de inicio de sesión, en el que los usuarios se autentican. Cuando un usuario está autenticado, tecnologías de control de acceso y autorización implementan la segunda fase de protección de recursos: determinar si el usuario autenticado tiene autorización para acceder a un recurso.

El contenido de este tema se aplica a las versiones de Windows designados en el **se aplica a** lista al principio de este tema.

Además, las aplicaciones y servicios pueden requerir que los usuarios inicien sesión acceso a dichos recursos ofrecidos por la aplicación o servicio. El proceso de inicio de sesión es similar al proceso de inicio de sesión, en que se requiere una cuenta válida y las credenciales correctas, pero la información de inicio de sesión se almacena en la base de datos de administrador de cuentas de seguridad (SAM) en el equipo local y en Active Directory, si procede. Inicio de sesión de credencial de información de cuenta y se administra mediante la aplicación o servicio y, opcionalmente, puede almacenarse localmente en la caja.

Para entender cómo funciona la autenticación, vea [conceptos de autenticación de Windows](windows-authentication-concepts.md).

En este tema se describe los escenarios siguientes:

-   [Inicio de sesión interactivo](#BKMK_InteractiveLogon)

-   [Inicio de sesión de red](#BKMK_NetworkLogon)

-   [Inicio de sesión de tarjeta inteligente](#BKMK_SmartCardLogon)

-   [Inicio de sesión biométrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Inicio de sesión interactivo
El proceso de inicio de sesión comienza cuando un usuario escribe las credenciales en el cuadro de diálogo de entrada de credenciales, o cuando el usuario inserta una tarjeta inteligente en el lector de tarjeta inteligente, o cuando el usuario interactúa con un dispositivo biométrico. Los usuarios pueden realizar un inicio de sesión interactivo con una cuenta de usuario local o una cuenta de dominio para iniciar sesión en un equipo.

El siguiente diagrama muestra los elementos de inicio de sesión interactivo y el proceso de inicio de sesión.

![Diagrama que muestra los elementos de inicio de sesión interactivo y el proceso de inicio de sesión](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Arquitectura de autenticación de cliente de Windows**

### <a name="BKMK_LocaDomainLogon"></a>Inicio de sesión locales y de dominio
Credenciales que presenta al usuario para un inicio de sesión de dominio contienen todos los elementos necesarios para un inicio de sesión local, por ejemplo, el nombre de la cuenta y contraseña o certificado e información de dominio de Active Directory. El proceso confirma la identificación del usuario a la base de datos de seguridad en el equipo del usuario local o a un dominio de Active Directory. Este proceso de inicio de sesión obligatorio no puede desactivarse para los usuarios de un dominio.

Los usuarios pueden realizar un inicio de sesión interactivo en un equipo de dos maneras:

-   De forma local, cuando el usuario tiene acceso físico directo al equipo, o cuando el equipo forma parte de una red de equipos.

    Un inicio de sesión local concede permiso al usuario tener acceso a recursos de Windows en el equipo local. Un inicio de sesión local requiere que el usuario tiene una cuenta de usuario en el Administrador de cuentas de seguridad (SAM) en el equipo local. El SAM protege y administra información de usuario y grupo en forma de las cuentas de seguridad almacenadas en el registro del equipo local. El equipo puede tener acceso a la red, pero no es necesario. Información de pertenencia a grupos y las cuentas de usuario local se utiliza para administrar el acceso a los recursos locales.

    Un inicio de sesión concede permiso al usuario para acceder a recursos de Windows en el equipo local además de todos los recursos en los equipos conectados como se define en el token de acceso de la credencial. Un inicio de sesión local y un inicio de sesión de red requieren que el usuario tiene una cuenta de usuario en el Administrador de cuentas de seguridad (SAM) en el equipo local. Información de pertenencia a grupos y las cuentas de usuario local se usa para administrar el acceso a los recursos locales y el token de acceso para el usuario define qué recursos se pueden acceder desde equipos en red.

    Un inicio de sesión local y un inicio de sesión no son suficientes para conceder los permisos de usuario y equipo para tener acceso a y usar los recursos del dominio.

-   De forma remota, a través de Terminal Services o servicios de escritorio remoto (RDS), en cuyo caso el inicio de sesión es más calificado como remoto interactivo.

Después de un inicio de sesión interactivo, Windows ejecuta las aplicaciones en nombre del usuario y el usuario puede interactuar con esas aplicaciones.

Un inicio de sesión local concede permiso al usuario para acceder a los recursos en el equipo local o los recursos en los equipos conectados. Si el equipo está unido a un dominio, la funcionalidad de Winlogon intenta iniciar sesión en ese dominio.

Un inicio de sesión del dominio concede un permiso de usuario para tener acceso a local y los recursos del dominio. Un inicio de sesión de dominio requiere que el usuario tiene una cuenta de usuario en Active Directory. El equipo debe tener una cuenta de dominio de Active Directory y estar conectado físicamente a la red. Los usuarios también deben tener los derechos de usuario para iniciar sesión en un equipo local o un dominio. Información de cuenta de usuario de dominio y la información de pertenencia a grupos se utilizan para administrar el acceso a recursos locales y de dominio.

### <a name="BKMK_RemoteLogon"></a>Inicio de sesión remoto
En Windows, acceso a otro equipo a través de inicio de sesión remoto se basa en el protocolo de escritorio remoto (RDP). Dado que el usuario debe ya ha iniciado sesión correctamente en el equipo cliente antes de intentar una conexión remota, los procesos de inicio de sesión interactivo han finalizado correctamente.

RDP administra las credenciales que el usuario escribe mediante el cliente de escritorio remoto. Estas credenciales están pensadas para el equipo de destino, y el usuario debe tener una cuenta en ese equipo de destino. Además, el equipo de destino debe configurarse para aceptar una conexión remota. Las credenciales del equipo de destino se envían al intentar realizar el proceso de autenticación. Si la autenticación se realiza correctamente, el usuario se conecta a local y los recursos de red sean accesibles mediante el uso de las credenciales proporcionadas.

## <a name="BKMK_NetworkLogon"></a>Inicio de sesión de red
Un inicio de sesión sólo puede utilizarse después de que ha realizado la autenticación de usuario, servicio o equipo. Durante el inicio de sesión de red, el proceso no usa los cuadros de diálogo de entrada de credenciales para recopilar datos. En su lugar, se estableció previamente las credenciales o se usa otro método para recopilar las credenciales. Este proceso confirma la identidad del usuario en cualquier servicio de red que el usuario está intentando obtener acceso. Este proceso es suelen ser invisible para el usuario a menos que deben proporcionarse credenciales alternativas.

Para proporcionar este tipo de autenticación, el sistema de seguridad incluye estos mecanismos de autenticación:

-   Protocolo Kerberos versión 5

-   Certificados de clave pública

-   Secure Sockets Layer/Transport Layer Security (SSL/TLS)

-   Implícita

-   NTLM para la compatibilidad con los sistemas basados en Microsoft Windows NT 4.0

Para obtener información sobre los elementos y los procesos, consulte el diagrama de inicio de sesión interactivo anterior.

## <a name="BKMK_SmartCardLogon"></a>Inicio de sesión de tarjeta inteligente
Las tarjetas inteligentes puede utilizarse para iniciar sesión sólo en cuentas de dominio, las cuentas no locales. Autenticación mediante tarjeta inteligente requiere el uso del protocolo de autenticación Kerberos. Introdujo en Windows 2000 Server, en los sistemas operativos basados en Windows una extensión de clave pública para la solicitud de autenticación inicial del protocolo se implementa de Kerberos. A diferencia de criptografía de clave secreta compartida, la criptografía de clave pública es asimétrica, es decir, se necesitan dos claves diferentes: uno para cifrar, otra para descifrar. Juntas, las claves que son necesarias para realizar ambas operaciones conforman un par de claves pública y privada.

Para iniciar una sesión típica, un usuario debe demostrar su identidad proporcionando información conocida solo por el usuario y la infraestructura subyacente del protocolo Kerberos. La información secreta es una clave compartida criptográfica que se deriva de la contraseña del usuario. Una clave secreta compartida es simétrica, lo que significa que se utiliza la misma clave para el cifrado y descifrado.

El siguiente diagrama muestra los elementos y los procesos necesarios para el inicio de sesión de tarjeta inteligente.

![Diagrama que muestra los elementos y los procesos necesarios para el inicio de sesión de tarjeta inteligente](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Arquitectura de proveedor de credenciales de tarjeta inteligente**

Cuando se usa una tarjeta inteligente, en lugar de una contraseña, un par de claves pública/privada almacenado en la tarjeta inteligente de usuario se sustituye por la clave secreta compartida, que se deriva de la contraseña del usuario. La clave privada se almacena sólo en la tarjeta inteligente. La clave pública puede estar disponible para cualquier persona con quien el propietario desea intercambiar información confidencial.

Para obtener más información sobre el proceso de inicio de sesión de tarjeta inteligente en Windows, consulte [funcionamiento en el inicio de sesión de tarjeta inteligente en Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Inicio de sesión biométrico
Un dispositivo se usa para capturar y crear una característica de un artefacto, como una huella digital. Esta representación digital, a continuación, se compara con un ejemplo del mismo artefacto y, al comparan los dos correctamente, podrá realizar la autenticación. Los equipos que ejecutan cualquiera de los sistemas operativos designados en el **se aplica a** lista al principio de este tema puede configurarse para aceptar este formulario de inicio de sesión. Sin embargo, si el inicio de sesión biométrico solo está configurada para inicio de sesión local, el usuario debe presentar credenciales de dominio al obtener acceso a un dominio de Active Directory.

## <a name="additional-resources"></a>Recursos adicionales
Para obtener información acerca de cómo Windows administra las credenciales enviadas durante el proceso de inicio de sesión, vea [administración de credenciales en la autenticación de Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Información técnica sobre inicio de sesión de Windows y autenticación](https://technet.microsoft.com/library/dn169029.aspx)


