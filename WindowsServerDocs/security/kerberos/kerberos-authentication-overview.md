---
title: Kerberos Authentication Overview
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-kerberos
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6b5ec9bfa5c17a9ee9a5ad15af183d25bd533d7e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856328"
---
# <a name="kerberos-authentication-overview"></a>Kerberos Authentication Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Kerberos es un protocolo de autenticación que se usa para comprobar la identidad de un usuario o host. Este tema contiene información sobre la autenticación Kerberos en Windows Server 2012 y Windows 8.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
Los sistemas operativos Windows Server implementan las extensiones y el protocolo de autenticación de la versión 5 de Kerberos para la autenticación de clave pública, el transporte de datos de autorización y la delegación. El cliente de autenticación Kerberos se implementa como un proveedor de compatibilidad para seguridad \(SSP\)y se puede obtener acceso a él a través de la interfaz del proveedor de compatibilidad de seguridad \(\)SSPI. La autenticación inicial de usuario se integra con el inicio de sesión único de Winlogon\-en la arquitectura.

El Centro de distribución de claves Kerberos \(KDC\) está integrado con otros servicios de seguridad de Windows Server que se ejecutan en el controlador de dominio. El KDC usa la base de datos de Active Directory Domain Services del dominio como base de datos de cuentas de seguridad. Active Directory Domain Services es necesario para las implementaciones predeterminadas de Kerberos dentro del dominio o bosque.

## <a name="practical-applications"></a><a name="kerb_tr_Kerb_Benefits"></a>Aplicaciones prácticas
Las ventajas que se obtienen al usar Kerberos para la autenticación basada en\-de dominio son:

-   **Autenticación delegada.**

    Los servicios que se ejecutan en sistemas operativos Windows pueden suplantar a un equipo cliente al obtener acceso a recursos en nombre del cliente. En muchos casos, un servicio puede completar su trabajo en beneficio del cliente al obtener acceso a recursos en el equipo local. Cuando un equipo cliente se autentica en el servicio, los protocolos NTLM y Kerberos proporcionan la información de autorización que un servicio necesita para suplantar el equipo cliente localmente. Sin embargo, algunas aplicaciones distribuidas están diseñadas de modo que un servicio Front\-end debe usar la identidad del equipo cliente cuando se conecta a servicios de back\-end en otros equipos. La autenticación Kerberos admite un mecanismo de delegación que permite que un servicio actúe en nombre de su cliente al conectarse con otros servicios.

-   **Inicio de sesión único.**

    Con la autenticación Kerberos dentro de un dominio o en un bosque, el usuario o servicio puede tener acceso a los recursos permitidos por los administradores sin varias solicitudes de credenciales. Después del inicio de sesión inicial del dominio a través de Winlogon, Kerberos administra las credenciales en todo el bosque cuando se intenta tener acceso a los recursos.

-   **Interoperabilidad.**

    La implementación del protocolo Kerberos V5 por parte de Microsoft se basa en los estándares\-realizar un seguimiento de las especificaciones recomendadas para el equipo de ingeniería de Internet \(IETF\). En consecuencia, en los sistemas operativos Windows, el protocolo Kerberos sienta las bases para la interoperabilidad con otras redes en las que el protocolo Kerberos se usa para la autenticación. Además, Microsoft publica documentación de Protocolos de Windows para implementar el protocolo Kerberos. La documentación contiene los requisitos técnicos, las limitaciones, las dependencias y el comportamiento del protocolo específico de Windows\-para la implementación de Microsoft del protocolo Kerberos.

-   **Autenticación más eficaz para los servidores.**

    Antes de Kerberos, se podía usar la autenticación NTLM, que necesita que un servidor de aplicaciones se conecte a un controlador de dominio para autenticar cada equipo cliente o servicio. Con el protocolo Kerberos, los vales de sesión renovables reemplazan el paso\-a través de la autenticación. No es necesario que el servidor vaya a un controlador de dominio \(a menos que necesite validar un certificado de atributo de privilegio \(PAC\)\). En lugar de ello, el servidor puede autenticar al equipo cliente examinando las credenciales que presenta el cliente. Los equipos cliente pueden obtener credenciales para un servidor en particular una vez y, luego, volver a usar esas credenciales durante una sesión de inicio de sesión de red.

-   **Autenticación mutua.**

    Al usar el protocolo Kerberos, una entidad que se encuentra en cualquiera de los dos extremos de una conexión de red puede comprobar que la otra entidad sea quien dice ser. NTLM no permite a los clientes comprobar la identidad de un servidor ni habilitar un servidor para comprobar la identidad de otro. La autenticación NTLM se diseñó para un entorno de red en el que se suponía que los servidores eran auténticos. El protocolo Kerberos no realiza esa suposición.

## <a name="see-also"></a>Consulta también
[Información general de la autenticación de Windows](../windows-authentication/windows-authentication-overview.md)


