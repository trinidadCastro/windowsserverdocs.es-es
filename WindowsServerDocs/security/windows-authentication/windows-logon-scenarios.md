---
title: "Escenarios de inicio de sesión de Windows"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="windows-logon-scenarios"></a>Escenarios de inicio de sesión de Windows

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de referencia para profesionales de TI resume inicio de sesión de Windows comunes y escenarios de inicio de sesión.

Los sistemas operativos Windows requiere todos los usuarios iniciar sesión en el equipo con una cuenta válida para el acceso local y recursos de red. Equipos basados en Windows proteger los recursos mediante la implementación el proceso de inicio de sesión, en el que los usuarios se autentican. Después de que un usuario se autentica, las tecnologías de control de acceso y autorización implementan la segunda fase de proteger los recursos: determinar si el usuario autenticado está autorizado para acceder a un recurso.

El contenido de este tema se aplica a las versiones de Windows que se enumeran en la **se aplica a** lista al principio de este tema.

Además, aplicaciones y servicios pueden requerir que los usuarios se registren para acceder a los recursos que se ofrecen por la aplicación o servicio. El proceso de inicio de sesión es similar al proceso de inicio de sesión, que se requiere una cuenta válida y las credenciales correctas, pero la información de inicio de sesión se almacena en la base de datos del Administrador de cuentas de seguridad (SAM) en el equipo local y en Active Directory cuando corresponda. Inicio de sesión credenciales información de cuenta y está administrada por la aplicación o servicio y, opcionalmente, que se puede almacenar localmente en la caja de seguridad.

Para comprender cómo funciona la autenticación, vea [conceptos de autenticación de Windows](windows-authentication-concepts.md).

Este tema describen los siguientes escenarios:

-   [Inicio de sesión interactivo](#BKMK_InteractiveLogon)

-   [Inicio de sesión de red](#BKMK_NetworkLogon)

-   [Inicio de sesión de tarjeta inteligente](#BKMK_SmartCardLogon)

-   [Inicio de sesión biométrico](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Inicio de sesión interactivo
Cuando un usuario escribe las credenciales en el cuadro de diálogo de entrada de credenciales, o cuando el usuario inserte una tarjeta inteligente en el lector de tarjeta inteligente, o cuando el usuario interactúa con un dispositivo biométrico, se inicia el proceso de inicio de sesión. Los usuarios pueden realizar un inicio de sesión interactivo mediante una cuenta de usuario local o una cuenta de dominio para iniciar sesión en un equipo.

El siguiente diagrama muestra los elementos de inicio de sesión interactivo y el proceso de inicio de sesión.

![Diagrama que muestra los elementos de inicio de sesión interactivo y el proceso de inicio de sesión](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Arquitectura de autenticación de cliente de Windows**

### <a name="BKMK_LocaDomainLogon"></a>Inicio de sesión local y de dominio
Las credenciales que se presenta al usuario para un inicio de sesión de dominio contengan todos los elementos necesarios para un inicio de sesión local, como el nombre de cuenta y contraseña o certificado e información de dominio de Active Directory. El proceso confirma la identificación del usuario para la base de datos de seguridad en el equipo del usuario local o a un dominio de Active Directory. Este proceso de inicio de sesión obligatorios no se puede desactivar para los usuarios en un dominio.

Los usuarios pueden realizar un inicio de sesión interactivo en un equipo de dos maneras:

-   Localmente, cuando el usuario tiene acceso físico directo en el equipo, o cuando el equipo forma parte de una red de equipos.

    Un inicio de sesión local concede permisos de usuario para acceder a recursos de Windows en el equipo local. Un inicio de sesión local requiere que el usuario tenga una cuenta de usuario en el Administrador de cuentas de seguridad (SAM) en el equipo local. SAM protege y administra la información de usuario y grupo en el formulario de cuentas de seguridad que se almacena en el registro del equipo local. El equipo puede tener acceso a la red, pero no es necesario. Información de pertenencia a grupos y cuentas de usuario local se usa para administrar el acceso a los recursos locales.

    Un inicio de sesión de red concede permisos de usuario para acceder a recursos de Windows en el equipo local, además de los recursos en equipos de red como se define en el token de acceso de la credencial. Un inicio de sesión local y un inicio de sesión de red requieren que el usuario tenga una cuenta de usuario en el Administrador de cuentas de seguridad (SAM) en el equipo local. Información de pertenencia a grupos y cuentas de usuario local se usa para administrar el acceso a los recursos locales y el token de acceso para que el usuario define los recursos que se puede acceder en equipos de red.

    Un inicio de sesión local y un inicio de sesión de red no son suficientes para conceder el permiso del usuario y del equipo para tener acceso y usar recursos de dominio.

-   De forma remota, a través de servicios de Terminal Server o servicios de escritorio remoto (RDS), en cuyo caso el inicio de sesión es más completo como remoto interactivo.

Después de un inicio de sesión interactivo, Windows ejecuta las aplicaciones en nombre del usuario y el usuario puede interactuar con esas aplicaciones.

Un inicio de sesión local concede permisos de usuario para acceder a los recursos en el equipo local o recursos de equipos en red. Si el equipo está unido a un dominio, la funcionalidad de Winlogon intenta iniciar sesión en ese dominio.

Un inicio de sesión de dominio concede permisos de usuario para tener acceso local y los recursos de dominio. Un inicio de sesión de dominio requiere que el usuario tenga una cuenta de usuario en Active Directory. El equipo debe tener una cuenta en el dominio de Active Directory y estar conectado físicamente a la red. Los usuarios también deben tener los derechos de usuario para iniciar sesión en un equipo local o en un dominio. Información de cuenta de usuario de dominio y la información de pertenencia a grupos se usan para administrar el acceso a recursos locales y de dominio.

### <a name="BKMK_RemoteLogon"></a>Inicio de sesión remoto
En Windows, acceso a otro equipo a través de inicio de sesión remoto se basa en el protocolo de escritorio remoto (RDP). Dado que el usuario debe ya haya iniciado sesión correctamente el equipo cliente antes de intentar una conexión remota, los procesos de inicio de sesión interactivo han terminado correctamente.

RDP administra las credenciales que el usuario especifica mediante el cliente de escritorio remoto. Las credenciales están destinadas para el equipo de destino, y el usuario debe tener una cuenta en dicho equipo de destino. Además, el equipo de destino debe configurarse para que acepte una conexión remota. Las credenciales del equipo de destino se envían al tratar de realizar el proceso de autenticación. Si la autenticación es correcta, el usuario está conectado a local y recursos de red que no son accesibles mediante las credenciales proporcionadas.

## <a name="BKMK_NetworkLogon"></a>Inicio de sesión de red
Un inicio de sesión de red solo puede usarse después de que realizó la autenticación de usuario, servicio o equipo. Durante el inicio de sesión de red, el proceso no usa los cuadros de diálogo de la entrada de credenciales para recopilar datos. En su lugar, establecido anteriormente credenciales o se utiliza otro método para recopilar las credenciales. Este proceso confirma la identidad del usuario a cualquier servicio de red que el usuario está intentando acceder. Este proceso generalmente es invisible al usuario a menos que deben proporcionarse credenciales alternativas.

Para proporcionar este tipo de autenticación, el sistema de seguridad incluye estos mecanismos de autenticación:

-   Protocolo de la versión 5 de Kerberos

-   Certificados de clave pública

-   Proteger la seguridad de capa de transporte/capa de Sockets (SSL/TLS)

-   Resumen

-   NTLM para la compatibilidad con sistemas basados en Microsoft Windows NT 4.0

Para obtener información sobre los elementos y los procesos, consulta el diagrama de inicio de sesión interactivo anterior.

## <a name="BKMK_SmartCardLogon"></a>Inicio de sesión de tarjeta inteligente
Para iniciar sesión solo para cuentas de dominio, las cuentas locales no se pueden usar tarjetas inteligentes. Autenticación con tarjeta inteligente requiere el uso del protocolo de autenticación Kerberos. Presentó en Windows 2000 Server, en sistemas operativos basados en Windows una extensión de clave pública a se implementa la solicitud de autenticación inicial del protocolo Kerberos. A diferencia de criptografía de clave secreta compartida, criptografía de clave pública es asimétrica, es decir, se necesitan dos claves diferentes: uno para cifrar, otra para descifrar. Juntos, las claves que sean necesarias para realizar ambas operaciones forman un par de claves pública y privada.

Para iniciar una sesión de inicio normal, un usuario deberá demostrar su identidad proporcionando información conocida solo al usuario y la infraestructura de protocolo Kerberos subyacente. La información secreta es una clave compartida criptográfica derivada de la contraseña del usuario. Una clave secreta compartida es el cifrado simétrica, lo que significa que se usa la misma clave para el cifrado y descifrado.

El siguiente diagrama muestra los elementos y los procesos necesarios para el inicio de sesión de tarjeta inteligente.

![Diagrama que muestra los elementos y los procesos necesarios para el inicio de sesión de tarjeta inteligente](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Arquitectura de proveedor de credenciales de tarjeta inteligente**

Cuando se usa una tarjeta inteligente en lugar de una contraseña, un par de claves pública y privada almacenado en la tarjeta inteligente se sustituye por la clave secreta compartida, que se deriva de la contraseña del usuario. La clave privada se almacena únicamente en la tarjeta inteligente. La clave pública puede estarán disponible para cualquier persona con la que el propietario quiere intercambiar información confidencial.

Para obtener más información sobre el proceso de inicio de sesión de tarjeta inteligente en Windows, consulta [funcionamiento en el inicio de sesión de tarjeta inteligente en Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Inicio de sesión biométrico
Un dispositivo se usa para capturar y generar una característica de un artefacto, como una huella digital. Esta representación digital, a continuación, se compara con una muestra de la misma artefacto y cuando correctamente se comparan los dos, podrá realizar la autenticación. Equipos que ejecutan cualquiera de los sistemas operativos que se indican en la **se aplica a** lista al principio de este tema puede configurarse para aceptar este formulario de inicio de sesión. Sin embargo, si el inicio de sesión biométrico solo está configurado para el inicio de sesión local, el usuario necesita presentar las credenciales de dominio al obtener acceso a un dominio de Active Directory.

## <a name="additional-resources"></a>Recursos adicionales
Para obtener información sobre cómo Windows administra credenciales enviadas durante el proceso de inicio de sesión, consulta [administración de las credenciales de autenticación de Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Información general técnica de inicio de sesión de Windows y autenticación](https://technet.microsoft.com/library/dn169029.aspx)


